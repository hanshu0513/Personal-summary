



## 你做的页面在哪些流览器测试过？这些浏览器的内核分别是什么?

***渲染引擎(rendering engine)，我们俗称内核。是浏览器的核心部件，负责把数据转换为用户在屏幕所看到的样式。***

- IE浏览器            ===> Trident内核  微软开发 也叫MSHTML
- Chrome浏览器 ===> WebKit内核  开源 2013年分离出来 创建了Blink
-  Firefox浏览器  ===>  Gecko内核
-  Safari浏览器    ===>  WebKit 内核
- 欧朋浏览器 opera ===>2013年2月以前Presto
                                           2013年2月以后 Webkit

## 每个HTML文件里开头都有个很重要的东西，Doctype，知道这是干什么的吗？	

*声明位于位于HTML文档中的第一行，处于 标签之前。告知浏览器的解析器     用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现*

## HTML5 为什么只需要写 !DOCTYPE HTML  ？ 

HTML5 不基于 SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为（让浏览器按照它们应该的方式来运行）； 

而HTML4.01基于SGML,所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。

DTD（文档类型定义）的作用是定义 XML 文档的合法构建模块。它使用一系列的合法元素来定义文档结构。


 ##  Quirks模式是什么？它和Standards（标准模式）模式有什么区别	

   Quirks模式：Quirks Mode 就是浏览器为了兼容很早之前针对旧版本浏览器设计、并未严格遵循 W3C 标准的网页而产生的一种页面渲染模式。Quirks Mode是一种浏览器（像IE，Firefox，Opera）操作模式。 从根本上说，怪异模式（也称之为兼容模式）意味着一个相对新的浏览器故意模拟许多在旧浏览器中存在的bug，特别是在IE4和IE5中

   > 区别

   - 盒模型： 
     Standards模式下中设置与一个元素的宽度和高度，指的元素的内容的宽度和高度，
     Quirks模式下，IE的宽度和高度还包含了padding和border
   - 设置行内元素的高度：
     在Standerds模式下,给<span>等行内元素设置width和height都不会生效，而在Quirks模式下会生效；
   - 设置百分比的高度：
      在Standards模式下，一个元素的高度是由其包含的内容来决定的，如果父元素没有设置百分比的高度，子元素的设置的一个百分比的高度是无效的
   - margin: 0 auto设置水平居中：
      标准的模式下是会生效的，而在诡异的模式下是不会是生效的

##  div+css的布局较table布局有什么优点？	

   > div+css

   - 符合W3C标准，代码结构清晰明了，结构、样式和行为分离，带来足够好的可维护性。
   - 布局精准，网站版面布局修改简单.
   - 加快了页面的加载速度（最重要的）
   - 节约站点所占的空间和站点的流量
   - 用只包含结构化内容的HTML代替嵌套的标签，提高另外搜索引擎对网页的搜索效率

   > table

   - 容易上手
   - 可以形成复杂的变化，简单快速
   - 比较严谨，兼容性在各大浏览器比较好

##  img的alt与title有何异同

  alt是指图片无法正常显示时的提示文字，title指的是鼠标滑过图片时的提示文字

## b与strong的区别?i与em的区别?title与h1的区别?

title属性没有明确意义只表示是个标题，H1则表示层次明确的标题，对页面信息的抓取也有很大的影响；

strong是标明重点内容,粗体强调，有语气加强的含义，使用阅读设备阅读网络时：<strong>会重读，而<B>是展示强调内容。

 i内容展示为斜体，em表示强调的文本,标签是斜体强调，更为强烈，表示内容的强调性；

