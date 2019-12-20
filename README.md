## snow类有两个主要方法 

​	play()  开启定时器，雪花开始生成飘落

​	stop() 删除定时器，雪花停止生成

类的使用需要：

1. 必要的

```
html,body{background-color:#282923;height:100%;width:100%;font-size:65.5%;margin:0;padding:0;}
```

【不是必要的】

- 需要定义一个动画 rotes 这是雪花飘落过程中雪花本身的动作，这里用的是旋转；可以不要

```
@keyframes rotes{
			100%{
				transform:rotate3d(1,-1,1,720deg);
			}
		}
```

- 定义一下.snow的样式 

````
		.snow{
			color:white;
			animation:rotes 3s infinite linear;
		}
````



### 说一下主要的实现方法

- 或许你没猜到，那么这个动画效果是如何实现的呢？transition属性！！`第一个定时器生成了雪花的初始状态，第二个在生成的100毫秒后给了元素一个结束状态(关键帧)；第三个定时器只用了一行代码，snowClone.remove();三个定时器嵌套使用了`
- 确定删除的属性。因为用的是嵌套，里面对元素都是用局部变量定义的，这样的话，不用给元素起id或者class，就可以确定是哪个元素了，因为一个局部区域里，只有一个雪花元素snowClone；第三个定时器选定到时间点则删除
- 3d的效果的实现：给父元素添加两个属性
  - transform-style:preserve-3d;  //保留子元素的3d效果
  - transform:perspective(50rem);  //屏幕离场景的距离，可以用px。rem主要是移动端用的单位；
- 子元素的3d移动 ：
  - 用 tranform ： translate3d（x,y,z）属性;三个值不表示三个方位的位移量水平x，竖直y，垂直向屏幕是z单位可以是px，rem，等

##  这样做有什么好处呢？

- 雪花的飘落用的是translate3d属性，这属性会触发GPU加速，和positionl相比动画会更流畅
- setInterval定时器只用了一个，用来生成雪花，雪花的移动和元素删除分别用了两个setTimeout定时器来完成；定时器的减少对降低了性能的开销；