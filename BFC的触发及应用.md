## BFC的定义
BFC是页面上的一块渲染区域，有一套渲染规则，规定了内部元素的布局，但不会影响外部布局。
## 触发条件
1. 根元素  
2. float的值不为none  
3. overflow的值不为visible  
4. display的值为flex，inline-flex，inline-block，table-cell，table-caption
5. position的值为absolute，fixed  
## BFC的内部渲染规则
1. 内部的box会在垂直方向上一个接一个的放置  
2. 垂直方向上的距离由margin决定（同一个bfc的两个相邻的box的margin会发生重叠）  
3. 每个元素的左外边距与包含块的左边界相接触，即使浮动元素也是如此  
4. bfc的区域不会与浮动的区域重叠  
5. 计算bfc的高度时，浮动元素的高度也参与计算  
6. bfc就是页面上一个隔离的独立容器，容器里面的元素不会影响到外面  
## 作用
1. 防止margin重叠  
当一个bfc下的两个元素发生margin重叠时，可以给其中一个元素包裹一个容器触发心的bfc  
*overflow:hidden/display:inline-block*  
2. 清除内部浮动
e.g 高度塌陷问题。通常父元素的高度会被子元素撑开，若子元素是浮动元素，父元素会发生高度塌陷，上下边界重合，可以用bfc来清楚浮动。  
让父元素触发bfc，因为计算bfc的高度时，浮动元素也会参与计算。  
3. 自适应多栏布局  
  * 两个并列的div，若侧边栏为浮动元素，且高度低于右边div，则右边div左边会与包含块的左边相接触。  
  可以给右边div触发bfc，因为bfc的区域不会与浮动区域重叠，所以右边div将与侧边栏并列形成自适应的两栏布局。  
  * 自适应三栏布局  
  左右div分别float：left、right，中间栏触发bfc。中间栏会根据浏览器宽度自适应。
