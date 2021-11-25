---
title: element翻页记忆
date: 2019-04-05 20:04:02
tags:
---

![](/images/fenye.jpg)

记忆翻页新需求，留作回顾。
记录当前选中的分页中的列表，分页保存数据，然后在用选中的数据进行某些操作.


### 效果展示
![](/images/table.png)

### HTML
``` bash
<!--table组件需要使用ref="table"-->
<template>
    <div>
        <el-table :data="tableData" ref="table" stripe style="width: 100%" @selection-change="handleSelectionChange">
            <el-table-column type="selection" width="55"></el-table-column>
            <el-table-column prop="name" label="商品名称"></el-table-column>
            <el-table-column prop="barcode" label="商品编码"></el-table-column>
            <el-table-column prop="quantity" label="区域总库存"></el-table-column>
        </el-table>
        <div class="block" v-show="page.pageTotal>10">
            <el-pagination @current-change="handleCurrentChange" :current-page.sync="page.currentPage" :page-size="page.pnum" layout="total, prev, pager, next , jumper" :total="page.pageTotal">
            </el-pagination>
        </div>
        <div>
            {{multipleSelectionAll.length}}
        </div>
    </div>
</template>
```

### JS
``` bash
export default {
    data(){
        return {
            tableData: [], // 表格数据 
            multipleSelectionAll:[],//所有选中的数据包含跨页数据
            multipleSelection:[],// 当前页选中的数据
            idKey: 'barcode', // 标识列表数据中每一行的唯一键的名称
            page:{
                //每页数据量
                pnum:10,
                //数据总数
                pageTotal:0,
                //当前页，从1开始
                currentPage:1,
            }
        }
    },
    methods: {
        handleCurrentChange(){
            this.changePageCoreRecordData();
            if(!this.$isNull(this.page.pageTotal)) this.getGoodsList();
        },
        handleSelectionChange (val) {
            this.multipleSelection = val;
            //实时记录选中的数据
            setTimeout(()=>{
                this.changePageCoreRecordData();
            }, 50)
        },
        setSelectRow() {
            if (!this.multipleSelectionAll || this.multipleSelectionAll.length <= 0) {
                return;
            }
            // 标识当前行的唯一键的名称
            let idKey = this.idKey;
            let selectAllIds = [];
            let that = this;
            this.multipleSelectionAll.forEach(row=>{
                selectAllIds.push(row[idKey]);
            })
            this.$refs.table.clearSelection();
            for(var i = 0; i < this.tableData.length; i++) {                    
                if (selectAllIds.indexOf(this.tableData[i][idKey]) >= 0) {
                    // 设置选中，记住table组件需要使用ref="table"
                    this.$refs.table.toggleRowSelection(this.tableData[i], true);
                }
            }
        },
        // 记忆选择核心方法
        changePageCoreRecordData () {
            // 标识当前行的唯一键的名称
            let idKey = this.idKey;
            let that = this;
            // 如果总记忆中还没有选择的数据，那么就直接取当前页选中的数据，不需要后面一系列计算
            if (this.multipleSelectionAll.length <= 0) {
                this.multipleSelectionAll = this.multipleSelection;
                return;
            }
            // 总选择里面的key集合
            let selectAllIds = [];
            this.multipleSelectionAll.forEach(row=>{
                selectAllIds.push(row[idKey]);
            })
            let selectIds = []
            // 获取当前页选中的id
            this.multipleSelection.forEach(row=>{
                selectIds.push(row[idKey]);
                // 如果总选择里面不包含当前页选中的数据，那么就加入到总选择集合里
                if (selectAllIds.indexOf(row[idKey]) < 0) {
                    that.multipleSelectionAll.push(row);
                }
            })
            let noSelectIds = [];
            // 得到当前页没有选中的id
            this.tableData.forEach(row=>{
                if (selectIds.indexOf(row[idKey]) < 0) {
                    noSelectIds.push(row[idKey]);
                }
            })
            noSelectIds.forEach(id=>{
                if (selectAllIds.indexOf(id) >= 0) {
                    for(let i = 0; i< that.multipleSelectionAll.length; i ++) {
                        if (that.multipleSelectionAll[i][idKey] == id) {
                            // 如果总选择中有未被选中的，那么就删除这条
                            that.multipleSelectionAll.splice(i, 1);
                            break;
                        }
                    }
                }
            })
        },
        //请求接口部分
        getGoodsList(){
            let data = {};
            data['page'] = this.page.currentPage;
            data['pnum'] = this.page.pnum;
            this.$ajax({
                url:'goods/list',
                data:data
            }).then(val => {
                this.tableData = val.data.rows ? val.data.rows : [];
                this.page = {
                    pnum:10,
                    pageTotal:val.data.total,
                    currentPage:val.data.page,
                };
                setTimeout(()=>{
                    this.setSelectRow();
                }, 50)
            })
        }
    },
    created () {
        this.getGoodsList()
    }
}
```