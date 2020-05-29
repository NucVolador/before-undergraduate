### 此处仅记录阅读react16.13.1官方文档时不理解的内容

#### 1.为什么使用JSX

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200528224541769.png" alt="image-20200528224541769" style="zoom:67%;" />

#### 2.JSX是不是涉及到了mixin

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200528224740352.png" alt="image-20200528224740352" style="zoom:67%;" />

#### 3.React.createElement是解析JSX的API，具体看一下它的API参数配置

最好能实现一个简单的createElement这个函数，了解一下原理，这个对象应该是虚拟dom里面除了diff算法外最核心的了！

#### 4.没有用过super，不太懂

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529222324384.png" alt="image-20200529222324384" style="zoom:67%;" />

知道super是干啥的，但是前段写的组件当中好像还没自己写过父组件，然后子组件extends