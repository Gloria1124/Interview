## JS 实现new
### 实现步骤
1. 创建一个新对象
2. 通过new创建的每个对象最终被[[prototype]]链接到这个函数的prototype对象上
3. 使用apply，改变构造函数this的指向到新建的对象
4. 若函数没有返回引用类型，那么就返回这个新的对象,若返回值是一个对象就返回这个对象
```javascript
function new(){
  var obj={};
  Consturctor=[].shift.call(arguments);
  obj._proto_=Consturctor.prototype;
  var res=Constructor.apply(obj,arguments);
  return typeof res==='object'?res:obj;
}
```

*[].shift.call(arguments)*  
获取传进来的构造函数，shift方法内部的this会绑定到arguments上，并且获得第一个参数即构造函数。  

*构造函数的返回值*  
若返回值为对象类型则则返回这个对象，否则返回构造的新对象。  

> 参考文章  
[JavaScript深入之new的模拟实现](https://github.com/mqyqingfeng/Blog/issues/13)

