---
title: CSS 元素垂直居中的几种常用方法
tags:
  - 前端
  - CSS
  - 垂直居中
excerpt: |2-

      
        
        
          <h3 id="子绝父相"><a href="#子绝父相" class="headerlink" title="子绝父相"></a>子绝父相</h3><p>子绝父相：子元素绝对定位，父元素相对定位。</p>
  <h4 id="不知道子元素高度和父元素高度的情况"><a href="
        
      
      
date: 2018-02-02 16:54:40
---

### [](#子绝父相 "子绝父相")子绝父相

子绝父相：子元素绝对定位，父元素相对定位。
<!-- more -->
### [](#子绝父相 "子绝父相")子绝父相

子绝父相：子元素绝对定位，父元素相对定位。

#### [](#不知道子元素高度和父元素高度的情况 "不知道子元素高度和父元素高度的情况")不知道子元素高度和父元素高度的情况

1  
2  
3  
4  
5  
6  
7  
8  
9  

.parentElement {  
 position: relative;  
}  
  
.childElement {  
 position: absolute;  
 top: 50%;  
 transform: translateY(-50%);  
}  

#### [](#知道子元素高度和父元素高度的情况 "知道子元素高度和父元素高度的情况")知道子元素高度和父元素高度的情况

1  
2  
3  
4  
5  
6  
7  
8  
9  
10  
11  
12  

.parentElement {  
 position: relative;  
}  
  
.childElement {  
 position: absolute;  
 top: 0;  
 bottom: 0;  
 left: 0;  
 right: 0;  
 margin: auto;  
}  

### [](#子元素相对定位 "子元素相对定位")子元素相对定位

父元素设置了高度，而且父元素里面只有一个子元素，可使用子元素相对定位。

1  
2  
3  
4  
5  
6  
7  
8  
9  

.parentElement {  
 height: xxpx;  
}  
  
.childElement {  
 position: relative;  
 top: 50%;  
 transform: translateY(-50%);  
}  

### [](#CSS3-flex "CSS3 flex")CSS3 flex

如果不考虑低版本浏览器兼容性，用CSS3的flex布局就非常简单咯。

1  
2  
3  
4  

.parentElement {  
 display: flex;  
 align-items: center;  
}