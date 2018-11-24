# Code-Drawing 码绘VS手绘
A comparison of Code-drawing ang Hand-drawing
## 绘图结果
![image](https://github.com/zzh97228/code-draw/blob/master/tree.png)
## "运动"主题绘图作业链接
####    [https://github.com/zzh97228/MotionalPainting](https://github.com/zzh97228/MotionalPainting)
## 技巧分析
-   递归生成子树，并变换树枝的长度和角度
```ecmascript 6
/**
 * 递归调用并画出树枝和树干
 * @param len
 * @param proportion
 */
function branch(len,proportion) {
    //起始线坐标及长度
    line(0, 0, 0, -len);
    translate(0, -len);
    //控制最小4个分叉点
    if (len > 4) {
        //递归画出右子树
        push();
        rotate(angle);
        //子树枝与父树枝的长度比例
        branch(len * proportion,proportion);
        pop();
        //递归画出左子树
        push();
        rotate(-angle);
        branch(len * proportion,proportion);
        pop();
    }
}
```
-   添加滑块可实时变更树枝的分叉角度和递归次数
```ecmascript 6
function draw() {
    background(51);
    //滑块控制展开角度
    angle = slider.value();
    //滑块控制树的深度
    depth=depth_slider.value()
    proportion=proportion_slider.value()
    stroke(255);
    translate(320, height);
    branch(depth,proportion);
}
```
## 对于码绘和手绘的异同分析
-   绘图思路：
互动媒体技术第一堂课的时候老师展示了许多分形几何的内容，而这些内容大多是用代码绘画而成的，同时我对数学方面的知识又十分感兴趣，因此决定试着用代码绘制出几何学上的分形图。
    相同点：不管在代码编写还是手绘操作上，整个过程都需要遵循几何定律，即以固定角度岔开父节点两条树枝，这种递归实现绘图的方法。从代码的逻辑结构上从第一个节点递归，而在手绘上也同样需要从第一个下笔点画出分支，之后将下一节点作为父节点。
    差异点：手绘的时候要考虑到整体的结构，要避免整棵树画歪，而代码绘画时则只需要考虑整体逻辑，其他API调用全靠P5.js
-   绘图技术：
相同点：都需要自己不断尝试，得出一个最符合数学美感的分形角度。
差异点：手绘的时候真的是感觉累死，尺规作图像是在画工程图，在实际操作中因为我要取上段树枝的2/3长度来创建下一级树枝，尺子的精度却不够，一度想放弃绘画，但是最终这种严谨的美感还是挺让人欣慰的。而在代码编写时却相对轻松，活学活用了以前数据结构的知识，递归调用二叉树的构建方法，所要做的无非就是更改树的深度已经二叉的角度。
-   创作体验：
相同点：由于我本人对几何比较感兴趣，所以在绘图的过程中往往更能静得下心去做，不得不说，不管码绘还是手绘都是一件令人享受的过程。
差异点：如果单从时间上来讲的画，手绘耗时太长，而且画出来的图也不尽美观，码绘调用API简单，但是创的图形规律性很强，有一种特别严谨的美感，但会少了点手工的成就感，适用于高效和工程上的作图。
 -  创作偏好：
相同点：手绘中用线条，码绘中也同样用的是线条
差异点：手绘用笔，码绘靠逻辑和对p5.js的熟练运用程度
