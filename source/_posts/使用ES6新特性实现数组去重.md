---
title: 使用ES6新特性实现数组去重
tags:
  - 前端
  - 数组去重
  - ES6
excerpt: >2-

      
        <p>数组去重，对于前端开发者来说家常便饭的事，更是初学者必须掌握的知识，面试经常会考。</p>
  <p>数组去重相关的方法，网上已有很多，大多使用了ES6以前的方法，本文对于不作赘述。但随着各大浏览器对ES6的支持性越来越好，以及ES6新特性深入人心，更何况作为一名前端工程师，我们应该顺应时代潮流，推动JavaScript发展，所以，尽情地拥抱ES6吧。</p>

  <p>本文讲解使用ES6新特性实现数组去重的一种新方法，代码及其简短又高效。  </p>

  <h4 id="JavaScript代码："><a href="#JavaScript代码：" class="headerlink"
  title="JavaScript代码："></a>JavaScript代码：</h4><figure class="highlight
  javascript"><table><tr><td class="gutter"><pre><span
  class="line">1</span><br><span class="line">2</span><br><span
  class="line">3</span><br><span class="line">4</span><br></pre></td><td
  class="code"><pre><span class="line"><span
  class="comment">/*</span></span><br><span class="line"><span class="comment">*
  @param arr 传入的参数：带有重复项的数组</span></span><br><span class="line"><span
  class="comment">* */</span></span><br><span class="line"><span
  class="built_in">Array</span>.from(<span class="keyword">new</span> <span
  class="built_in">Set</span>(arr));</span><br></pre></td></tr></table></figure>
      
      
date: 2018-01-17 22:18:40
---

数组去重，对于前端开发者来说家常便饭的事，更是初学者必须掌握的知识，面试经常会考。

数组去重相关的方法，网上已有很多，大多使用了ES6以前的方法，本文对于不作赘述。但随着各大浏览器对ES6的支持性越来越好，以及ES6新特性深入人心，更何况作为一名前端工程师，我们应该顺应时代潮流，推动JavaScript发展，所以，尽情地拥抱ES6吧。

本文讲解使用ES6新特性实现数组去重的一种新方法，代码及其简短又高效。

#### [](#JavaScript代码： "JavaScript代码：")JavaScript代码：

1  
2  
3  
4  

/\*  
\* @param arr 传入的参数：带有重复项的数组  
\* \*/  
Array.from(new Set(arr));
<!-- more -->
数组去重，对于前端开发者来说家常便饭的事，更是初学者必须掌握的知识，面试经常会考。

数组去重相关的方法，网上已有很多，大多使用了ES6以前的方法，本文对于不作赘述。但随着各大浏览器对ES6的支持性越来越好，以及ES6新特性深入人心，更何况作为一名前端工程师，我们应该顺应时代潮流，推动JavaScript发展，所以，尽情地拥抱ES6吧。

本文讲解使用ES6新特性实现数组去重的一种新方法，代码及其简短又高效。

#### [](#JavaScript代码： "JavaScript代码：")JavaScript代码：

1  
2  
3  
4  

/\*  
\* @param arr 传入的参数：带有重复项的数组  
\* \*/  
Array.from(new Set(arr));  

#### [](#例子： "例子：")例子：

1  
2  
3  
4  
5  
6  
7  

/\*  
\* @param oldArr 带有重复项的旧数组  
\* @param newArr 去除重复项之后的新数组  
\* \*/  
let oldArr = \[1, 1, 1, 2, 3, 2, 4, 4, 4, 9, 9, 0, 0, NaN, NaN\];  
let newArr = Array.from(new Set(oldArr));  
console.log(newArr);  // \[1, 2, 3, 4, 9, 0, NaN\]  

#### [](#分析： "分析：")分析：

##### [](#Set对象 "Set对象")Set对象

Set对象允许存储任何类型的唯一值，无论是原始值或者是对象引用。它可以是任何类型的单个值的集合。Set中的元素只会出现一次，即Set中的元素是唯一的。  
`语法：new Set([iterable]);`  
参数：iterable，如果传递一个可迭代对象(包括 Array，Map，Set，String，TypedArray，arguments 对象等等)，它的所有元素将被添加到新的 Set中。如果不指定此参数或其值为null，则新的Set为空。

##### [](#Set对象例子 "Set对象例子")Set对象例子

1  
2  
3  

let testArr = \[0, 1, 1, 2, 3, 3, 3, 3, 4, NaN, NaN, undefined, undefined\];  
let setTestArr = new Set(testArr);  
console.log(setTestArr);  // Set(6) {1, 2, 3, 4, NaN, undefined}  

![Set对象例子](http://www.www.www)

##### [](#from对象 "from对象")from对象

Array.from()方法从一个类似数组或可迭代的对象(包括 Array，Map，Set，String，TypedArray，arguments 对象等等) 中创建一个新的数组实例。

##### [](#from对象例子 "from对象例子")from对象例子

1  
2  
3  
4  
5  
6  

let testArr = \[0, 1, 1, 2, 3, 3, 3, 3, 4, NaN, NaN, undefined, undefined\];  
let setTestArr = new Set(testArr);  
console.log(setTestArr);  // {1, 2, 3, 4, NaN, undefined}  
  
let newArr = Array.from(setTestArr);  
console.log(newArr);  // \[1, 2, 3, 4, NaN, undefined\]  

![from对象例子](http://www.www.www)

**使用ES6实现数组去重，就是如此简单，快去试试吧~**