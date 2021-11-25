---
title: 验证码功能
date: 2018-11-09 19:38:20
tags:
---

![](/images/yanzheng.jpg)

登陆验证样式。

## 数字验证码

### 效果
![](/images/num.png)

## 代码示例

### HTML
``` bash
<input type="text" name="phone" id="phone" value="" placeholder="请输入手机号" maxlength="11" />
<input type="" name="verCode" id="verCode" value="" placeholder="请输入验证码" maxlength="6"/>
<input type="button" name="" id="verCodeBtn" value="获取验证码"  onclick="settime(this);"/>
```
### JS
``` bash
//验证码
var counts = 60;

function setTime(val) {
    if(counts == 0) {
        val.removeAttribute("disabled");
        val.value = "获取验证码";
        counts = 60;
        return false;
    } else {
        val.setAttribute("disabled", true);
        val.value = "重新发送（" + counts + "）";
        counts--;
    }
    setTimeout(function() {
        setTime(val);
    }, 1000);
}
```

### 请求接口
``` bash
$(function(){
   //获取验证码
    $("#verCodeBtn").click(function() {
        $.ajax({
            url: "",
            data: {},
            type: "post",
            success: function(data) {
                if(JSON.parse(data).state === 404 || JSON.parse(data).state === 202) {
                    alert("验证码发送失败")
                } else {
                    alert("验证码发送成功，请耐心等待")
                }
            },
            error: function() {
                alert("发送失败");
            }
        });

    });
})
```

## 图形验证码

### 效果
![](/images/tuxing.png)

## 代码示例

### HTML
``` bash
<div class="code">
    <input type="text" value="" placeholder="请输入验证码（不区分大小写）" class="input-val">
    <canvas id="canvas" width="100" height="30"></canvas>
    <button class="btn">提交</button>
</div>
```

### CSS
``` bash
<style>
    .input-val {
    width: 200px;
    height: 32px;
    border: 1px solid #ddd;
    box-sizing: border-box;
    }
    #canvas {
    vertical-align: middle;
    box-sizing: border-box;
    border: 1px solid #ddd;
    cursor: pointer;
    }
    .btn {
    display: block;
    margin-top: 20px;
    height: 32px;
    width: 100px;
    font-size: 16px;
    color: #fff;
    background-color: #457adb;
    border: none;
    border-radius: 50px;
    }
</style>
```

### JS
``` bash
<script type="text/javascript" src="js/jquery-3.3.1.min.js" ></script>
<script>
    $(function(){
        var show_num = [];
        draw(show_num);

        $("#canvas").on('click',function(){
            draw(show_num);
        })
        $(".btn").on('click',function(){
            var val = $(".input-val").val().toLowerCase();
            var num = show_num.join("");
            if(val==''){
                alert('请输入验证码！');
            }else if(val == num){
                alert('提交成功！');
                $(".input-val").val('');
                // draw(show_num);

            }else{
                alert('验证码错误！请重新输入！');
                $(".input-val").val('');
                // draw(show_num);
            }
        })
    })

    //生成并渲染出验证码图形
    function draw(show_num) {
        var canvas_width=$('#canvas').width();
        var canvas_height=$('#canvas').height();
        var canvas = document.getElementById("canvas");//获取到canvas的对象，演员
        var context = canvas.getContext("2d");//获取到canvas画图的环境，演员表演的舞台
        canvas.width = canvas_width;
        canvas.height = canvas_height;
        var sCode = "a,b,c,d,e,f,g,h,i,j,k,m,n,p,q,r,s,t,u,v,w,x,y,z,A,B,C,E,F,G,H,J,K,L,M,N,P,Q,R,S,T,W,X,Y,Z,1,2,3,4,5,6,7,8,9,0";
        var aCode = sCode.split(",");
        var aLength = aCode.length;//获取到数组的长度
        
        for (var i = 0; i < 4; i++) {  //这里的for循环可以控制验证码位数（如果想显示6位数，4改成6即可）
            var j = Math.floor(Math.random() * aLength);//获取到随机的索引值
            // var deg = Math.random() * 30 * Math.PI / 180;//产生0~30之间的随机弧度
            var deg = Math.random() - 0.5; //产生一个随机弧度
            var txt = aCode[j];//得到随机的一个内容
            show_num[i] = txt.toLowerCase();
            var x = 10 + i * 20;//文字在canvas上的x坐标
            var y = 20 + Math.random() * 8;//文字在canvas上的y坐标
            context.font = "bold 23px 微软雅黑";

            context.translate(x, y);
            context.rotate(deg);

            context.fillStyle = randomColor();
            context.fillText(txt, 0, 0);

            context.rotate(-deg);
            context.translate(-x, -y);
        }
        for (var i = 0; i <= 5; i++) { //验证码上显示线条
            context.strokeStyle = randomColor();
            context.beginPath();
            context.moveTo(Math.random() * canvas_width, Math.random() * canvas_height);
            context.lineTo(Math.random() * canvas_width, Math.random() * canvas_height);
            context.stroke();
        }
        for (var i = 0; i <= 30; i++) { //验证码上显示小点
            context.strokeStyle = randomColor();
            context.beginPath();
            var x = Math.random() * canvas_width;
            var y = Math.random() * canvas_height;
            context.moveTo(x, y);
            context.lineTo(x + 1, y + 1);
            context.stroke();
        }
    }

    //得到随机的颜色值
    function randomColor() {
        var r = Math.floor(Math.random() * 256);
        var g = Math.floor(Math.random() * 256);
        var b = Math.floor(Math.random() * 256);
        return "rgb(" + r + "," + g + "," + b + ")";
    }

</script>
```

