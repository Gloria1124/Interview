## 类数组对象
类数组对象可以像数组一样进行读写，获取长度，遍历。_但不能使用数组的方法!_  

### 类数组对象使用数组方法
可以间接调用：使用**Array.prototype.方法.call()

### 类数组转数组
1. Array.prototype.slice.call(obj);
2. Array.prototype.splice.call(obj,0);
3. Array.from(obj);

## arguments对象
arguments对象就是一个类数组对象，只定义在函数体中，包括了函数的参数和其他属性。  
在函数中，arguments就代表Arguments对象。对于传入的参数，实参和arguments的值会共享，当没有传入时，实参与arguments值不会共享。  

