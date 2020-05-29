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