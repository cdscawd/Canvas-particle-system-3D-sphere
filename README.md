

## 动画生成过程
    - 创建`canvas`，设置`canvas`中心点，变量初始化:`function setParameters`
    - 生成指定数量的例子 同时完成例子的初始化(设置粒子`x`,`y`,`z` 以及增量`vx`,`vy`,`vz`)：`function createParticles`
    - 初始化不同形状:`function setupFigure`
    - 给`click`(立即切换到下一图形) 和 `onmouseover`(旋转), 绑定`this` 因为`addEventListener` 会把`this` 绑定在DOM上`:function reconstructMethod`
    - `bindEvent` 给 `click`(立即切换到下一图形) 和 `onmouseover`(旋转) 绑定事件
    - 绘图:`function drawFigure`

## 重点 (绘图: drawFigure)
    对于环形制作，看看源码里的环形公式：

    ```
    createTorus : function(){ 
        var theta = Math.random() * Math.PI * 2, 
        x = this.SCATTER_RADIUS + this.SCATTER_RADIUS / 6 * Math.cos(theta), 
        y = this.SCATTER_RADIUS / 6 * Math.sin(theta), 
        phi = Math.random() * Math.PI * 2
    }
    return {
         x : x * Math.cos(phi), 
         y : y, 
         z : x * Math.sin(phi), 
         hue : Math.round(phi / Math.PI * 30) 
    };
    ```

    环形 `x`, `y`, `z` 推导 (`x`屏幕面的`x`方向， `y`屏幕面的`y`方向， `z` 垂直于屏幕面的方向)
    
    <img src="/static/torus.png">

    环形的z轴是相对屏幕对称的。

    公式：
    ```
    createCone: function() {
        var status = Math.random() > 1 / 3,
        x,
        y,
        phi = Math.random() * Math.PI * 2,
        rate = Math.tan(30 / 180 * Math.PI) / this.CONE_ASPECT_RATIO;
        if (status) {
            y = this.SCATTER_RADIUS * (1 - Math.random() * 2);
            x = (this.SCATTER_RADIUS - y) * rate;
        } else {
            y = -this.SCATTER_RADIUS;
            x = this.SCATTER_RADIUS * 2 * rate * Math.random();
        }
        return {
            x: x * Math.cos(phi),
            y: y,
            z: x * Math.sin(phi),
            hue: Math.round(phi / Math.PI * 30)
        };
    },

    ```

    <img src="/static/cons.png">

    这里 `tana` 被作者替换成 `1.5 tan30`， 这里我们可以理解为`a` 为`30`，宽高比为`1.5`。

    花瓶形(不知道数学里叫什么) 看看源码里的公式

    ```
    createVase: function() {
        var theta = Math.random() * Math.PI,
            // x = Math.abs(this.SCATTER_RADIUS * Math.cos(theta) / 2) + this.SCATTER_RADIUS / 8, 
            x = Math.abs(this.SCATTER_RADIUS * Math.cos(theta) / 2) + this.SCATTER_RADIUS / 8, 
            y = this.SCATTER_RADIUS * Math.cos(theta) * 1.2, 
            phi = Math.random() * Math.PI * 2; 
        return { 
            x : x * Math.cos(phi), 
            y : y, z : x * Math.sin(phi), 
            hue : Math.round(phi / Math.PI * 30) 
        }; 
    }
    
    ```

    <img src="/static/vase.png">

## 结语
Canvas 3D 坐标转换可谓基础，掌握好canvas坐标转换，配合图形的三维方程，可以让粒子做出美妙的运动，释放无穷尽的想象。
