## 百分比流式布局(慢慢买官网用的就是这种思路)
- 遇到等分就用百分比
- 左浮动 + 右浮动(导航部分实现、折扣推荐导航部分) --> 适合于所有的元素宽度固定的
- 左浮动 + padding挤(见超值折扣推荐内容部分) 本质上元素大小在任何尺寸下面都是一致，改变的其实是元素与元素之间的间距大小 --> 适合一个元素宽度固定，另一个宽度自适应

### 采用百分比流式布局的网站
- http://m.duba.com/
- http://m.lagou.com/
- http://m.jd.com/

### 常用的一些技巧
多列等分 -> 百分比等分
左侧固定宽度 + 右侧自适应宽度 
    思路一 -> 左侧左浮动+右侧环绕在右侧
    思路二 -> 父级给padding-left，预留出来左侧区域的宽度，左侧用绝对定位上去，右侧用百分百宽度
左侧自适应 + 右侧固定宽度
    思路一 -> 左侧用百分百宽度,右侧用绝对定位上去
左右固定宽度 + 中间自适应
    思路一 -> 左侧左浮动 + 中间百分之百（中间部分再分为左侧百分之百+右侧右浮动）
    思路二 -> 左侧左浮动 + 中间百分之百 + 右侧右浮动 （负margin法减掉左右侧）
    思路三 -> 左右绝对定位 + 中间百分之百（父元素padding-left,padding-right预留左右侧的位置）
左中右全自适应 -> 全部用百分比
font-size、padding,margin,height直接量像素（当然像当当网是把所有的尺寸用了rem来写）
小的地方可以用display:inline-block;让几个容器放在一排
小图标之类的，可以考虑用::before,::after来实现
小的图标用svg,base64或者Iconfont（字蛛 http://font-spider.org/）
 另类圣杯布局（研究国美移动端的时候看到的）： .left+.right+.center -> 左左浮，右右浮，中间用margin正值来减宽度

 #### 百分比布局的缺点
 在大屏幕的手机下显示效果会变成有些页面元素宽度被拉的很长，但是高度还是和原来一样，实际显示非常的不协调，这就是流式布局的最致命的缺点，往往只有几个尺寸的手机下看到的效果是令人满意的，其实很多视觉设计师应该无法接受这种效果，因为他们的设计图在大屏幕手机下看到的效果相当于是被横向拉长来一样。流式布局并不是最理想的实现方式，通过大量的百分比布局，会经常出现许多兼容性的问题，还有就是对设计有很多的限制，因为他们在设计之初就需要考虑流式布局对元素造成的影响，只能设计横向拉伸的元素布局，设计的时候存在很多局限性。


### rem布局

#### 理论说明
- https://github.com/amfe/article/issues/17

#### 推荐网站
- https://m.taobao.com/


#### 更专业的玩法
https://github.com/amfe/lib-flexible

注意事项：
```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```
如果加上这句，最终渲染出来整体宽度是320px,dpr为1，如果没有加，则渲染出来整体宽度是640,dpr为2，其实本质上是没有区别的



理论上每张图在各个手机上是比例一致的，我们很容易可以得出如下的结论：
由于：640下某元素容器的宽度 /  = 750下某元素容器的宽度 / 750
我们知道：(640下某元素容器的宽度 / 640) * 640 最终得到的还是原始的某元素容器的大小
所以，我们可以让html的font-size为640px，这样的话，实际上1px就相当于是 1 / 640 rem;
但这样算出来的值太小了，所以，我们可以让html为64px,这样，100px就相当于是100 / 64rem;
由于我们这个项目并没有psd图，我们的策略可以这样：
把慢慢买官网缩放到320px左右下面，然后比如量出来的是50px,其实相当于是100px,所以，相当于50 / 32 rem;
所以，我们把基准调成是32
这样，所有的尺寸可以用它的尺寸除以32得到rem的尺寸

### 注意事项
把慢慢买的项目切换到320尺寸下面

## 编辑器设置
### sublime
- rem-unit
```json
{
    "exts": [".html",".css", ".scss", ".less", ".sass", ".styl"], //启用此插件的文件类型。
    "fontsize": 32,//html元素font-size值，默认为16。
    "precision": 8,//px转rem的小数部分的最大长度，默认为8.
    "leadingzero": false,//是否保留前导0，默认不保留。
}
```

### vsCode设置
- px to rem

### webstorm设置
暂时没发现合适的插件

### less中字体自动生成mixin写法
#### 如何定义
```less
.ft(@fontSize){
    font-size: @fontSize;
    [data-dpr="2"] & {
        font-size: @fontSize * 2;
    }
    [data-dpr="3"] & {
        font-size: @fontSize * 3;
    }
}
```

#### 如何使用
```less
.ft(16px);
```

### sass中字体自动生成mixin写法
#### 如何定义
```scss
@mixin font-dpr($font-size){
    font-size: $font-size;
    [data-dpr="2"] & {
        font-size: $font-size * 2;
    }
    [data-dpr="3"] & {
        font-size: $font-size * 3;
    }
}
```

#### 如何使用
```scss
@include font-dpr(12px);
```

### 考拉如何使用(不需要安装任何插件最简单的使用less,sass的方法)
- [官网](http://koala-app.com/index-zh.html) 
