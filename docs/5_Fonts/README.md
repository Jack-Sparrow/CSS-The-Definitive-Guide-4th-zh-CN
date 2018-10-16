# 字体

字体无法绝对控制，使用的字体下载失败，fallback 到用户浏览器的字体。

## Font Families
1. CSS  定义的五种字体：
    1. Serif fonts：有衬线、不等宽， Times，Georgia，New Century Schoolbook
    2. Sans-serif fonts：成比例、无衬线，Helvetica，Geneva，Verdana，Univers
    3. Monospace fonnts：不成比例，字符等宽，有或者无衬线，Courier, Courier New, Consolas, and Andale Mono
    4. Cursive fonts：模拟手写，花体/草体？Zapf Chancery，Author，Comic Sans
    5. Fantasy fonts：其他类型，不能归为上面的，装饰、显示字体，Western，Woodblock，Klingon
2. 字体名中间有空格、尾部有# $等符号，建议使用引号， 字体中有关键字（cursive 等）
~~~css
p {font-family: Times, 'Times New Roman', 'New Century Schoolbook', Georgia,'New York', 'Karrank%', serif;}
~~~

## @font-face
~~~css
@font-face {
    font-family: "SwitzeraADF";
    src: local("Switzera-Regular"),
         local("SwitzeraADF-Regular "),
         url("SwitzeraADF-Regular.otf") format("opentype"),
         url("SwitzeraADF-Regular.true") format("truetype");
 }
~~~
1. 懒加载，浏览器实现若不是这样的需要被当做bug
2. 字体需要与样式同源，除非服务器设置 Access-Control-Allow-Origin.
3. 其中的font-family是描述符descriptor不是属性，描述符定义字体名之后，插入到用户代理的字体表中；样式中字体属性的值使用该名称可以refer到该字体
4. format()，用户代理可跳过下载不支持的格式：embedded-opentype EOT，opentype OTF，svg SVG，truetype TTF，woff WOFF
5. Flash of Unstyled Content  Flash of Un-Fonted Text，浏览器加载所有资源前显示内容，字体变化可能引起重排，（e.g.字体高度差别较大）
6. local() 检查本地字体先，local可用来重命名本地字体
7.
    ~~~css
    @font-face {
        font-family: "SwitzeraADF";
        src: url("SwitzeraADF-Regular.eot");
        src: url("SwitzeraADF-Regular.eot?#iefix") format("embedded-opentype"),
             url("SwitzeraADF-Regular.woff") format("woff"),
             url("SwitzeraADF-Regular.ttf") format("truetype"),
             url("SwitzeraADF-Regular.svg#switzera_adf_regular") format("svg");
     }
     ~~~
  - 前两行fix IE6 - IE9，
  - woff： web open font format 大部分现代浏览器，
  - ttf： 大部分 iOS和Androids设备，
  - svg： 旧iOS

## font-weight
1. 100 - 900 keywords normal bold lighter bolder；基准：400 = normal，700 = bold
2. 字体本身定义的weight，就近匹配，只保证数值大的显示的weight大于等于数值小的
3. 假如只定400为基准，normal、regular等 等于 400同时Medium 等于 500，但如果只有Medium，则等于400
4. 接上条，如果500没有对应的变体，与400相同；
5. 接上条，如果没有300对应的变体，300对应到比400小一级的变体，如果也没有，与400相同；同样的方法用与200和100；
6. 接上条，如果没有600对应的变体，600对应到比500darker一级的变体，如果也没有，与500相同；同样的方法用于700，800和900；
7.  值 bolder 和 lighter，相对父级继承的weight基础上加重或减轻，若存在比父级更重/轻一级的变体，则应用该级变体；如果没有，则在父级weight数值基础上加/减100，直到900/100
8. @font-face中的font-weight可按数值指定