##  你能描述一下渐进增强和优雅降级之间的不同吗

   > 渐进增强

   ```js
   .transition{
     -webkit-transition: all .5s;
        -moz-transition: all .5s;
          -o-transition: all .5s;
             transition: all .5s;  
   }
   ```

   针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。（从被所有浏览器支持的基本功能开始，逐步地添加那些只有新式浏览器才支持的功能，向页面添加无害于基础浏览器的额外样式和功能。当浏览器支持时，它们会自动地呈现出来并发挥作用。）

   > 优雅降级

   ```js
   .transition{ 
   　　     transition: all .5s;
   　　  -o-transition: all .5s;
     　-moz-transition: all .5s;
    -webkit-transition: all .5s;
   }
   ```

   一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。（Web站点在所有新式浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会检查以确认它们是否能正常工作。由于IE独特的盒模型布局问题，针对不同版本的IE的hack实践过优雅降级了，为那些无法支持功能的浏览器增加候选方案，使之在旧式浏览器上以某种形式降级体验却不至于完全失效。）

   #### *区别：*

   优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的、能够起作用的版本开始，并不断扩充，以适应未来环境的需要。

   ***\*渐进增强观点：\****

   渐进增强观点认为应该关注于内容本身。内容是我们建立网站的诱因，有的网站展示它，有的则收集它，有的寻求、有的操作，还有的网站甚至包含以上的种种，但相同点是他们全都涉及到内容，这使得“渐进增强”成为一种更为合理的设计范例。这也是它立即被Yahoo!所采纳并用以构建其“分级式浏览器支持（Graded Browser Support)“策略的原因所在。

   ***\*优雅降级观点：\****

   优雅降级观点认为应该针对那些最高级、最完善的浏览器来设计网站。而将那些被认为“过时”或有功能缺失的浏览器下的测试工作安排在开发周期的最后阶段，并把测试对象限定为主流浏览器（如IE、Mozilla等）的前一个版本。在这种设计范例下，旧版的浏览器被认为仅能提供“简陋却无妨（poor,but passable)”的浏览体验。你可以做一些小的调整来适应某个特定的浏览器。但由于它们并非我们所关注的焦点，因此除了修复较大的错误之外，其它的差异将被直接忽略。

##  为什么利用多个域名来存储网站资源会更有效？

   - CDN缓存更方便
   - 突破浏览器并发限制
   - Cookieless, 节省带宽，尤其是上行带宽 一般比下行要慢
   - 对于UGC的内容和主站隔离，防止不必要的安全问题。正是这个原因要求用户内容的域名必须不是自己主站的子域名，而是一个完全独立的第三方域名
   - 数据做了划分，甚至切到了不同的物理集群，通过子域名来分流比较省事. 这个可能被用的不多

## CDN缓存是什么

CDN是Content Delivery Network的简称，即“内容分发网络”的意思。 http缓存是浏览器端缓存，cdn是服务器端缓存 

## CDN的优势是什么？

- CDN节点解决了跨运营商和跨地域访问的问题，访问延时大大降低
- 大部分请求在CDN边缘节点完成，CDN起到了分流作用，减轻了源站的负载

## CDN怎么缓存？

​     和Http类似，客户端请求数据时，先从本地缓存查找，如果被请求数据没有过期，拿过来用，如果过期，就向CDN边缘节点发起请求。CDN便会检测被请求的数据是否过期，如果没有过期，就返回数据给客户端，如果过期，CDN再向源站发送请求获取新数据。和买家买货，卖家没货，卖家再进货一个道理^^。

​    CDN边缘节点缓存机制，一般都遵守http标准协议，通过http响应头中的Cache-Control和max-age的字段来设置CDN边缘节点的数据缓存时间

## 请谈一下你对网页标准和标准制定机构重要性的理解

网页标准和标准制定机构都是为了能让web发展的更‘健康’，首先约束浏览器开发者遵循统一的标准，其次约束网站开发者，这样降低开发难度，开发成本，SEO也会更好做，也不会因为滥用代码导致各种BUG、安全问题，最终提高网站易用性

## cookies，sessionStorage和localStorage的区别

cookie 是网站为了标识用户信息而存储在本地（client side）上的数据，一般会加密，cookie数据始终在同源请求中传递，即在浏览器和服务器之间来回传递，seesionstorage 和 localstorage 不会把数据发到服务器，仅在本地保存

存储大小，cookie的大小不能超过4k，sessionstorage 和 localstorage 的大小一般在5M以上，比cookie大的多，

有效时间不同，localstorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据，sessionstorage 数据在当前浏览器窗口关闭后自动删除，cookie  设置的cookie过期之前一直有效，即使窗口或者浏览器关闭

## src与href的区别

src用于替换当前元素；href用于在当前文档和引用资源之间确立联系。

src是source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置

href是Hypertext Reference的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接

