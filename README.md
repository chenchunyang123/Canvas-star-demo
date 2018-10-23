## Canvas星星连线特效
>demo演示：[猛戳这里](https://chenchunyang123.github.io/Canvas-star-demo/star.html)
### 思路分析
#### 第一步：制作单个星星
>定义一个星星类，参数依次为（画笔，x坐标，y坐标，半径），并且定义一个随机的控制速度和方向的值（如下）:
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























