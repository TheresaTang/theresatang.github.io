---
title: exports与module.exports、export与export default
date: 2019-07-02 23:09:46
tags:
---
### exports与module.exports
Node.js采用的是CommonJS模块规范，在 Node.js 模块系统中，每个文件都被视为一个独立的模块，模块内的本地变量是私有的，因为模块由 Node.js 封装在一个函数中。它采用exports、module.exports导出模块，require导入模块。
exports 是对module.exports的{}的引用，我们可以exports将函数和对象添加到模块的根部，他只能使用.进行添加属性，如下所示：
```javascript
// a.js
exports.a = '1'

//index.js
let a = require('./a.js')
console.log(a)  // {a: '1'}
```
注意：不能对exports直接赋值一个变量，这样的话直接切断了export与module.exports之间的联系，即exports不再是对module.exports的引用，而是对赋值的变量的引用，如下所示：
```javascript
// a.js
let a = '1'
exports = a
console.log(exports) // 1

//index.js
let a = require('./a.js')
console.log(a)  // 控制台输出{}
```
当然，在对module.exports进行重新赋值，exports与，module.exports同样也会切断联系，exports引用的依旧是{}，而module.exports则指向了新的内存地址，如下所示
```javascript
// a.js
let a = 1
module.exports = a
exports.b = 
console.log(exports) // {b: 1}

//index.js
let a = require('./a.js')
console.log(a)  // 控制台输出1
```
require导入模块实际上是导入module.exports，所以上述代码中index.js输出的是1，使用exports添加的属性是无法访问到的。
### export与export default
在ES6中，export语句用于从模块中导出函数、对象或原始值，以便其他程序可以通过import语句使用它们。
他有两种导出方式分别为export(命名导出)和export default(默认导出)
export与export default的区别有：
1.每一个模块中可以有多个export导出，但是只能有一个export default导出
2.在导入期间，export导出的模块必须使用相应对象的相同名称。但是，可以使用任何名称导入默认导出
示例如下：
```javascript
// a.js
// export导出
//导出变量
export const a = '100';  
 //导出方法
export const say = function(){ 
    console.log('hi');
}
function sing(){
   console.log('everbody'); 
}
export { sing };

//b.js
//export default导出
const a = 100;
export default a; 
//export defult const a = 100;// 这里不能写这种格式。

//index.js
import { say, sing } from './a.js'; //导入了 export 方法 
import m from './b.js';  //导入了 export default 
// import * from './b.js';  //export default 不能用这种方法导入
import * as test from './a.js'; //as 集合成对象导入
```
