**1.JSX中的 Virtual Dom**

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529131542183.png" alt="image-20200529131542183" style="zoom:67%;" />



props里面包括attr以及children



**2.组件名称必须大写**

![image-20200529134511324](C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529134511324.png)



**3.props只读，不能修改，只能使用传入的值，不能修改**

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529135420314.png" alt="image-20200529135420314" style="zoom:67%;" />

所有 React 组件都必须像**纯函数**一样保护它们的 props 不被更改。



**4.state与props的区别**

State 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。

**5.react生命周期函数（了解即可，建议采用Hooks取代）**

***挂载***

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529222902545.png" alt="image-20200529222902545" style="zoom:60%;text-align:left" />

注：componentWillMount()将被移除

***更新***

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529223129655.png" alt="image-20200529223129655" style="zoom:67%;" />

注：componentWillUpdate()和componentWillReceiveProps()将被移除

***卸载***

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529223226850.png" alt="image-20200529223226850" style="zoom:67%;" />

***错误处理***

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529223303699.png" alt="image-20200529223303699" style="zoom:67%;" />

***生命周期中的其他属性***

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529223432271.png" alt="image-20200529223432271" style="zoom:50%;" />

***生命周期图谱***

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529223703867.png" alt="image-20200529223703867" style="zoom:50%;" />

**具体生命周期相关API请查看**：https://zh-hans.reactjs.org/docs/react-component.html （导航API中的React.component）

**6.state三大特点**

①只有在constructor中才能对state直接赋值，其他需要调用setState进行更改

②state的更新是异步的

假如更新state需要依赖现有state进行更新，即setState中依赖state进行赋值操作，则可能会有延迟

解决方案：setState接收一个函数，函数的两个形参分别为prior_state，prior_props，进而解决依赖旧state更新现有state造成的延迟

③state的更新会被合并

这么解释吧，state中有A、B两个对象，A、B分别独立更新，更新A、B只是单独改变state中A、B两个的值，并非直接把state拷贝一份，然后把新state重赋值然后用新state取代旧的（即未改变原state地址）(A、B的合并只是浅合并，可以考虑使用immutable)

**7.单向数据流**

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200529224820879.png" alt="image-20200529224820879" style="zoom:67%;" />

函数组件无状态，class组件有状态，state是私有的，局部的，封装的！可以把state传递给子组件的props属性，但子组件并不知道自己的props是父的state还是props还是手动输入来的！

**8.react事件**

- 事件名为小驼峰
- 显式调用event.preventDefault进行终止默认行为，不能通过返回false阻止。
  - SyntheticEvent：https://zh-hans.reactjs.org/docs/events.html
  - https://www.w3.org/TR/DOM-Level-3-Events/
- 组件事件绑定三种方法
  - handleClick = () => {}
  - 构造器中 this.handleClick= this.handleClick.bind(this)
  - render中 <button onclick={this.handleClick.bind(this)}/>

**在 JavaScript 中，class 的方法默认不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this`。**

**9.react中传递event对象**

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200616180600474.png" alt="image-20200616180600474" style="zoom:50%;" />

**10.条件渲染/逻辑判断**

一共三种条件渲染

- 通过变量在组件内部或render中判断渲染（因为在JSX中不能进行语句判断，故逻辑控制提升在函数中而不是return的JSX中）
- 在JSX中利用**逻辑与**运算符进行判断（利用短路效应）
- 在JSX中使用三目运算符（其实就相当于给if，只不过if是逻辑语句，三目是表达式express）

**11.diff算法中列表渲染的key**

<img src="C:\Users\19755\AppData\Roaming\Typora\typora-user-images\image-20200617095645791.png" alt="image-20200617095645791" style="zoom:50%;" />

现在说一下key的底层原理：

一、react的渲染过程（非生命周期）

普通的浏览器渲染是：

解析html文件，生成dom tree，同时解析CSS，生成CSS层级渲染规则树，二者结合，形成render tree，在解析过程中，遇到script JS代码时，会阻塞JS dom树，通常加入defer/async或调整script在恰当的位置，涉及到JS DOM操作时，JS在CSS后执行，覆盖css。render tree主要是形成各个盒子的具体信息，相当于是布局layout，如果后续出现回流reflow、重绘repaint等修改dom tree 或 cssom情况，会重新生成render tree。

react渲染过程：

react通过JSX先构建内部的 virtual dom tree，然后在渲染成为真实的dom tree和render tree。每一次页面交互出现变化，均为对比修改virtual dom tree前后的不同，减少重排重绘次数，合并为一次渲染，然后再把不同映射到 dom tree 、render tree上。（重排重绘最耗性能，js中操作dom的部分相对效率最低下，所以把多次重排重绘合为一次有助于提高性能）

**react的渲染的核心：比较virtual dom变化前后的结构，进而渲染**

而virtual dom是如何进行比较的？

核心采用diff算法比较：

1. 比较外层容器，从根节点开始比，若根节点类型发生变化直接覆盖旧virtual dom
2. 比较节点属性，属性变化，只修改属性即可
3. 若节点类型、属性均为变化，比较key值是否发生变化，key发生变化则替换。