# js遍历

[TOC]

## 1.for循环

对于数值索引的数组来说，可以使用标准的for循环来遍历值

```javascript
const arr=[1,2,3,4];
for(let i=0;i<arr.length;i++){
    console.log(i);
}
```

通过遍历数组下标来取得数组的值；

## 2.for...in循环

for...in循环可以用来遍历对象的可枚举属性列表（包括原型链上的属性）

```javascript
const myObject={};

Object.defineProperty(myobject,"a",{
    //可枚举
	enumerable:true,
    value:2,
})
Object.defineProperty(myobject,"b",{
    //不可枚举
	enumerable:false,
    value:2,
})

for(let k in myObject){
    console.log(k,myObject[k])
	// a 2
}
//使用for...in循环是无法直接获得属性值的，因为它实际遍历的是对象中的所有可枚举属性，
//所以你需要手动获得属性值.
```

在数组上应用for...in循环，不仅仅会包含所有数值索引，还会包含所有可枚举属性.

所以最好在对象上应用for...in循环。如果要遍历数组最好使用传统的for循环来遍历.

## 3.for...of循环

#### 1.ES6新增的for...of循环

```javascript
const arr=[1,2,3];
for(let value of arr){
    console.log(value)
    //1
    //2
    //3
}
```

for...of循环首先会向所有被访问的对象请求一个迭代器对象，然后通过调用迭代器对象的next()方法来遍历所有返回值

在数组中有内置的@@iterator，因此for...of可以直接应用在数组上。

使用内置的@@iterator遍历数组

```javascript
const arr=[1,2,3];
//获取数组中的iterator对象:使用ES6中的符号Symbol.iterator来获取对象的@@iteraotr内部属性.
//@@iterator本身不是一个迭代器，而是一个返回迭代器对象的函数。
const it=arr[Symbol.iterator]();

it.next(); //{value:1,done:false}
it.next(); //{value:2,done:false}
it.next(); //{value:3,done:false}
it.next(); //{done:true}

//调用迭代器的next()方法会返回形式为{value:..,done:..}的值；
//value为当前的值，done是一个布尔值，表示是否还存在可以遍历的值
```

#### 2.给对象定义@@iterator

```javascript
const myObject={
    a:2,
    b:3
}
Object.defineProperty(myObject,Symbol.iterator,{
	enumerable:false,
    writeable:false,
    configurable:true,
    value:function(){
        let o=this;
        let idx=0;
        //对象中的属性数组
        let ks=Object.keys(o);
        return{
            value:o[ks[idx++]],
            done:(idx>ks.length);
        }
    }
})

const it=myObject[Symbol.iterator]();
it.next(); //{value:2,done:false}
it.next(); //{value:3,done:false}
it.next(); //{done:true}


for(let value of myObject){
	console.log(value);
}
// 2
// 3
```



## 4.foreach(...)

`**forEach()**` 方法对数组的每个元素执行一次给定的函数。

```javascript
const arr = ['a', 'b', 'c'];
arr.forEach(element => console.log(element));
// a
// b
// c

```

```
arr.forEach(callback(currentValue [,index [,array]])[,thisArg])
```

forEach方法中的function回调有三个参数：
		第一个参数是遍历的数组内容，
		第二个参数是对应的数组索引，
		第三个参数是数组本身

## 5.some(...)

some()是对数组中每一项运行给定函数，如果该函数对任一项返回true，则返回true。

```javascript
var arr = [ 1, 2, 3, 4, 5, 6 ]; 
 
console.log( arr.some( function( item, index, array ){ 
    console.log( 'item=' + item + ',index='+index+',array='+array ); 
    return item > 3; 
})); 
// item=1,index=0,array=1,2,3,4,5,6
// item=2,index=1,array=1,2,3,4,5,6
// item=3,index=2,array=1,2,3,4,5,6
// item=4,index=3,array=1,2,3,4,5,6
// true
```



## 6.every(...)

every()是对数组中每一项运行给定函数，如果该函数对每一项返回true,则返回true。

```javascript
var arr = [ 1, 2, 3, 4, 5, 6 ]; 

console.log( arr.every( function( item, index, array ){ 
    console.log( 'item=' + item + ',index='+index+',array='+array ); 
    return item > 3; 
}));
// item=1,index=0,array=1,2,3,4,5,6
// false
```


