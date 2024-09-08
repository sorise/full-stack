## [CSS 颜色、透明度](#)
> **介绍** : 如何设置颜色和透明度

| 属性       |CSS Version 版本|继承性|简介|
|:---------|:---|:---|:---|
| color	|CSS1|	有	|指定颜色。请参阅颜色值。|
| opacity	|CSS3	|无|	检索或设置对象的不透明度。|

**颜色值**

| 属性                    |CSS Version 版本| 简介       |
|:----------------------|:---|:-------------------------------------|
| Color Name	           |CSS1| 	用颜色名称来指定颜色。包括基本颜色关键字、系统颜色、SVG颜色关键字等 |
| HEX	                  |CSS1| 	十六进制记法。语法如：#rrggbb或#rgb             |
| rgb(红,绿,蓝)	           |CSS2| 	rgb记法                               |
| rgb(红,绿,蓝,透明度)	       |CSS3| 	rgba记法                              |
| hsl(色调,饱和度,亮度)	       |CSS3| 	hsl记法                               |
| hsla((色调,饱和度,亮度,透明度)	 |CSS3| 	hsla记法                              |
| transparent           |	CSS1/CSS3| 	transparent是全透明黑色(black)的速记法，即一个类似rgba(0,0,0,0)这样的值。                                 |


**opacity** 透明度设置
```css
div{
    filter:alpha(opacity=50);
} /* for IE8 and earlier */
div{
    opacity:.5;
} /* for IE9 and other browsers */
```