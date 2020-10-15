---
title: Angular Material Dialog 拖拽功能的实现
tags:
  - Angular
  - Angular
  - Material
excerpt: >2-

      
        <h4 id="版本"><a href="#版本" class="headerlink" title="版本"></a>版本</h4><p>本案例使用的 Angular 7.x 版本，Material 7.x 版本，但不限于此版本。</p>
  <h4 id="新建Angular指令"><a href="#新建Angular指令" class="headerlink"
  title="新建Angular指令"></a>新建Angular指令</h4><p>新建指令命令：<br><code>ng generate
  directive dialog-draggable</code></p>
      
      
date: 2019-06-28 16:38:51
---

#### [](#版本 "版本")版本

本案例使用的 Angular 7.x 版本，Material 7.x 版本，但不限于此版本。

#### [](#新建Angular指令 "新建Angular指令")新建Angular指令

新建指令命令：  
`ng generate directive dialog-draggable`
<!-- more -->
#### [](#版本 "版本")版本

本案例使用的 Angular 7.x 版本，Material 7.x 版本，但不限于此版本。

#### [](#新建Angular指令 "新建Angular指令")新建Angular指令

新建指令命令：  
`ng generate directive dialog-draggable`

指令中拖拽功能代码实现：

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
13  
14  
15  
16  
17  
18  
19  
20  
21  
22  
23  
24  
25  
26  
27  
28  
29  
30  
31  
32  
33  
34  
35  
36  
37  
38  
39  
40  
41  
42  
43  
44  
45  
46  
47  
48  
49  
50  
51  
52  
53  
54  
55  
56  
57  
58  
59  
60  
61  
62  
63  
64  
65  
66  
67  
68  
69  
70  
71  
72  
73  
74  
75  
76  
77  
78  
79  
80  

import { Directive, HostListener, OnInit } from '@angular/core';  
import { MatDialogContainer, MatDialogRef } from '@angular/material';  
import { Subscription, fromEvent } from 'rxjs';  
import { takeUntil } from 'rxjs/operators';  
  
export interface Position {  
 x: number;  
 y: number;  
}  
  
@Directive({  
 selector: '\[dialog-draggable\]'  
})  
export class DialogDraggableDirective implements OnInit {  
  
 private \_subscription: Subscription;  
  
 mouseStart: Position;  
  
 mouseDelta: Position;  
  
 offset: Position;  
  
 constructor(  
 private matDialogRef: MatDialogRef<any\>,  
 private container: MatDialogContainer  
 ) {  
 }  
  
 ngOnInit() {  
 this.offset = this.\_getOffset();  
 this.\_updatePosition(this.offset.y, this.offset.x);  
 }  
  
 @HostListener('mousedown', \['$event'\])  
 onMouseDown(event: MouseEvent) {  
 this.mouseStart = { x: event.pageX, y: event.pageY };  
  
 const mouseup$ = fromEvent(document, 'mouseup');  
 this.\_subscription = mouseup$.subscribe(() => this.onMouseup());  
  
 const mousemove$ = fromEvent(document, 'mousemove')  
 .pipe(takeUntil(mouseup$))  
 .subscribe((e: MouseEvent) => this.onMouseMove(e));  
  
 this.\_subscription.add(mousemove$);  
 }  
  
 onMouseMove(event: MouseEvent) {  
 this.mouseDelta = { x: (event.pageX - this.mouseStart.x), y: (event.pageY - this.mouseStart.y) };  
  
 this.\_updatePosition(this.offset.y + this.mouseDelta.y, this.offset.x + this.mouseDelta.x);  
 }  
  
 onMouseup() {  
 if (this.\_subscription) {  
 this.\_subscription.unsubscribe();  
 this.\_subscription = undefined;  
 }  
 if (this.mouseDelta) {  
 this.offset.x += this.mouseDelta.x;  
 this.offset.y += this.mouseDelta.y;  
 }  
 }  
  
 private \_updatePosition(top: number, left: number) {  
 this.matDialogRef.updatePosition({  
 top: top + 'px',  
 left: left + 'px'  
 });  
 }  
  
 private \_getOffset(): Position {  
 const box = this.container\['\_elementRef'\].nativeElement.getBoundingClientRect();  
 return {  
 x: box.left + pageXOffset,  
 y: box.top + pageYOffset  
 };  
 }  
}  

在全局的CSS样式文件添加下面的CSS代码：

1  
2  
3  
4  
5  
6  
7  

\[dialog-draggable\] {  
 margin: -24px -24px 20px -24px !important;  
 padding: 10px 24px;  
 background: #283593 !important;  
 color: #fff;  
 cursor: move;  
}  

#### [](#如何使用 "如何使用")如何使用

在Dialog组件中使用dialog-draggable指令即可实现窗口拖拽，如下

1  
2  
3  

<h2 mat-dialog-title dialog-draggable\>  
 Angular Material Dialog Draggable  
</h2\>