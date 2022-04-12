---
title: css集锦
renderNumberedHeading: true
grammar_cjkRuby: true
---


   animation 动画  infinite  无限   translate   平移
   radius 半径  opacity  透明度
   justify-content: center  水平居中
   align-items: center   垂直居中
   vertical-align: middle;  垂直居中
   perspective  3D景深
        再牛逼的梦想,也抵不住你傻逼似的坚持！     
   background: radial-gradient(circle,#333333,#000000)
      径向渐变 成一个圆形扩散的形式把颜色过渡出来
   overflow: hidden;  超处部分隐藏掉
   z-index:   层级
   cursor: pointer; 检测鼠标
   <del>删除线</del> 文本具有删除线和删除的语义
   text-decoration: line-through;  添加横向的删除线段
   text-decoration:none;  取消下划线
   list-style: none;  删除列表项的前缀
   :active 鼠标点按   :hover 鼠标悬停  :link 未访问
   :visited  访问后(限a标签)
   overflow:hidden; 隐藏超出部分
   overflow:scroll; 超出部分可以滚动查看
   border-collapse: collapse;  边框线合并
   perspective-origin: 30% 80%;  设置3d场景切换时所在的轴位置
   box-shadow:    盒阴影:1.横向偏移量 2.纵向偏移量 3.模糊程度 4.阴影大小 5.阴影颜色 inset设置元素在阴影内
   text-shadow:   文本阴影 1.横向偏移量 2.纵向偏移量 3.模糊程度 4.阴影颜色
   transition-delay:0.2s  延迟0.2秒执行过渡动画
   placeholder  // 输入框属性名  输入框提示语
   reseze:none;   //不支持留言板拉伸
   background-repeat: no-repeat;   背景图不重复
   white-space: nowrap;   规定段落中的文本不进行换行    
   white-space: pre;      空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。
   background-size:cover; 左右铺满 上下居中
   background-size:contain; 上下铺满 左右居中;
   background: linear-gradient(blue 10%,purple 10%);  线性渐变  默认从上向下;
   word-break:break-all;  数字字母换行显示；
   flex-shrink: 0;   // 弹性布局可设置宽度(设置给当前元素)
   width:100px;
	文本超出省略号:
   overflow: hidden;
   text-overflow:ellipsis;
   white-space: nowrap;
	超出两行省略
overflow:hidden; 
text-overflow:ellipsis;
display:-webkit-box; 
-webkit-box-orient:vertical;
-webkit-line-clamp:2; enter code here