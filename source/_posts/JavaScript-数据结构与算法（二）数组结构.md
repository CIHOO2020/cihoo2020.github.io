---
title: JavaScript 数据结构与算法（二）数组结构
tags:
  - 算法专辑
  - JavaScript
  - 数组
  - 数据结构
  - 算法
excerpt: |2-

      
        
        
          <p>几乎所有的编程语言都原生支持数组类型，因为数组是最简单的内存数据结构。<br>数组通常情况下用于存储一系列同一种数据类型的值。<br>但在 JavaScript 里，数组中可以保存不同类型的值。但我们还是要遵守最佳实践，别这么做（大多数语言都没这个能力）。</p>
  <h3 
        
      
      
date: 2020-07-21 11:48:56
---

几乎所有的编程语言都原生支持数组类型，因为数组是最简单的内存数据结构。  
数组通常情况下用于存储一系列同一种数据类型的值。  
但在 JavaScript 里，数组中可以保存不同类型的值。但我们还是要遵守最佳实践，别这么做（大多数语言都没这个能力）。
<!-- more -->
几乎所有的编程语言都原生支持数组类型，因为数组是最简单的内存数据结构。  
数组通常情况下用于存储一系列同一种数据类型的值。  
但在 JavaScript 里，数组中可以保存不同类型的值。但我们还是要遵守最佳实践，别这么做（大多数语言都没这个能力）。

### [](#创建和初始化数组 "创建和初始化数组")创建和初始化数组

*   new Array()  
    `const daysOfWeek = new Array('Sunday', 'Monday', 'Tuesday', 'Wednesday','Thursday', 'Friday', 'Saturday');`
    
*   \[\]  
    `const daysOfWeek = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];`
    

### [](#数组常见操作 "数组常见操作")数组常见操作

#### [](#添加元素 "添加元素")添加元素

*   添加一个元素到数组的最后位置 `array.push(item)`
*   在数组首位插入一个元素 `array.unshift(item)`
*   在指定索引位置插入元素 `array.splice(index, 0, item)`  
    splice() 第二个参数为 0 时，表示插入数据。
    
    1  
    2  
    3  
    4  
    
    let myArray = \[1, 2, 3\];  
    // 在 索引 0 的位置，插入 A  
    myArray.splice(0, 0, "A");  
    console.log(myArray); //--> \['A', 1, 2, 3\]  
    

#### [](#删除元素 "删除元素")删除元素

*   删除数组最后的元素 `array.pop(item)`
*   删除数组首位的元素 `array.shift(item)`
*   删除指定索引位置的元素 `array.splice(start, number)`  
    例如：
    
    1  
    2  
    3  
    4  
    
    let myArray2 = \[1, 2, 3, 4, 5\];  
    // 删除索引 4 位置起，2 个元素  
    myArray2.splice(4, 2);  
    console.log(myArray2); //--> \[1, 2, 3\]  
    

### [](#修改元素 "修改元素")修改元素

*   修改指定索引位置的元素 `array.splice(index, 1, item)`
    
    1  
    2  
    3  
    4  
    
    let myArray3 = \[1, 2, 3, 4, 5, 6\];  
    // 修改 索引 1 的位置的元素为 AA  
    myArray2.splice(1, 1, "AA");  
    console.log(myArray3); //--> \[1, "AA", 3, 4, 5, 6\]  
    
*   修改指定索引位置的几个元素 `array.splice(index, number, item)`
    
    1  
    2  
    3  
    4  
    
    let myArray4 = \[1, 2, 3, 4, 5, 6, 7\];  
    // 在 索引 2 的位置起，修改两个元素为 AA BB  
    myArray2.splice(2, 2, "AA", "BB");  
    console.log(myArray3); //--> \[1, 2, "AA", "BB", 5, 6, 7\]