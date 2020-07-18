# Class的本质

在react的类组件事件绑定中，遇到了一下几种情况，进行深入挖掘class底层实现。

情况如下

```
//情形①
class App extends React.Component{
	constructor(props){
		super(props);
		this.state = {
			isToggleOn: true
		}
		// 写法①
		this.handleClick= this.handleClick.bind(this);
	}
	// 写法①	
	handleClick(){
	
	}
	
	render(){
		return (
			//写法①
			<button onClick={this.handleClick}>
				{this.state.isToggleOn? 'On': 'off'}
			</button>
		)
	}
	
}
```

```
//情形②
class App extends React.Component{
	constructor(props){
		super(props);
		this.state = {
			isToggleOn: true
		}
	}
	// 写法②	
	handleClick= ()=>{
	
	}
	
	render(){
		return (
			<button onClick={this.handleClick}>
				{this.state.isToggleOn? 'On': 'off'}
			</button>
		)
	}
	
}
```

```
//情形③
class App extends React.Component{
	constructor(props){
		super(props);
		this.state = {
			isToggleOn: true
		}
	}
    
	handleClick(){
	
	}
	
	render(){
		return (
			//写法③
			<button onClick={this.handleClick.bind(this)}>
				{this.state.isToggleOn? 'On': 'off'}
			</button>
		)
	}
	
}
```

## 精简，深挖class底层实现

针对情况①的精简

```
class Demo{
	constructor(){
		this.state = {
			name: "123"
		}
	}
	handleClick(){
		console.log("test")
	}
}
```

babel转化

```
'use strict'

function _instanceof(left, right) {
  if (
    right != null &&
    typeof Symbol !== 'undefined' &&
    right[Symbol.hasInstance]
  ) {
    return !!right[Symbol.hasInstance](left)
  } else {
    return left instanceof right
  }
}

function _classCallCheck(instance, Constructor) {
  if (!_instanceof(instance, Constructor)) {
    throw new TypeError('Cannot call a class as a function')
  }
}

function _defineProperties(target, props) {
  for (var i = 0; i < props.length; i++) {
    var descriptor = props[i]
    descriptor.enumerable = descriptor.enumerable || false
    descriptor.configurable = true
    if ('value' in descriptor) descriptor.writable = true
    Object.defineProperty(target, descriptor.key, descriptor)
  }
}

function _createClass(Constructor, protoProps, staticProps) {
  if (protoProps) _defineProperties(Constructor.prototype, protoProps)
  if (staticProps) _defineProperties(Constructor, staticProps)
  return Constructor
}

var Toogle = /*#__PURE__*/ (function () {
  function Toogle() {
    _classCallCheck(this, Toogle)

    this.state = {
      name: '123',
    }
  }

  _createClass(Toogle, [
    {
      key: 'handleClick',
      value: function handleClick() {
        console.log('test')
      },
    },
  ])

  return Toogle
})()

```

********

情况②

```
class Demo{
	constructor(){
		this.state = {
			name: "123"
		}
	}
	handleClick= ()=>{
		console.log("test")
	}
}
```

babel

```
'use strict'

function _instanceof(left, right) {
  if (
    right != null &&
    typeof Symbol !== 'undefined' &&
    right[Symbol.hasInstance]
  ) {
    return !!right[Symbol.hasInstance](left)
  } else {
    return left instanceof right
  }
}

function _classCallCheck(instance, Constructor) {
  if (!_instanceof(instance, Constructor)) {
    throw new TypeError('Cannot call a class as a function')
  }
}

function _defineProperty(obj, key, value) {
  if (key in obj) {
    Object.defineProperty(obj, key, {
      value: value,
      enumerable: true,
      configurable: true,
      writable: true,
    })
  } else {
    obj[key] = value
  }
  return obj
}

var Demo = function Demo() {
  _classCallCheck(this, Demo)

  _defineProperty(this, 'handleClick', function () {
    console.log('test')
  })

  this.state = {
    name: '123',
  }
}
```

***

扩展第三种形式

方法和箭头函数共存时如何转化、构造器中声明成员变量与外部声明的区别

```
class Toogle {
	constructor(){
  		 this.state = {
       		name: "123"  
       }
  	}
  app = "appvalue"
  
  handleClick= ()=> {
  	console.log("test")
  }
  
  handleMouse(){}
}
```

babel

```
'use strict'

function _instanceof(left, right) {
  if (
    right != null &&
    typeof Symbol !== 'undefined' &&
    right[Symbol.hasInstance]
  ) {
    return !!right[Symbol.hasInstance](left)
  } else {
    return left instanceof right
  }
}

function _classCallCheck(instance, Constructor) {
  if (!_instanceof(instance, Constructor)) {
    throw new TypeError('Cannot call a class as a function')
  }
}

function _defineProperties(target, props) {
  for (var i = 0; i < props.length; i++) {
    var descriptor = props[i]
    descriptor.enumerable = descriptor.enumerable || false
    descriptor.configurable = true
    if ('value' in descriptor) descriptor.writable = true
    Object.defineProperty(target, descriptor.key, descriptor)
  }
}

function _createClass(Constructor, protoProps, staticProps) {
  if (protoProps) _defineProperties(Constructor.prototype, protoProps)
  if (staticProps) _defineProperties(Constructor, staticProps)
  return Constructor
}

function _defineProperty(obj, key, value) {
  if (key in obj) {
    Object.defineProperty(obj, key, {
      value: value,
      enumerable: true,
      configurable: true,
      writable: true,
    })
  } else {
    obj[key] = value
  }
  return obj
}

var Toogle = /*#__PURE__*/ (function () {
  function Toogle() {
    _classCallCheck(this, Toogle)

    _defineProperty(this, 'app', 'appvalue')

    _defineProperty(this, 'handleClick', function () {
      console.log('test')
    })

    this.state = {
      name: '123',
    }
  }

  _createClass(Toogle, [
    {
      key: 'handleMouse',
      value: function handleMouse() {},
    },
  ])

  return Toogle
})()
```



## 总结

ok，class还真就是个语法糖！

- 分两类，成员变量和方法
- 成员变量
  - ①在构造器内声明的：在constructor中定义赋值的成员变量，转义后直接在'构造器'（es5的原生构造，即工厂模式）内定义。（构造器内的，相当于直接this.name，自带了原型对象），**默认不枚举**
  - ②在构造器外声明的：在constructor外部定义的成员变量，需要使用Object.defineProperty把该变量挂载到原型下，**默认能枚举**
- 方法
  - ①handle(){}，这种类似constructor式的，**默认不可枚举**，其他一样，可修改（writable，也就是可以赋值）、可删除（configurable，是否能被delete该属性）、
  - ②handle = ()=>{}，这种类似随便声明一个变量方法的，在转义后的构造器中进行defineProperty进行绑定，**默认可以枚举**，其他一致

***nice，搞定了一个挺高难度的问题！***