## 滑动验证码

### 效果
![](/images/huadong.png)

## 代码示例

### HTML
``` bash
<div class="drag">
    <div class="bg"></div>
    <div class="text" onselectstart="return false;">请拖动滑块解锁</div>
    <div class="btn">&gt;&gt;</div>
</div>
```

### CSS
``` bash
<style>
    .drag{
        width: 300px;
        height: 40px;
        line-height: 40px;
        background-color: #e8e8e8;
        position: relative;
        margin:0 auto;
    }
    .bg{
        width:40px;
        height: 100%;
        position: absolute;
        background-color: #75CDF9;
    }
    .text{
        position: absolute;
        width: 100%;
        height: 100%;
        text-align: center;
        user-select: none;
    }
    .btn{
        width:40px;
        height: 38px;
        position: absolute;
        border:1px solid #ccc;
        cursor: move;
        font-family: "宋体";
        text-align: center;
        background-color: #fff;
        user-select: none;
        color:#666;
    }
</style>
```

### JS
``` bash
<script>
        //一、定义一个获取DOM元素的方法
        var $ = function(selector){
                return  document.querySelector(selector);
            },
            box = $(".drag"),//容器
            bg = $(".bg"),//背景
            text = $(".text"),//文字
            btn = $(".btn"),//滑块
            success = false,//是否通过验证的标志
            distance = box.offsetWidth - btn.offsetWidth;//滑动成功的宽度（距离）

        //二、给滑块注册鼠标按下事件
        btn.onmousedown = function(e){

            //1.鼠标按下之前必须清除掉后面设置的过渡属性
            btn.style.transition = "";
            bg.style.transition ="";

            //说明：clientX 事件属性会返回当事件被触发时，鼠标指针向对于浏览器页面(或客户区)的水平坐标。

            //2.当滑块位于初始位置时，得到鼠标按下时的水平位置
            var e = e || window.event;
            var downX = e.clientX;

            //三、给文档注册鼠标移动事件
            document.onmousemove = function(e){

                var e = e || window.event;
                //1.获取鼠标移动后的水平位置
                var moveX = e.clientX;

                //2.得到鼠标水平位置的偏移量（鼠标移动时的位置 - 鼠标按下时的位置）
                var offsetX = moveX - downX;

                //3.在这里判断一下：鼠标水平移动的距离 与 滑动成功的距离 之间的关系
                if( offsetX > distance){
                    offsetX = distance;//如果滑过了终点，就将它停留在终点位置
                }else if( offsetX < 0){
                    offsetX = 0;//如果滑到了起点的左侧，就将它重置为起点位置
                }

                //4.根据鼠标移动的距离来动态设置滑块的偏移量和背景颜色的宽度
                btn.style.left = offsetX + "px";
                bg.style.width = offsetX + "px";

                //如果鼠标的水平移动距离 = 滑动成功的宽度
                if( offsetX == distance){

                    //1.设置滑动成功后的样式
                    text.innerHTML = "验证通过";
                    text.style.color = "#fff";
                    btn.innerHTML = "&radic;";
                    btn.style.color = "green";
                    bg.style.backgroundColor = "lightgreen";

                    //2.设置滑动成功后的状态
                    success = true;
                    //成功后，清除掉鼠标按下事件和移动事件（因为移动时并不会涉及到鼠标松开事件）
                    btn.onmousedown = null;
                    document.onmousemove = null;

                    //3.成功解锁后的回调函数
                    setTimeout(function(){
                        alert('解锁成功！');
                    },100);
                }
            }

            //四、给文档注册鼠标松开事件
            document.onmouseup = function(e){

                //如果鼠标松开时，滑到了终点，则验证通过
                if(success){
                    return;
                }else{
                    //反之，则将滑块复位（设置了1s的属性过渡效果）
                    btn.style.left = 0;
                    bg.style.width = 0;
                    btn.style.transition = "left 1s ease";
                    bg.style.transition = "width 1s ease";
                }
                //只要鼠标松开了，说明此时不需要拖动滑块了，那么就清除鼠标移动和松开事件。
                document.onmousemove = null;
                document.onmouseup = null;
            }


        }
</script>
```

More info: [Baidu](https:baidu.com)