## 知道的网页制作会用到的图片格式有哪些

- png：支持 256 色调色板，静态图片
- GIF：多达256种的颜色，动态图片
- jpg：静态图

## 知道什么是微格式吗？谈谈理解。	

是把语义嵌入到HTML以便有助于分离式开发而制定的一些简单约定，是兼顾人机可读性设计的数据表达方式，对Web网页进行语义注解的方法。

这种方法依托于标准的Web页面写作技术，例如，XHTML，这样引入语义信息对浏览器等所有现存的Web技术冲击最小。采用Microformat的 Web页面，在XHTML文档中给一些标签（Tag）增加一些属性（attribute），这些属性对信息的语义结构进行注解，处理XHTML文档的软件，

## 在前端构建中应该考虑微格式吗？构建中微格式的意义

- 在爬取Web内容时，能够更为准确地识别内容块的语义；
- 对内容进行操作，包括提供访问、校对，还可以将其转化成其他的相关格式，提供给外部程序和Web服务使用

## 在css/js代码上线之后开发人员经常会优化性能，从用户刷新网页开始，一次js请求一般情况下有哪些地方会有缓存处理？

- dns缓存
- cdn缓存
- 浏览器缓存
- 服务器缓存

## 一个页面上有大量的图片（大型电商网站），加载很慢，你有哪些方法优化这些图片的加载，给用户更好的体验

- 图片懒加载，在页面上的未可视区域可以添加一个滚动条事件，判断图片位置与浏览器顶端的距离与页面的距离，如果前者小于后者，优先加载。
- 如果为幻灯片、相册等，可以使用图片预加载技术，将当前展示图片的前一张和后一张优先下载
- 如果图片为css图片，可以使用CSSsprite，SVGsprite，Iconfont、Base64等技术
- 如果图片过大，可以使用特殊编码的图片，加载时会先加载一张压缩的特别厉害的缩略图，以提高用户体验
- 如果图片展示区域小于图片的真实大小，则因在服务器端根据业务需要先行进行图片压缩，图片压缩后大小与展示一致。

## 你如何理解HTML结构的语义化

- 语义化，顾名思义，就是你写的HTML结构，是用相对应的有一定语义的英文字母（标签）表示的

- 用正确的标签做正确的事情。

   html语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析;

   即使在没有样式CSS情况下也以一种文档格式显示，并且是容易阅读的;

  搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO;

  使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。



## 什么是SEO？

SEO由英文Search Engine Optimization缩写而来，中文意译为“搜索引擎优化”。

其实叫做针对搜索引擎优化更容易理解。它是指从自然搜索结果获得网站流量的技术和过程，是在了解搜索引擎自然排名机制的基础上，对网站进行内部及外部的调整优化， 改进网站在搜索引擎中的关键词自然排名，获得更多流量，从而达成网站销售及品牌建设的目标。

## 谈谈以前端角度出发做好SEO需要考虑什么

**1.维护网站，提高内容质量，保持更新**

搜索引擎会考评网站的质量，保持网站的经常更新，使网站拥有大量的、有用的、可读性强的优质信息。网站的权重会相应的提升。为了保持更新频率而去抄袭并不可取。

**2.网站结构布局优化：尽量简单、开门见山，提倡扁平化结构。**

一般而言，建立的网站结构层次越少，越容易被“蜘蛛”抓取，也就容易被收录。一般中小型网站目录结构超过三级，“蜘蛛”便不愿意往下爬，并且根据相关调查：访客如果经过跳转3次还没找到需要的信息，很可能离开。因此，三层目录结构也是体验的需要。

**3. 控制首页链接数量**

网站首页是权重最高的地方，如果首页链接太少，没有“桥”，“蜘蛛”不能继续往下爬到内页，直接影响网站收录数量。但是首页链接也不能太多，一旦太多，没有实质性的链接，很容易影响用户体验，也会降低网站首页的权重，收录效果也不好。注意链接要建立在用户的良好体验和引导用户获取信息的基础之上。

**4.导航优化**

