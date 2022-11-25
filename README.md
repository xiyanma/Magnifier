# 放大镜实现及性能测试
HTML
简单。适合元素数量少的。React环境fiber切片执行的时候会阻塞渲染。
dom是一种中间代理模式，维护文档树、处理事件交互、样式和绘制，对开发者最友好，实现上最容易，方便调试，样式丰富，交互控件多，界面体验好，通过keyframes来实现动画也很便捷。缺点是不管你的实际应用要不要这些服务，它都是标配，没法绕过去，导致开销大，性能差，不适用于元素数量很多的情况。
canvas原生
离屏canvas方案。可以裁切图片做部分渲染。
canvas则是给开发者开放了一种直接访问底层绘图功能的接口，去除了中间代理，好处当然是更快了，坏处是什么都得自己来，交互、刷新、维护数据和渲染，此外canvas在处理大分辨率设备时比较吃力（比如地图这方面不如基于dom的svg, svg是draw，canvas是paint ），处理文本和屏幕适配方面也不太好。
three.js
用webgl协议绘制，渲染多纹理、渲染大量绘制物体的时候会比较有优势。性能上限高，放大镜有点大材小用？
webgl实际上用了canvas的一个3d渲染上下文，在绘制平面内容时，和canvas 2d相比，webgl更为直接的利用了gpu硬件，在某些场合，几乎可以摆脱cpu的限制，达到性能极致。
性能检测
测试三者的性能区别：渲染大量的精灵粒子。
1. dom
https://wow.techbrood.com/uploads/2101/perf_test/dom.html
2. canvas (2d)
https://wow.techbrood.com/uploads/2101/perf_test/canvas.html
3. webgl
https://wow.techbrood.com/uploads/2101/perf_test/webgl.html
在i7独显笔记本上，当粒子数量超过5000的时候，
dom的fps掉到10几了。
canvas勉强还有20+，
而webgl仍然保留在130+。
考虑到我们的放大镜元素并不多：设置100个元素，录制的堆内存使用情况如下：
dom：3.3MB。
[图片]
canvas：1.8MB。
[图片]
webGl：1.9MB
[图片]

降为10个元素时，dom实现 堆内存降到3.5MB。canvas和webGl无明显差异。
