## CSS水平居中
1. 元素宽度已知  
给内部元素设置
```JavaScript
margin:0 auto;
```
2. 针对行内元素
```javascript
text-align:center;
```
3. 利用定位（未知宽度）
```javcascript
.container{
  position:relative;
}
.box{
  position:absolute;
  left:50%;
  transform:translate(-50%,0);
}
```
4. flex布局
对父元素：
```javascript
.father{
  display:flex;
  justify-content:center;
}
```

## CSS垂直居中
1. 使用绝对定位和transform
```javascript
.container{
  position:relative;
}
.child{
  position:absolute;
  top:50%;
  transform:translate(0,-50%);
}
```
2. 绝对定位结合margin:auto
```javascript
.container{
  position:relative;
}
.child{
  position:absolute;
  top:0;
  bottom:0;
  magin:auto;
}
```
*把要垂直居中的元素相对于父元素绝对定位，top和bottom设为相等的值，然后再将margin设为auto。*  
3. 使用flex布局
```javascript
.father{
  display:flex;
  align-itms:center;
}
```

## CSS水平垂直居中
1. 绝对定位+margin:auto(*top，bottom，right，left的值相等即可，不一定要为0*)
```javascript
.container{
  position:relative;
}
.child{
  position:absolute;
  top:0;
  bottom:0;
  right:0;
  left:0;
  margin:auto;
}
```

2. 绝对定位+负margin(元素已知宽度)
*margin-left，margin-top为负值时会向左/上移动，margin-right，margin-bottom为负时参考线会向左、上移动*  
```javascript
.container{
  position:relative;
}
.child{
  position:absolute;
  width:100px;
  height:100px;
  top:50%;
  left:50%;
  margin-left:-50px;
  margin-top:-50px;
}
```
3. 使用flex布局
```javascript
.father{
  display:flex;
  justify-content:center;
  align-items:center;
}
