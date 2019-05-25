[Toc]
# ES6
## 类 Class
类是函数语法糖，用关键字Class声明 ，子类可以通过extends来继承父类。  
必有一个constructor，没有会默认添加一个。在类被new的时候，constructor会被执行  
new的参数会被constructor接收。  
```js
    class A {
        static anotherName(){console.log(this)}
        constructor(Axx)
        {
            console.log(Axx)
            console.log(this.showName)

        }

        showName(){console.log(this.noName)}
        otherName(){console.log(this.showName)}
        
    }
    
    class B extends A{
        constructor(Bxx){
            super('connectA')  // 这里的参数会被A的contructor接收到,并且会先执行A的constructor
            this.noName = Bxx
            console.log(Bxx)

        }
        showName(){console.log(this.noName,super.noName)}
        runFun(){return this.showName}
        runSuperFun(){ return super.showName}
        botherName(){console.log(this.anotherSuperName)}
    }
    let b = new B('connectB') // 这里的参数会被B的constructor接收到
    //connectA 
    //ƒ showName(){console.log(this.noName,super.noName)}
    //connectB
```     
A的constructor接受到了B中super传的值  
这里输出的是子类的showName  
B也接受到了 new B的时候传入的值  
 
```js
    b.runFun()
    //ƒ showName(){console.log(this.noName,super.noName)}
```
 runFun输出了子类的同名函数
 ```js
    b.runSuperFun()
    //ƒ showName(){console.log(this.noName)} 
```
   runSuperFun输出了父类的同名函数

所以，结合以上的例子，可以得出结论，  
==1.子类constructor内的super会调用父类constructor，但this绑定为子类==。  
==2.当super.XXXX的形式引用super时，此时指向父类的原型链，也就是A.prototype。== 
```js
b.otherName() //ƒ showName(){console.log(this.noName,super.noName)}
```
这段代码说明，==b在调用父类原型链上的方法时，方法内的this也指向了子类==
```js
A.anotherName()
 //class A {
 //       static anotherName(){console.log(this)}
 //       constructor(Axx)
 //       {
 //           console.log(Axx)
 //           console.log(this.showName)

 //       }

 //       showName(){console.log(t…
 ```
 ==静态方法则指向类本身。==
 
 ## export import
export default {}  
import 引入名 from 上述文件路径

export function 方法名(){}  
import {方法名} from 文件路径

export const 变量名 = 值  
import {变量名} from 文件路径  
import {变量名 as 重命名} from 文件路径  

==直接export一个变量的时候一定要加{}==

## let const 
1.建立块级作用域  
2.一旦声明不可再次声明  
3.有临时死区（TDZ）  
4.无变量提升（有声明提升，无初始化提升）  
经常被用来问，循环绑定事件监听
```js
for(var i = 0 ; i < 5; i++)
{
    setTimeout(function(){console.log(i)},0)
}
```
解决方案   
1.利用let
```js
for(let i = 0 ; i < 5; i++)
{
    setTimeout(function(){console.log(i)},0)
}
```
2.利用立即执行函数IIFE
```js
for(let i = 0 ; i < 5; i++)
{
    (function(i){setTimeout(function(){console.log(i)},0)})(i)
}
```
3.利用setTimeout的传值
```js
for(let i = 0 ; i < 5; i++)
{
    setTimeout(function(i){console.log(i)},0,i)
}
```

## 箭头函数
==没有constructor==  
==this指向外层的this==

## 解构赋值
ES6的解构只包括数组结构  
```js
const ary = [1,2,3]
const [a,b,c] = ary
a // 1
b // 2
c // 3
```
肥肠简单，可以用来快速值得交换
```js
let a = 1,b=2
[a,b] = [b,a]
```
对象的解构
```js
const person = {
    name: 'wk',
    age: 26
}
const {name, age} = person
```

## 展开运算
这个用来开发react的组件传值特别好用。  
假如说需要传递用户信息，
```html
<ReleaseCard isLast={index === data.length - 1} style={[styles.scrollItem]} data={val}/>
```
```html
let data = {
    isLast: index===data.length - 1,
    data : val
}
<ReleaseCard {...data}/>
```