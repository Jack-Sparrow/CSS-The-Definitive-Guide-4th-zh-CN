# 值和单位

## 关键字、字符串和其他文本值
1. CSS3 三个 global keywords：inherit、initial、unset
  1. inherit 也可设置默认不继承的属性
  2. initial 设置为默认值，属性没有默认值时设置为用户代理设置的值
  3. unset 当属性可继承时行为与 inherit 相同，非继承属性时行为与 initial 相同
  4.  IE 11 不支持它们，也不支持 all
  5. 属性 all 的值只能使用 global keywords
2. 字符串值，使用单引号或双引号包起来的直接引用的值，可用 \ 在行末断行，真的要插入换行符使用 \A
3. url值使用相对地址时，相对的是样式文件的位置，url和左括号之间不能插入空格
4. images：一般是 url 值，或者 image-set()，或者 gradients 渐变图片
5. 有些属性接受标识符值，用户自定义的 identifier

## 数值和百分比
1. 整数值 +- 符号，一些属性超出取值范围的时候，取距离最近的值，z-index 没有严格范围 ±1,073,741,824 (±230)
2. 数值，包含整数值和浮点值，超出范围时一些属性会失效，一些会同整数值行为
3. 百分比一般是相对于其他值的值，任何接受百分比值的属性将定义对允许的百分比值范围的任何限制，还将定义相对计算百分比的方式。
4. 分数值，标签 fr，用于 Grid Layout

## 距离
度量长度，长度都由正/负的数值加上标签（缩写的长度单位），0 不需要跟随单位

1. 绝对长度单位
  1.  in、cm、mm、q（1cm = 40q）、pt（1in = 72pt）、pc（1pc = 12pt）、px（1/96 in defined in css）
  2. CSS 2.1 / 3 建议 96  ppi，多数显示器高于此数值，打印设备假定 96px 为1inch
2. 分辨率单位，用于 Media query 和 响应式设计
  1. dpi 每英寸点数，打印纸点、LED 屋里像素、电子墨水显示器 Kindle
  2. dpcm 同上，每厘米点数
  3.  dppx 每个 CSS px 单位的点数，CSS3 约定比率 1dppx 等于 96dpi
3. 相对长度单位
  1.  em，相对于本元素 font-size 的值，当设置 font-size 属性时，em 是相对于父元素的 font-size；名称来源于小写字母 m 的宽度， 但 CSS 中并不精确等于
  2. ex，小写字母 x 的高度，不同字体有不同的 x 高度
  3.  rem，基于根元素 font-size 值，基于绝对 px 而非相对字体大小 ，html {font-size: 75%;} 得到值12px 而非 75%（假设默认 font-size 是 16px）；支持 initial 关键字的浏览器中，如果根元素没有设置字体大小，font-size: 1rem 等于 font-size: initial
4. Viewport 相对单位， CSS3 中新增，依据视口尺寸百分之一
  1. vw、vh、vmin、vmax（视口宽高中较小或较大的一个）

## 计算值
1. calc() 可进行简单四则运算，可以使用括号
2. 可使用在这些值类型允许使用的地方：长度、频率、角度、时间、百分比、数值、整数
3. `+ -` 号两边必须使用同样的值类型，`*` 号一边必须是数值，/ 号右侧必须是数值，不能 0 除
4. `+ -` 号两边必须有空格， * / 非必须
5. 规范要求用户代理必须支持最少 20 terms

## 属性值
1. `attr()` 少数 CSS 属性允许使用当前元素 HTML 属性的值：`p::before {content: "[" attr(id) "]";}`
2. `input[type="text"] {width: attr(maxlength em);}`
3. 2016 年底还都不支持

## 颜色
1. 命名颜色， 16 to 148 in CSS Color Module Level 4 base on X11 RGB
2.  RGB 和 RGBa 颜色
    1. `rgb(R, G, B)` 取值 0 - 255、0% - 100%，超出范围取上下限，浮点数值四舍五入为 0 - 255 整数
    2. `Rgba(R, G, B, a)`，a 取值 0 - 1，超出范围无效或重置为上下限，不能使用百分比
    3. 十六进制 RGB
    4. 十六进制 RGBa，2017 年底 Firefox 和 Safari 支持， Chrome 和 Opera 试验性实现
3. HSL 和 HSLa 颜色：CSS3 新增， H 色调 0 - 360、 S 饱和 0 - 100%、 L 亮度 0 - 100%
4. 颜色关键字，可使用在任何允许使用颜色值的地方
    1. `transparent` 完全透明颜色，背景颜色默认值，常用于定义渐变
    2. `currentColor` 当前元素 color 属性的值

## 角度
1. 数值 + 单位：deg 360角度、grad 400一圈、rad 弧度 2π、turn 一圈

## 时间和频率
1. 数值 + s/ms
2. 数值 + Hz/kHz 忽略大小写

## 位置
1. 取值顺序
~~~
[
  [ left | center | right | top | bottom | <percentage> | <length> ] |
  [ left | center | right | <percentage> | <length> ]
  [ top | center | bottom | <percentage> | <length> ] |
  [ center | [ left | right ] [ <percentage> | <length> ]? ] &&
  [ center | [ top | bottom ] [ <percentage> | <length> ]? ]
]
~~~
2. 如果只声明一个值，第二个值默认设置为 `center`
3. 如果声明两个值且第一个值为长度或百分比，则第一个值 always 被当做水平值
4. 如果声明四个值，必须有两个值是长度或百分比，每个值前面有一个关键字，`right 10px bottom 30px` 表示从右边向左 10px 和 从底边向上 30px
5. 如果定义三个值，与四个值一样，最后一个值默认为 0

## 自定义值
1. 以 -- 开始， 区分大小写
2. 可以 scope 覆盖
3. 记得使用 @supports() 验证哦
4. 浏览器支持尚不确定
~~~css
@supports (color: var(--custom)) {
    /* variable-dependent styles go here */
 }
 @supports (--custom: value) {
    /* alternate query pattern */
 }
html {
    --gutter: 3ch;
    --offset: 1;
 }
 ~~~