导航应该尽量采用文字方式，也可以搭配图片导航，但是图片代码一定要进行优化，img标签必须添加“alt”和“title”属性，告诉搜索引擎导航的定位，做到即使图片未能正常显示时，用户也能看到提示文字。其次，在每一个网页上应该加上面包屑导航，好处：从用户体验方面来说，可以让用户了解当前所处的位置以及当前页面在整个网站中的位置，帮助用户很快了解网站组织形式，从而形成更好的位置感，同时提供了返回各个页面的接口，方便用户操作；对“蜘蛛”而言，能够清楚的了解网站结构，同时还增加了大量的内部链接，方便抓取，降低跳出率。

**5.控制页面的大小**

减少http请求，提高网站的加载速度。当速度很慢时，用户体验不好，留不住访客，并且一旦超时，“蜘蛛”也会离开。

**6.适量的关键词和网页描述**

关键词不宜太多也不宜太少，列举出几个页面的重要关键字即可，切记过分堆砌。网页描述要准确，精简地描述网页的内容。

1.title标题：只强调重点即可，尽量把重要的关键词放在前面，关键词不要重复出现，尽量做到每个页面的title标题中不要设置相同的内容。

![img](https://upload-images.jianshu.io/upload_images/7178954-f35e50b1538953e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/351)

2.meta keywords标签：关键词，列举出几个页面的重要关键字即可，切记过分堆砌。

3.meta description标签：网页描述，需要高度概括网页内容，切记不能太长，过分堆砌关键词，每个页面也要有所不同。

![img](https://upload-images.jianshu.io/upload_images/7178954-a914fdf0e9194bf3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/627)

4.body中的标签：尽量让代码语义化，在适当的位置使用适当的标签，用正确的标签做正确的事。让阅读源码者和“蜘蛛”都一目了然。比如：h1-h6 是用于标题类的，nav标签是用来设置页面主导航的等。

对于搜索引擎来说，最直接面对的就是网页HTML代码，如果代码写的语义化，搜索引擎就会很容易的读懂该网页要表达的意思。例如文本模块要有大标题，合理利用h1-h6，列表形式的代码使用ul或ol，重要的文字使用strong等等。总之就是要充分利用各种HTML标签完成他们本职的工作，当然要兼容IE、火狐、Chrome等主流浏览器。我们来看看著名的禅意花园网站（http://www.csszengarden.com/），在没有样式的情况下，代码非常语义化，看起来很工整，加载不同的样式之后可以随心所欲的改变页面外观。

无样式表：

![img](https://upload-images.jianshu.io/upload_images/7178954-5614369bbfa4c681.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/684)

有样式表：

![img](https://upload-images.jianshu.io/upload_images/7178954-b8679bc75dd923ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/637)

5.a标签：页内链接，要加 “title” 属性加以说明，让访客和 “蜘蛛” 知道。而外部链接，链接到其他网站的，则需要加上 el="nofollow" 属性, 告诉 “蜘蛛” 不要爬，因为一旦“蜘蛛”爬了外部链接之后，就不会再回来了。

6.img应使用 "alt" 属性加以说明。为图片加上alt属性,当网络速度很慢，或者图片地址失效的时候，就可以体现出alt属性的作用，他可以让用户在图片没有显示的时候知道这个图片的作用。

7.strong、em标签 : 需要强调时使用。strong标签在搜索引擎中能够得到高度的重视，它能突出关键词，表现重要的内容，em标签强调效果仅次于strong标签。

9.巧妙利用CSS布局，将重要内容的HTML代码放在最前面，最前面的内容被认为是最重要的，优先让“蜘蛛”读取，进行内容关键词抓取。搜索引擎抓取HTML内容是从上到下，利用这一特点，可以让主要代码优先读取，广告等不重要代码放在下边。例如，在左栏和右栏的代码不变的情况下，只需改一下样式，利用float:left;和float:right;就可以随意让两栏在展现上位置互换，这样就可以保证重要代码在最前，让爬虫最先抓取。同样也适用于多栏的情况。

![img](https://upload-images.jianshu.io/upload_images/7178954-c706c09140cc2fe7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/686)

10.谨慎使用 display：none ：对于不想显示的文字内容，应当设置z-index或设置到浏览器显示器之外。因为搜索引擎会过滤掉display:none其中的内容。

11.保留文字效果

如果需要兼顾用户体验和SEO效果，在必须用图片的地方，例如个性字体的标题，我们可以利用样式控制，让文本文字不会出现在浏览器上，但在网页代码中是有该标题的。例如这里的“电视剧分类”，为了完美还原设计图，前端工程师可以把文字做成背景图，之后用样式让html中的文字的缩进设置成足够大的负数，偏离出浏览器之外，也可以利用设置行高的方法让文字隐藏。

![img](https://upload-images.jianshu.io/upload_images/7178954-a0414f6583703518.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/231)

12.利用CSS截取字符

如果文字长度过长，可以用样式截取，设置高度，超出的部分隐藏即可。这样做的好处是让文字完整呈现给搜索引擎，同时在表现上也保证了美观。

![img](https://upload-images.jianshu.io/upload_images/7178954-57ce125f4453476a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/276)

## 有哪项方式可以对一个DOM设置它的CSS样式

-  可以使用行内样式
- 可以使用style标签
- 可以使用link引入css文件
- 可以使用js动态修改 

## CSS都有哪些选择器

- 标签选择器
- ID选择器
- 类选择器
- 后代选择器
- 子代选择器
- 组合选择器
- 交集选择器
- 相邻兄弟选择器
- 通用兄弟选择器
- 属性选择器
- 伪类选择器

## CSS中可以通过哪些属性定义，使得一个DOM元素不显示在浏览器可视范围内

- display:none
-  visibility:hidden
- width:0 height:0
- z-index:-数

##     css新增特性

- css3选择器
  -  E:last-child 匹配父元素的最后一个子元素E 
  -  E:nth-child(n)匹配父元素的第n个子元素E
  -  E:nth-child(n)css3匹配父元素的倒数第n个子元素E 
- @Font-face特性 Font-face可以用来加载字体样式，而且它还能够加载服务器端的字体文件，让客户端显示 客户端没有安装的字体。
-  圆角:border-redius:15px; 
- 多列布局(multi-column layout) 兼容性不好，还不够成熟。还不能用在实际项目中        

## transition和animation的区别

Animation和transition大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是transition需要触发一个事件才能改变属性，而animation不需要触发任何事件的情况下才会随时间改变属性值，并且transition为2帧，从from .... to，而animation可以一帧一帧的。 

## visibility=hidden, opacity=0，display:none, z-index:-1的区别

- opacity=0，该元素隐藏起来了，但不会改变页面布局，并且，如果该元素已经绑定一些事件，如click事件，那么点击该区域，也能触发点击事件的
- visibility=hidden，该元素隐藏起来了，但不会改变页面布局，但是不会触发该元素已经绑定的事件
- display=none，把元素隐藏起来，并且会改变页面布局，可以理解成在页面中把该元素删除掉一样。

## position

- absolute; 绝对定位 
- fixed  固定定位 
- static 静态定位 
- sticky  粘性定位 
- relative 相对定位 
- inherit 继承父元素 
- initial 把属性设为默认值，类似于static unset;initial 和 inherit 的组合，父级设定定位继承父级，父级不设置定位，设为默认值。

## CSS中可以通过哪些属性定义，使得一个DOM元素不显示在浏览器可视范围内 （透明）

- visibility=hidden
- opacity=0
- display:none
- z-index:-100
- 宽高为0

## 超链接访问过后hover样式就不出现的问题是什么？如何解决

被点击访问过的超链接样式不在具有hover和active了,解决方法是改变CSS属性的排列顺序: L-V-H-A（link,visited,hover,active）

## 什么是Css Hack？ie6,7,8的hack分别是什么

 针对不同的浏览器写不同的CSS code的过程，就是CSS hack 

​    background-color:blue;   /*firefox*/

​    background-color:red\9;   /*all ie*6+/

​    background-color:yellow;  /*ie8*/

​    +background-color:pink;    /*ie7*/

​    _background-color:orange;    /*ie6*/  } 

##  行内元素和块级元素的具体区别是什么？行内元素的padding和margin可设置吗？	

###### 块级元素(block)特性：

总是独占一行，表现为另起一行开始，而且其后的元素也必须另起一行显示;

宽度(width)、高度(height)、内边距(padding)和外边距(margin)都可控制;

###### 内联元素(inline)特性：

和相邻的内联元素在同一行;

宽度(width)、高度(height)、内边距的top/bottom(padding-top/padding-bottom)和外边距的top/bottom(margin-top/margin-bottom)都不可改变（也就是padding和margin的left和right是可以设置的），就是里面文字或图片的大小。

##  浏览器还有默认的天生inline-block元素（拥有内在尺寸，可设置高宽，但不会自动换行），有哪些 ？

####  < input > 、< img > 、< button > 、< texterea > 、< label > 

##  什么是外边距重叠？重叠的结果是什么

 外边距重叠就是margin-collapse 

在CSS中，相邻的两个盒子的外边距可以结合成一个单独的外边距，这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距

1、两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值

2、两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值

3、两个外边距一正一负时，折叠结果是两者的相加的和

## XHTML 1.0 提供了三种DTD声明可供选择

 1.过渡的(Transitional):要求非常宽松的DTD，它允许你继续使用HTML4.01的标识(但是要符合xhtml

的写法)。完整代码如下：

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"

"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

2.严格的(Strict):要求严格的DTD，你不能使用任何表现层的标识和属性，例如<br>。完整代码如下

：<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"

"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

3. 框架的(Frameset):专门针对框架页面设计使用的DTD，如果你的页面中包含有框架，需要采用这种

DTD。完整代码如下：

< !DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN "


"http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">

## HTML5的离线储存怎么使用，工作原理能不能解释一下？

在用户没有连网时，可以正常访问站点或应用，在用户与网络连接时更新用户机器上的缓存文件。

 

原理：HTML5的离线存储是基于一个新建的.appcache文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。

如何使用：

页面头部像下面一样加入一个manifest的属性；

在cache.manifest文件的编写离线存储的资源；

  CACHE MANIFEST

  \#v0.11

  CACHE:

   js/app.js

   css/style.css

  NETWORK:

  resourse/logo.png

  FALLBACK:

  / /offline.html

在离线状态时，操作window.applicationCache进行需求实现。

## 浏览器是怎么对HTML5的离线储存资源进行管理和加载的呢？

在线的情况下，浏览器发现html头部有manifest属性，它会请求manifest文件，如果是第一次访问app，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储。如果已经访问过app并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的manifest文件与旧的manifest文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。

离线的情况下，浏览器就直接使用离线存储的资源。

## iframe有那些缺点？

- iframe会阻塞主页面的Onload事件；

- iframe会阻塞主页面的Onload事件；

- 搜索引擎的检索程序无法解读这种页面，不利于SEO;

- iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。

使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript

动态给iframe添加src属性值，这样可以绕开以上两个问题。

## Label的作用是什么？是怎么用的？

label标签来定义表单控制间的关系,当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。

< label for="Name">Number:< /label >

<input type=“text“name="Name" id="Name"/>

< label>Date:< input type="text" name="B"/>< /label >

## 如何实现浏览器内多个标签页之间的通信?

WebSocket、也可以调用localstorge、cookies等本地存储方式，还可以使用页面的路有参数传递

localstorge另一个浏览上下文里被添加、修改或删除时，它都会触发一个事件，

我们通过监听事件，控制它的值来进行页面信息通信；

## 如何在页面上实现一个圆形的可点击区域？

map+area或者svg

border-radius

纯js实现 需要求一个点在不在圆上简单算法、获取鼠标坐标等等

## data-属性的作用是什么？

h5新增的属性    可以通过ele.dataset获取到标签上的data-*的属性 返回一个对象 

## H5新增的特性

- 新增的语义化标签

  1.   根据页面的结构，由div派生的标签 

     - section：独立内容区块，可以用h1~h6组成大纲，表示文档结构，也可以有章节、页眉、页脚或页眉的其他部分；
-  article：特殊独立区块，表示这篇页眉中的核心内容；
     -  aside：标签内容之外与标签内容相关的辅助信息；
-  header：某个区块的头部信息/标题；
     -  hgroup：头部信息/标题的补充内容；
-  footer：底部信息；
     -  nav：导航条部分信息
-  figure：独立的单元，例如某个有图片与内容的新闻块。
   
2. 媒体元素组合
  
    < figure >和 < figcaption >：< figure>为父元素，用于图片的外层，其子元素< figcaption >用来对内容进行说明。  
  
3. time标签 专门用来表示时间
  
4. datalist标签 定义选项列表 与input配合使用该元素 来定义input可能的值
  
   datalist 及其选项不会被显示出来，它仅仅是合法的输入值列表。
  
   请使用 input 元素的 list 属性来绑定 datalist。
  
   ```html
     <input id="myCar" list="cars" />
   <datalist id="cars">
       <option value="BMW">
     <option value="Ford">
       <option value="Volvo">
     </datalist>
   ```
  
  5. datails 标签  用于描述文档或文档某个部分的细节 
  
     与< summary >标签配合使用可以为 details 定义标题。标题是可见的，用户点击标题时，会显示出 details 

   ```html
     <details>
   <summary>Copyright 2011.</summary>
     <p>All pages and graphics on this web site are the property of W3School.</p>
   </details>
   ```

  6. address  定义文档或文章的作者/拥有者的联系信息  元素中的文本通常呈现为斜体。大多数浏览器会在 address 元素前后添加折行 
  
  7.  mark   定义带有记号的文本。在需要突出显示文本时使用 该 标签，默认加黄色背景 
  
8.  keygen  规定用于表单的密钥对生成器字段。（用于给表单添加公钥）当提交表单时，私钥存储在本地，公钥发送到服务器。
  
   ```html
     <form action="/example/html5/demo_form.asp" method="get">
   用户名：<input type="text" name="usr_name" />
     加密：<keygen name="security" />
           <input type="submit" />
     </form>
   ```

- 拖放 drag drop

- 画布 canvas 

- svg

- 地理定位  navigator.geolocation.getCurrentPosition 

- 存储  localStorage 和 sessionStorage  

-  Application Cache --- web 应用可进行缓存，并可在没有因特网连接时进行访问 

-  Web Workerjs线程， 

  由于 web worker 位于外部文件中，它们无法访问下例 JavaScript 对象：

  - window 对象
  
- document 对象
  
  - parent 对象
  
-  服务器发送事件 

  ```js
var source=new EventSource("demo_sse.php");
  source.onmessage=function(event)
  {
    document.getElementById("result").innerHTML+=event.data + "<br />";
    };
  ```
  
-  新增的表单类型 

  - email：必须输入邮件；
  
-  url：必须输入url地址；
  
  -  number：必须输入数值；
  
-  range：必须输入一定范围内的数值；
  -  Date Pickers：日期选择器
  -  search：搜索常规的文本域；
  -  color：颜色；
  
-  新增的表单属性 

  - autocomplete
  
- novalidate
  
-  新的input属性 

  - autocomplete
  
- autofocus
  
  - form
  
- form overrides (formaction, formenctype, formmethod, formnovalidate, formtarget)
  - height 和 width
  - list
  - min, max 和 step
  - multiple
  - pattern (regexp)
  - placeholder
  - required
  
- 媒体标签

  - 视频
  
- audio：音频
  
- embed：嵌入内容（包括各种媒体），Midi、Wav、AU、MP3、Flash、AIFF等。 
  
- 总结新增属性

  - manifest属性：定义页面需要用到的离线应用文件，一般放在<html>标签里
  - charset属性：meta属性之一,定义页面的字符集
  - sizes属性： <link>新增属性，当link的rel="icon"时，用以设置图标大小
  - base属性: <base href="http://localhost/" target="_blank">表示当在新窗口打开一个页面时，会将href中的内容作为前缀添加到地址前
  - defer属性：script标签属性，表示脚本加载完毕后，只有当页面也加载完毕才执行（推迟执行）
  - async属性：script标签属性，脚本加载完毕后马上执行（运行过程中浏览器会解析下面的内容），即使页面还没有加载完毕（异步执行）
  - media属性： <a>元素属性：表示对何种设备进行优化
  - hreflang属性： <a>的属性，表示超链接指向的网址使用的语言
  - ref属性：<a>的属性,定义超链接是否是外部链接
  - reversed属性:<ol>的属性，定义序号是否倒叙
  - start属性：<ol>的属性，定义序号的起始值
  - scoped属性：内嵌CSS样式的属性，定义该样式只局限于拥有该内嵌样式的元素，适用于单页开发

  

  
