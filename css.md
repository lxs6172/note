## <center>css</center>
### scss/sass
对css语言的扩展，可以使用变量，嵌套规则，mixins,导入等功能。

参考文档：`https://juejin.im/post/5d565610e51d4561ce5a1c39#heading-2`

1. 变量：定义变量，在样式中将定义的变量填入其中。定义变量使用$,如果变量名相同，后定义的属性会覆盖掉前面的属性

2. 嵌套规则：子元素可以继承父元素的样式，还有一个 & 父元素选择的标识符,在嵌套层写入这个符号就代表的是选中了它的父元素。

3. mixins：混合器可以将公共代码提取出来，然后有需要的地方进行引用。

4. 导入：引入写好的scss/sass的样式，在页面中使用，使用@import加路径引入,或者可以省略文件后缀，但是名称前面要加入'_'。

   `@import './index.scss'`
   
### less
``

1. 定义变量的时候，使用的是@，定义出来的是常量，只能定义一次，不可以重复使用。
2. 选择器是变量的，但必须使用‘{}’将名称括起来。  

### scss与less的区别
1. scss定义变量使用‘$’，后定义的相同变量名的，后一个可以覆盖前一个。less定义变量使用‘@’，只能定义一次

### Flex布局
`https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/`

1. Flexbox布局最适合应用程序的组件和小规模布局（一维布局），而Grid布局则适用于更大规模的布局（二维布局）
2. 设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。
3. Webkit 内核的浏览器，必须加上-webkit前缀。
4. 给div这类块状元素元素设置display:flex或者给span这类内联元素设置display:inline-flex，flex布局即创建！其中，直接设置display:flex或者display:inline-flex的元素称为flex容器，里面的子元素称为flex子项。
5. 当多属性同时使用时，使用空格分隔。


* 设置在父级上的属性
  1. flex-direction属性决定主轴的方向（即项目的排列方向）row 为行，column为列显示。
  2. flex-wrap属性排列方式。nowrap:不换行，wrap:换行，第一行在上侧,wrap-reverse：换行，第一行在下方。
  3. flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，表示flex布局的flow流动特性，默认值为row nowrap。
  4. justify-content属性定义了项目在主轴上的对齐方式。 可以设置为水平居中对齐。
 
     `justify-content: flex-start（左对齐） | flex-end（右对齐） | center（居中对齐） | space-between | space-around;`
     
  5. align-items属性定义项目在交叉轴上如何对齐。y轴的排列属性，设置垂直居中
  
  
      		flex-start：容器顶部对齐。
			flex-end：容器底部对齐。
			center：垂直居中对齐。
			baseline: 项目的第一行文字的基线对齐。
			stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
			
 6. align-content 垂直方向上的对齐方式，center垂直居中对齐
 
* 设置在子元素上的属性
  1. order：改变子项的排序顺序
  2. flex-grow：扩展子项的宽度，占据除元素外的空白间隙。 
  
  			
### Grid布局(网格布局)

`https://www.zhangxinxu.com/wordpress/2018/11/display-grid-css-css3/`

1. 给'div'这类块状元素元素设置display:grid或者给<span>这类内联元素设置display:inline-grid，Grid布局即创建！