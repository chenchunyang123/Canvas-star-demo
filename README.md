## Canvas星星连线特效
>demo演示：[猛戳这里](https://chenchunyang123.github.io/Canvas-star-demo/star.html)
### 思路分析
#### 第一步：定义星星类
>参数依次为（画笔，x坐标，y坐标，半径），并且定义一个随机的控制速度和方向的值（如下）:
```
function Star(ctx,x,y,r) {
        this.ctx = ctx;
        this.x = x;
        this.y = y;
        this.r = r;
        this.x_speed = (parseInt(Math.random()* 3) + 1) * Math.pow(-1,parseInt(Math.random()* 10) + 1)/5;
        this.y_speed = (parseInt(Math.random()* 3) + 1) * Math.pow(-1,parseInt(Math.random()* 10) + 1)/5;
    }
```
>然后在它的原型上添加移动的方法，上面的随机速度值就起到了作用：
```
Star.prototype.move = function() {
        this.x += this.x_speed;
        this.y += this.y_speed;
    }
```
>再添加渲染方法
```
Star.prototype.render = function() {
        this.ctx.beginPath();
        this.ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2);
        this.ctx.closePath();
        this.ctx.fill();
    }
```
>此时星星会出显示区域，我们需要边界判断，到达边界则用相反的速度值
```
 Star.prototype.changeX = function() {
        this.x_speed = -this.x_speed;
    }
    Star.prototype.changeY = function() {
        this.y_speed = -this.y_speed;
    }
```

#### 第二步：实例化100颗星星
>当对象比较多的时候，用数组来存储
```
var arr = [];
    for(var i = 0; i < 100; i++) {
        arr.push(new Star(ctx, Math.random() * width, Math.random() * height, 1));
    }
```
#### 第三步：创建鼠标位置的星星
>根据效果，还有一颗星星位于鼠标的顶点，通过鼠标移动事件来触发，实时获取顶点位置
```
var mouse_star = new Star(ctx, 0, 0, 2);
    document.onmousemove = function(e) {
        var mouse_x = e.clientX;
        var mouse_y = e.clientY;
        mouse_star.x = mouse_x;
        mouse_star.y = mouse_y;
    }
```
#### 第四步：开启定时器动画
>第一步是清屏，可以理解为动画是每一帧画面的集合，进行下一帧时，如果不清除前面帧的动画，则星星会变成向四处发生的射线。
```
var timer = setInterval(function() {
        // 清屏
        ctx.clearRect(0, 0, width, height);
        // 渲染鼠标星星
        mouse_star.render();

        arr.forEach(function(value,index) {
            value.move();
            // 判断边界
            if (value.x < 0 || value.x > width) {
                value.changeX();
            }
            if (value.y < 0 || value.y > height) {
                value.changeY();
            }
            value.render();
        })
        arr.forEach(function(value,index) {
                for(var i = index + 1; i < arr.length; i++) {
                    if(Math.abs(value.x - arr[i].x) < 50 && Math.abs(value.y - arr[i].y) < 50) {
                        // console.log(1)
                        line(value.x, value.y, arr[i].x, arr[i].y);
                    }
                }
                // 判断星星与其它所有星星之间的关系
                if (Math.abs(value.x - mouse_star.x) < 150 && Math.abs(value.y - mouse_star.y) < 150) {
                    // 连线
                    line(value.x, value.y, mouse_star.x, mouse_star.y);
                }
            }
        )
    },20);
```
里面的line是封装的连线方法。
