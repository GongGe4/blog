# IE6的问题
### 选择器的问题
> IE6不支持连续类交集选择.box1.box2{}
### 盒模型的兼容性问题
> 如果不写DTD，那么IE6的盒模型是內减的，而不是外扩的。
> 解决办法，就是一定要写DTD。
> 不支持小于一个文字高度的盒子。任何浏览器都有默认字号，IE6默认字号是14px，所有小于字号的盒子都不能正常渲染，解决办法，使用_或-单独为IE6设置一个盒子的字号，这个字号比盒子小就行，比如设置0；
### 浮动的兼容性
		标准流的盒子不会钻到浮动盒子的下边，解决方法：不准使用浮动标准流这个小技巧，使用定位层级实现覆盖。
		overflow：hidden；不被支持。
		高级浏览器标准流的盒子不能被脱标的儿子撑出高度，使用overflow：hidden；可以做到，但是在IE6里边overflow：hidden；是不好使的，想做到需要触发IE6的haslayout机制。
		div{
			border：1px solid red；
			overflow:hidden;
			_zoom:1;
		}
		haslayout解释：
		IE6里面有两个儿子撑父亲的机制，一种是有hasLayout，一种是没有。所谓的layout就是布局的意思。
		zoom 总是可以触发 hasLayout。zoom这个属性，是用来控制一个元素缩放倍数的，是IE特有的属性，Chrome现在的版本也支持了。加上zoom属性的元素，都能触发这个元素的hasLayout，所以IE6就用另一种方式渲染盒子了。
		双倍margin的问题：
			连续浮动的元素，浮动的方向和margin方向相同，最后一个和开头一个都有可能出现双倍margin的现象。
		解决方法：
			1）严禁儿子踹父亲，所以这个问题根本不应该出现。
			2）如果必须要踹父亲，那就给第一个元素的margin设置为正常的一半。
		IE6中3像素bug：
			如果一个盒子浮动，一个盒子不浮动,两者并排之间会出现3像素间隙。
			解决办法：
				1）不允许出现一个浮动一个不浮动的情况。
				2）如果必须这么写，就让浮动的盒子	_margin-right: -3px;这么解决。
### 定位的兼容性问题
		IE6不支持position：fixed；固定定位。使用js模拟定位。
### 文字样式兼容
		在IE6、7、8中所有的超链接图片都会有一个蓝色的边框。
### 盒子的透明
		opacity就是透明度的意思，值是0-1之间，1就是不透明实心，0就是纯透明
		这个属性IE678不支持，要写IE自己的滤镜属性
		filter ：alpha（opacity=40）；
		盒子中的文字也会跟着透明，解决文字不透明：不要把opacity属性的盒子里边写文字，把文字单独放出去写一个绝对定位钉在一起。
### 图片透明
> jpg/jepg
			压缩格式，颜色是失真的，为了保证尺寸较小，所以有压缩算法，所以尺寸颜色是失真的。网页中照片，新闻图片，banner，焦点图片都要用jpg格式，因为图片尺寸小，没有图层，不支持透明和半透明。
> png
			不可压缩，颜色不失真，是fireworks这个软件默认保存的格式，可以有图层。在上传服务器的时候，所有的png图片一定要去掉图层，导出文件就可以去掉图层。
			jpg是压缩的，png是不可压缩的，但是同一张图片，jpg不一定比png图片尺寸小.经过对比像素点复杂的颜色渲染比较多的jpg小于png，像素点简单的，颜色单一的图片jpg大于png。
			所以在网页中，杂碎的图标，使用png，图标颜色比较单一，使用png尺寸更小，png图片支持半透明和透明，但是IE6不支持png图片的透明和半透明。
> Gif
			没有压缩与不压缩，它仅支持固定数量的颜色，可以是256种，128种，64种2种...所以它是严重的颜色失真，根本表示不全自然界的颜色。
			支持动画。
			尺寸比较小，如果不动，尺寸更小，为什么不用gif？因为严重失真。
			gif支持透明。
			gif图片在IE6没有任何兼容问题，都是支持透明不支持半透明。
			所以说工作中需要做一个透明的元素，使用gif不使用png，因为IE6不支持png的透明。
> bmp
			是Windows画图保存的图片格式，不压缩，不失真。

# require.js加载兼容
```javascript
<script src="js/require.js" defer async="true" ></script>
```
async属性表明这个文件需要异步加载，避免网页失去响应。IE不支持这个属性，只支持defer，所以把defer也写上。