## font-size
1. 值larger和smaller类似font-weight的bolder和lighter
2. 约定一个 em 大小的方块，但是字符未必完全显示在方块内部，由字体本身的设计决定；baseline之间的默认距离，大部分字体的字符定义低于这个距离
3. 绝对尺寸：xx-small, x-small, small, medium, large, x-large, and xx-large
    1. css1约定1.5倍步进or 0.66
    2. css2约定1.0 - 1.2 之间
    3. css3草案重新约定各个值
4. 相对尺寸：larger和smaller
5. 百分比和em一样
6. font-size继承是继承父级的计算值（百分比累加）
7. 渲染引擎差异 OOOOOOOOOO 10px - 10.5px- 11px，不同OS上不同浏览器的不同表现
8.
    ~~~css
     p {font-size: medium;}   /* the default value */
     span {font-family: monospace; font-size: 1em;}
     ~~~
    默认medium大小为16px，设置在span上的monospace，span继承 medium，某些浏览器计算font-size的时候，参照用户performance settinng来设定实际的文字大小，一般默认是13px；使用：
    ~~~css
    p {font-size: medium;}   /* the default value */
    span {font-family: monospace, serif; font-size: 1em;}
    ~~~
    添加一个额外的serif，所有浏览器会使用medium的计算值继承给span，而不使用medium的原始值。
9. font-size可以使用长度单位，使用 pixel-sizing text 获得一致性，但确定是不总是能resize to pixel，移动设备。。。所以不推荐？
10.  自动调整尺寸
    1. 字体清晰度取决于size和x-height which referred to aspect value，as 有较大aspect value的字体随着尺寸缩小比值小的字体更清晰
    2. font-size-adjust属性：Declared font-size × (font-size-adjust value ÷ aspect value of available font) = Adjusted font-size
    3. 不同字体的 x-height 不同导致声明相同的font-size的时候，实际显示的 adjusted font-size不同，使用 font-size-adjust 设定参数，调整字体实际显示的adjusted font-size
    4. 字体的 aspect value，支持@font-face的浏览器可以直接从字体文件中取，字体文件里可能没有，用户代理会试着计算，no guarantee
    5. 使用值auto，使用浏览器计算的aspect值，none是默认值
    6. 2017年底只有Gecko支持

## font-style
1. normal italic oblique

## font-stretch
1. 字体压缩or舒展，若@font-face约定了值，可以设定，否则无效
2. 2017年底，mac和ios的safari不支持

## font-kerning
1. 字符间距的方式 AB and AW may have different separation distances
2. If a font does not contain kerning data, font-kerning will have no effect.
3. 属性letter-spacing可用于kerned字体，kerning之后由letter-spacing设定间距

## font-variant
## font-feature-settings
## font-synthesis

## font属性
1. `[[ <font-style> ‖ [ normal | small-caps ] ‖ <font-weight> ]? <font-size> [ / <line-height> ]? <font-family>] | caption | icon | menu | message-box | small-caption | status-bar`
2. font属性值的覆盖，整个是一个属性，值会一起背新值覆盖
3. 使用系统字体：`caption | icon | menu | message-box | small-caption | status-bar`

## 字体匹配
1. 用户代理创建字体数据库，列出CSS属性里的所有字体；机器上的所有字体，用户自定义字体
2. 分离元素为每个元素构建一个显示内容时必需的字体列表，基于列表选择一个显示元素的初始字体族；若完全匹配，使用该字体，否则；
3. 首先匹配 `font-stretch` 属性
4. 再匹配 `font-style`，The keyword italic is matched by any font that is labeled as either “italic” or “oblique.” If neither is available, then the match fails
5. 然后匹配 `font-weight`
6. 匹配 `font-size`，范围匹配
7. 若第二步没有匹配，找同字体族中的可选字体，若有重复步骤2
8. 如果一个普通匹配被找到，但没有显示元素需要的所有东西 then the user agent goes back to Step 3, which entails a search for another alternate font and another trip through Step 2.
9. Finally, if no match has been made and all alternate fonts have been tried, then the user agent selects the default font for the given generic font family and does the best it can to display the element correctly.
