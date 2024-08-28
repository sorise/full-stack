## [HTML 标签](#)
> **介绍**：HTML 5以HTML 4为基础，对HTML 4进行了大量修改。

-----
* [1. 排版标签](#1-排版标签)
* [2. 文本标签](#2-文本标签)
* [3. 媒体元素标签](#3-媒体元素标签)
* [4. 表格标签](#4-表格标签)
* [5. 表单标签](#5-表单标签)
* [6. 链接标签](#6-链接标签)
* [7. 面板标签](#7-面板标签)
* [8. 列表标签](#8-列表标签)
* [9. 功能标签](#9-功能标签)
* [10. 大内容 canvas](#10-大内容-canvas)
* [11. iframe](#11-iframe)

-----
### [1. 排版标签](#)

#### [1.1 div 块级元素](#)
`<div>` HTML 元素是流式内容的通用容器。它对内容或布局没有影响。除非以某种方式使用 CSS 对其进行样式设置（例如，直接应用样式，或者对其父元素应用某种布局模型，如弹性盒子），否则它对内容或布局没有影响。
* 这个元素包含全局属性。

```html
<div class="warning">
  <img src="/media/examples/leopard.jpg" alt="An intimidating leopard." />
  <p>Beware of the leopard</p>
</div>

<div class="shadowbox">
    <p>这是一张非常有趣的说明，陈列在一个可爱的影盒里。</p>
</div>
```
只有当没有其他合适的语义元素（例如 `<article>` 或 `<nav>`）时，才应使用 `<div>` 元素。

#### [1.2 html5语义标签](#)
HTML 语义标签（Semantic HTML Tags）是指那些不仅用于布局或样式，还能传达文档内容含义的标签。这些标签有助于搜索引擎、屏幕阅读器和其他工具理解网页的结构和内容，增强可访问性和搜索引擎优化 (SEO)。

以下是一些常见的 **HTML 语义标签**及其用途：
* `<header>`：定义页面或区域的头部内容，通常包含导航链接、标志、标题等。
* `<nav>`：定义导航链接的部分。
* `<main>`：定义文档主体内容的主要部分，通常是页面中独一无二的主要内容。
* `<article>`：定义独立的内容块，适用于博客文章、新闻报道等。
* `<section>`：定义文档中的某个部分，通常包含一组主题相关的内容。
* `<aside>`：定义与页面内容相关的附属内容，通常用于侧边栏或广告等。
* `<footer>`：定义页面或区域的底部内容，通常包含版权信息、联系信息等。
* `<figure>`：用于包含图像、图表、照片或其他媒体内容。通常与 `<figcaption>` 一起使用。
* `<figcaption>`：为 `<figure>` 元素中的内容提供说明或标题。
* `<mark>`：标记文本的一部分，表示重点或高亮内容。
* `<time>`：表示时间或日期。
* `<address>`：定义联系信息，通常用于提供作者或所有者的地址。
* `<code>`：表示计算机代码片段。
* `<summary>`：用于 `<details>` 元素中，为可展开的详细内容提供标题或概要。
* `<details>`：定义用户可以展开或隐藏的额外细节。

```html
<body>
    <header></header>
    <nav></nav>
    <main>
        <article>
            <section></section>
            <section>
                <figure>
                    <figcaption>RPC</figcaption>
                    <p>Remote Process Call</p>
                </figure>
            </section>
        </article>
        <aside></aside>
    </main>
    <footer>
        <address></address>
    </footer>
</body>
```

只允许`“<br/>”`的书写形式

#### [1.1 main](#)
main元素表示网页中的主要内容。主内容区域意指与网页标题或应用程序中本页面主要功能直接相关或进行扩展的内容,**本质上等同于div**。

```html
<main></main>
```

#### [1.2 header](#)
header元素表示页面中一个内容区块或整个页面的标题,**本质上等同于div**。

#### [1.3 nav](#)
nav元素表示页面中导航链接的部分,**本质上等同于div**。

#### [1.4 article](#)
article元素article元素表示页面中的一块与上下文不相关的独立内容，譬如博客中的一篇文章或报纸中的一篇文章,**本质上等同于div**。

```html
<article>一篇文章</article>
```
#### [1.5 section](#)
**section元素语义表示** 页面中的一个内容区块，比如章节、页眉、页脚或页面中的其他部分。
它可以与h1、h2、h3、h4、h5、h6元素结合使用，标示文档结构,**本质上等同于div**。 
```html
<section>一个章节</section>
```

#### [1.6 footer](#)
footer元素表示整个页面或页面中一个内容区块的脚注。一般来说，它会包含创作者的姓名、创作日期以及创作者联系信息。

```html
<footer></footer>
```

#### [1.7 figure、figcaption](#)
元素代表一段独立的内容，可能包含 `<figcaption>` 元素定义的说明元素。该插图、标题和其中的内容通常作为一个独立的引用单元。

```html
<figure>
  <img src="favicon-192x192.png" alt="The beautiful LTC logo." />
  <figcaption>My Logo</figcaption>
</figure>

<figure>
    <figcaption><b>Edsger Dijkstra:</b></figcaption>
    <blockquote>
        If debugging is the process of removing software bugs, then programming must
        be the process of putting them in.
    </blockquote>
</figure>

<figure>
    <p style="white-space:pre">
        Bid me discourse, I will enchant thine ear, Or like a fairy trip upon the
        green, Or, like a nymph, with long dishevelled hair, Dance on the sands, and
        yet no footing seen: Love is a spirit all compact of fire, Not gross to
        sink, but light, and will aspire.
    </p>
    <figcaption><cite>Venus and Adonis</cite>, by William Shakespeare</figcaption>
</figure>
```

#### [1.8 address](#)
`<address>` HTML 元素表示其包含的 HTML 内容提供了与个人、团体或组织联系的信息,**本质上等同于div**。

```html
<address>
  你可以通过
  <a href="http://www.example.com/contact">www.example.com</a>
  <br />
  与作者联系。如果你发现了任何错误，请<a href="mailto:webmaster@example.com">联系网站管理员</a>。
  <br />
  你也可以前来访问：美国加利福尼亚州山景城伊芙琳大道东 331 号 Mozilla 基金会，邮编：94041
</address>
```

#### [1.9 aside](#)
HTML `<aside>` 元素表示一个和其余页面内容几乎无关的部分，被认为是独立于该内容的一部分并且可以被单独的拆分
出来而不会使整体受影响。其通常表现为侧边栏或者标注框（call-out boxes）,**本质上等同于div**。

```html
<aside>
  <p>The Rough-skinned Newt defends itself with a deadly neurotoxin.</p>
</aside>
```

#### [1.10 hgroup](#)
`<hgroup>` HTML 元素代表文档标题和与标题相关联的内容，它将一个 `<h1>–<h6>` 元素与一个或多个 `<p>` 元素组合在一起。

```html
<style>
  hgroup {
    text-align: right;
    padding-right: 16px;
    border-right: 10px solid #00c8d7;
  }
  hgroup h1 { margin-bottom: 0; }
  hgroup p { margin: 0;font-weight: bold;}
</style>

<hgroup>
  <h1>Frankenstein</h1>
  <p>Or: The Modern Prometheus</p>
</hgroup>
<p>
  Victor Frankenstein, a Swiss scientist, has a great ambition: to create intelligent life. But when his creature first
  stirs, he realizes he has made a monster. A monster which, abandoned by his master and shunned by everyone who sees
  it, follows Dr Frankenstein to the very ends of the earth.
</p>
```

### [2. 文本标签](#)
HTML 的主要工作之一是赋予文本结构，使浏览器能够按照开发者的意图显示 HTML 文档。本文解释了 HTML 如何通过添加标题和段落、强调单词、创建列表等方式来构造文本。

学习如何用标记（段落、标题、列表、强调、引用等）来建立页面的基本文本结构和文本内容。

| 标签 | 介绍        | 备注        |
|:---|:----------|:----------|
| p  | 段落标签、块级元素 | 语义为一个文字段落 |

#### [2.1 段落标签 p、div](#)
在 HTML 中，每个段落是通过 <p> 元素标签进行定义的，比如下面这样, 
```html
<p>我是一个段落，可以放下一段文字。</p>
<div>我是一个段落，可以放下一段文字。</div>
```
段落是块级元素，如果在关闭的`</p>` 标签之前解析了另一个块级元素，则该标签将自动关闭, html5之后这个元素仅包含全局属性。

> 备注：`<p>` 标签的 align 属性已被弃用，请不要使用。

p标签本身和div标签几乎没有什么区别，指示p更具有语义，表示一个段落，而div表示表示一个盒子。

`<div>` HTML 元素是流式内容的通用容器。它对内容或布局没有影响,它内部还可以存放一个p标签。
```html
<div>
  <p>这里可以是任何内容。例如 &lt;p&gt;、&lt;table&gt;。你说什么就是什么！</p>
</div>
```
#### [2.2 标题标签 h](#) 
每个标题（Heading）都必须被包裹在一个标题元素中：

一共有六种标题元素标签——h1、h2、h3、h4、h5 和 h6。每个元素代表文档中不同级别的内容：
```html
<h1>这是一个一级标题</h1>
<h2>这是一个二级标题</h2>
<h3>这是一个三级标题</h3>
<h4>这是一个四级标题</h4>
<h5>这是一个五级标题</h5>
<h6>这是一个六级标题</h6>
```
hi 元素也是一个语义元素，它所包裹的文本具有“页面上的顶级标题”的作用（或意义）。


#### [2.3 em、strong、small、span、del、ins、sub、sup、mark、addr、dfn](#)

| 标签     | 介绍        | 备注                                                 |
|:-------|:----------|:---------------------------------------------------|
| em     | 文字修饰，行内元素 | 着重阅读，效果为**斜体**                                     |
| strong | 文字修饰，行内元素 | 十分重要的内容，效果为**加粗**                                  |
| small | 文字修饰，行内元素 | 十分重要的内容，效果为**变细小**                                 |
| span   | 文字修饰，行内元素 | 没有语义，通用包谷内容的容器                                     |
| del    | 文字修饰，行内元素 | 没有语义，文件增加中划线                                       |
| ins    | 文字修饰，行内元素 | 没有语义，文件增加下划线                                       |
| sub    | 文字修饰，行内元素 | 下标内容 水为：H<sub>2</sub>O                             |
| sup    | 文字修饰，行内元素 | 上标内容 一共要2 <sup>12</sup> 升水。                        |
| mark    | 文字修饰，行内元素 | 表示为引用或符号目的而标记或突出显示的文本，这是由于标记的段落在封闭上下文中的相关性或重要性造成的。 |
|abbr| 文字修饰，行内元素  | 元素表示一个缩写词或首字母缩略词,一般配合 **title** 属性使用。              |
|dfn|文字修饰，行内元素| 表示术语的一个定义。<dfn> 元素有一个 title 属性，那么该术语的值就是该属性的值。     |
```html
<p>
    <span>无修饰内容</span> 
    <em>斜体</em>
    <strong>内容加粗</strong>
</p>
<p>
    <span>商品原价为</span> <del>999</del>, 限时秒杀：<ins>199</ins>
</p>
<P>
    水为：H<sub>2</sub>O, 一共要2 <sup>12</sup> 升水。
</P>
<p>
    &lt;mark&gt; 元素用于 <mark>高亮</mark> 文本
</p>
<p>
    <p>你可以用 <abbr title="邮政特快专递服务">EMS</abbr> 把这个包裹寄给我。</p>
</p>
<dl>
    <!-- Define "World-Wide Web" and reference definition for "the Internet" -->
    <dt>
        <dfn>
            <abbr title="World-Wide Web">WWW</abbr>
        </dfn>
    </dt>
    <dd>
        The World-Wide Web (WWW) is a system of interlinked hypertext documents
        accessed on <a href="#def-internet">the Internet</a>.
    </dd>
</dl>
```
**显示效果**：
<p>
    <span>无修饰内容</span> 
    <em>斜体</em>
    <strong>内容加粗</strong>
</p>
<p>
    <span>商品原价为</span> <del>999</del>, 限时秒杀：<ins>199</ins>
</p>
<P>
    水为：H<sub>2</sub>O, 一共要2 <sup>12</sup> 升水。
</P>
<p>&lt;mark&gt; 元素用于 <mark>高亮</mark> 文本</p>
<p>
    <p>你可以用 <abbr title="邮政特快专递服务">EMS</abbr> 把这个包裹寄给我。
</p>
<dl>
  <!-- Define "World-Wide Web" and reference definition for "the Internet" -->
  <dt>
    <dfn>
      <abbr title="World-Wide Web">WWW</abbr>
    </dfn>
  </dt>
  <dd>
    The World-Wide Web (WWW) is a system of interlinked hypertext documents
    accessed on <a href="#def-internet">the Internet</a>.
  </dd>
</dl>


#### [2.4 不再推荐的标签](#)

| 标签 | 介绍        | 备注          |
|:---|:----------|:------------|
| s  | 文字修饰，行内元素 | 使用删除线来渲染文本。 |
| b  | 文字修饰，行内元素 | 仍然以粗体显示文本。  |
| i  | 文字修饰，行内元素 | 仍然以斜体显示文本。 |
| u  | 文字修饰，行内元素 | 默认情况下渲染为一个实线下划线，可以用 CSS 替换。 |

HTML `<s>` 元素 使用删除线来渲染文本。使用 `<s>` 元素来表示不再相关，或者不再准确的事情。但是当表示文档编辑时，不提倡使用 `<s>` ；为此，提倡使用 `<del>` 和 `<ins>` 元素。

```html
<s>Today's Special: Salmon</s> SOLD OUT<br>
```
**显示效果**：

<s>Today's Special: Salmon</s> SOLD OUT<br>

HTML `<b>` 元素用于吸引读者注意元素内容，而这些内容本身并不具有特别重要性。它以前被称为粗体元素，大多数浏览器
仍然以粗体显示文本,应该使用 CSS font-weight 属性。

```html
<p>
  The Latin phrase <i class="latin">Veni, vidi, vici</i> is often mentioned in
  music, art, and literature
</p>
<p>This paragraph includes a <u class="spelling">wrnogly</u> spelled word.</p>
```

<p>
  The Latin phrase <i class="latin">Veni, vidi, vici</i> is often mentioned in
  music, art, and literature
</p>
<p>This paragraph includes a <u class="spelling">wrnogly</u> spelled word.</p>

#### [2.5 code、kbd、samp、var](#)

| 标签     | 介绍        | 备注                        |
|:-------|:----------|:--------------------------|
| code   | 文字修饰，行内元素 | 表示内容为一段代码。                |
| kbd   | 文字修饰，行内元素 | 用于表示用户输入。                 |
| samp   | 文字修饰，行内元素 | 元素用于标识计算机程序输出。            |
| var   | 文字修饰，行内元素 | 元素表示数学表达式或编程上下文中的变量名称，斜体。 |
```html
<p><code>console.assert(me.age > 18);</code></p>
<p>Save the document by pressing <kbd>Ctrl</kbd> + <kbd>S</kbd></p>

<p>Regular text. <samp>This is sample text.</samp> Regular text.</p>

<p>
  The volume of a box is <var>l</var> × <var>w</var> × <var>h</var>, where <var>l</var> represents the length,
  <var>w</var> the width and <var>h</var> the height of the box.
</p>
```
**显示效果**：

<p><code>console.assert(me.age > 18);</code></p>
<p>Save the document by pressing <kbd>Ctrl</kbd> + <kbd>S</kbd></p>
<p>Regular text. <samp>This is sample text.</samp> Regular text.</p>

<p>
  The volume of a box is <var>l</var> × <var>w</var> × <var>h</var>, where <var>l</var> represents the length,
  <var>w</var> the width and <var>h</var> the height of the box.
</p>


#### [2.6 bdi、bdo、wbr](#)

| 标签  | 介绍        | 备注                                                                       |
|:----|:----------|:-------------------------------------------------------------------------|
| bdi | 文字修饰，行内元素 | 主要用于防止文本在不同书写方向的混合情况下出现显示问题。|
| bdo | 文字修饰，行内元素 | 元素覆盖了当前文本的方向，使文本以不同的方向渲染出来。  |
| wbr | 文字修饰，行内元素 | 标签在 HTML 中用于在文本中添加可能的换行机会  |

**bdi 标签** 中用于双向文本隔离的标签。它的全称是 "Bidirectional Isolation"。

具体来说，当你在一个方向为从左到右（LTR）的文本中插入一个方向为从右到左（RTL）的文本（例如阿拉伯语或希伯来语）时，如果
不使用 `<bdi>` 标签，可能会导致混乱的显示。例如，标点符号和数字的位置可能会出现错位。使用 `<bdi>` 标签可以确保插
入的文本在文档中按照其自然的书写方向显示，而不会影响周围的内容。

**bdo属性**: 
* **dir** 文本在此元素内容中渲染的方向。可能的值有：
  * ltr：指示文本应从左到右绘制。
  * rtl：指示文本应从右到左绘制。
```html
<p>该文本应从左到右绘制。</p>
<p><bdo dir="rtl">该文本应从右到左绘制。</bdo></p>
```

**wbr** 标签在 HTML 中用于在文本中添加可能的换行机会。它的全称是 "Word Break Opportunity"。
当浏览器遇到 `<wbr>` 标签时，如果文本需要换行，它会在这个位置进行断行；如果不需要换行，则忽略它。

```html
<p>这是一个非常长的单词：supercalifragilisticexpiali<wbr>docious。</p>
```
在上面的例子中，`<wbr>` 标签插入在单词 "supercalifragilisticexpialidocious" 的中间。
如果该单词在某些屏幕上超出了容器的宽度，浏览器会在 `<wbr>` 标签所在的位置将其分成两行显示。

#### [2.6 q、blockquote、pre](#)
语义标签罢了，实际效果完全可以使用div来实现。

| 标签  | 介绍        | 备注  |
|:----|:----------|:----------------------------------------------------|
| q   | 行内元素 | 这个标签是用来引用短的文本，加双引号，对于长的文本的引用请使用 `<blockquote>` 替代。 |
| blockquote | 块级元素      | 代表其中的文字是引用内容，通常在渲染时，这部分的内容会有一定的缩进。。                 |
| cite  | 文字修饰，行内元素 | 表示一个作品的引用，且必须包含作品的标题。 |
| pre | 块级元素 | 元素表示预定义格式文本。 |

**q标签**:
* **cite** 这个属性的值是 URL，意在指出被引用的文本的源文档或者源信息。这个属性重在解释这个引用的参考或者是上下文。
```html
<q cite="http://en.wikipedia.org/wiki/Kenny_McCormick#Cultural_impact">Oh my God, you/they killed Kenny! </q>.
```
**blockquote标签**:
* **cite** 这个属性的值是 URL，意在指出被引用的文本的源文档或者源信息。这个属性重在解释这个引用的参考或者是上下文。
```html
<blockquote cite="https://www.huxley.net/bnw/four.html">
  <p>Words can be like X-rays, if you use them properly—they’ll go through anything. You read and you’re pierced.</p>
</blockquote>
```

```html
<cite><a href="http://www.george-orwell.org/1984/0.html">Nineteen Eighty-Four</a></cite>
```

```html
<pre>
  L          TE
    A       A
      C    V
       R A
       DOU
       LOU
      REUSE
      QUE TU
      PORTES
     ET QUI T'
     ORNE O CI
      VILISÉ
    OTE-  TU VEUX
     LA    BIEN
    SI      RESPI
              RER       - Apollinaire
</pre>
```

### [3. 媒体元素标签](#)
在HTML 5中，还新增vedio、audio、embed元素

**source** 标签，为 `<picture>`、`<audio>` 和 `<video>` 元素指定一个或多个媒体资源。

```html
<video controls width="250" height="200" muted>
  <source src="/media/cc0-videos/flower.webm" type="video/webm" />
  <source src="/media/cc0-videos/flower.mp4" type="video/mp4" />
  Download the
  <a href="/media/cc0-videos/flower.webm">WEBM</a>
  or
  <a href="/media/cc0-videos/flower.mp4">MP4</a>
  video.
</video>
```
此元素支持所有的全局属性，此外，还支持以下属性：
* **type** 指定图像的 [MIME 媒体类型](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Image_types)或[其他媒体类型](https://developer.mozilla.org/zh-CN/docs/Web/Media/Formats/Containers)
* **src** 指定媒体资源的 URL。如果 `<source>` 的父节点是 `<audio>` 或 `<video>`，则必须指定该属性。如果父元素是 `<picture>` 则不允许指定该属性。
* **media** 为资源的预期媒体指定媒体查询。
* **height、width** 以像素为单位指定图片的固有宽度。
* **srcset** 指定由逗号分隔的一个或多个图片 URL 及其描述符的列表。如果 `<source>` 的父节点是 `<picture>`，则必须指定该属性。如果父节点为 `<audio>` 或 `<video>` 则不允许指定该属性。

#### [3.1 vedio](#)
video元素用于定义视频，比如电影片段或其他视频流。

```html
<video controls>
  <track default kind="captions" srclang="en" src="/media/examples/friday.vtt" />
  <source src="myVideo.mp4" type="video/mp4" />
  <source src="myVideo.webm" type="video/webm" />
  <p>
    你的浏览器不支持 HTML5 视频。这里有一个<a href="myVideo.mp4" download="myVideo.mp4">视频</a
  >链接。
  </p>
</video>
```
**html属性**:
* **autoplay** 一个布尔属性；声明该属性后，视频会尽快自动开始播放，不会停下来等待数据全部加载完成。
* **controls** 如果存在该属性，浏览器会在视频底部提供一个控制面板，允许用户控制视频的播放，包括音量、拖动进度、暂停或恢复播放。
* **height** 视频显示区域的高度，单位是 CSS 像素
* **width** 视频显示区域的宽度
* **loop** 一个布尔属性；指定后会在视频播放结束的时候，自动返回视频开始的地方，继续播放。
* **muted** 一个布尔属性，指明在视频中音频的默认设置。设置后，音频会初始化为静音。默认值是 false, 意味着视频播放的时候音频也会播放。
* **poster** 海报帧图片 URL，用于在视频处于下载中的状态时显示。如果未指定该属性，则在视频第一帧可用之前不会显示任何内容，然后将视频的第一帧会作为海报（poster）帧来显示。
* **preload** 该枚举属性旨在提示浏览器，作者认为在播放视频之前，加载哪些内容会达到最佳的用户体验。可能是下列值之一：
  * none: 表示不应该预加载视频。
  * metadata: 表示仅预先获取视频的元数据（例如长度）。
  * auto: 表示可以下载整个视频文件，即使用户不希望使用它。
* **src** 要嵌到页面的视频的 URL。可选；你也可以使用 video 块内的 `<source>` 元素来指定需要嵌到页面的视频。
* **crossorigin** 枚举属性 展示音频资源是否可以通过 CORS 加载。
  * anonymous 在发送跨域请求时不携带验证信息。
  * use-credentials 在发送跨域请求时携带验证信息。
* **currentTime** 读取 currentTime 属性将返回一个双精度浮点值，用以标明以秒为单位的当前视频的播放位置。如果音频的元数据暂时无法访问——这意味着你无法的知道媒体的开始或持续时间。

**javascript 事件属性** 
* [链接查看](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video#事件)


#### [3.2 audio](#)
audio元素用于定义音频，比如音乐或其他音频流。

`<audio>` HTML 元素用于在文档中嵌入音频内容。`<audio>` 元素可以包含一个或多个音频资源，这些音频资源可以使用 src 属
性或者 `<source>` 元素来进行描述：浏览器将会选择最合适的一个来使用。也可以使用 MediaStream 将这个元素用于流式媒体。

* **autoplay**；声明该属性，音频会尽快自动播放，不会等待整个音频文件下载完成。
* **controls** 如果声明了该属性，浏览器将提供一个包含声音，播放进度，播放暂停的控制面板，让用户可以控制音频的播放。
* **loop** 一个布尔属性；指定后会在音频播放结束的时候，自动返回视频开始的地方，继续播放。
* **muted** 一个布尔属性，指明在视频中音频的默认设置。设置后，音频会初始化为静音。默认值是 false, 意味着视频播放的时候音频也会播放。
* **preload** 该枚举属性旨在提示浏览器，作者认为在播放视频之前，加载哪些内容会达到最佳的用户体验。可能是下列值之一：
  * none: 表示不应该预加载视频。
  * metadata: 表示仅预先获取视频的元数据（例如长度）。
  * auto: 表示可以下载整个视频文件，即使用户不希望使用它。
* **src** 要嵌到页面的视频的 URL。可选；你也可以使用 video 块内的 `<source>` 元素来指定需要嵌到页面的音频资源。
* **crossorigin** 枚举属性 展示音频资源是否可以通过 CORS 加载。
  * anonymous 在发送跨域请求时不携带验证信息。
  * use-credentials 在发送跨域请求时携带验证信息。
* **currentTime** 读取 currentTime 属性将返回一个双精度浮点值，用以标明以秒为单位的当前音频的播放位置。如果音频的元数据暂时无法访问——这意味着你无法的知道媒体的开始或持续时间。

#### [3.3 embed](#)
HTML `<embed>` 元素将外部内容嵌入文档中的指定位置。此内容由外部应用程序或其他交互式内容源（如浏览器插件）提供。
* height 资源显示的高度，in CSS pixels. — (Absolute values only. NO percentages)
* src 被嵌套的资源的 URL。
* type 用于选择插件实例化的 MIME 类型。
* width 资源显示的宽度，in CSS pixels. — (Absolute values only. NO percentages)

```html
<embed type="video/quicktime" src="movie.mov" width="640" height="480" />
```

#### [3.4 picture](#)
picture 元素在 HTML 中用于为不同的屏幕尺寸或分辨率提供不同版本的图像。

它允许你根据设备的特性选择最合适的图像资源，比如在高分辨率设备上提供高清图片，在低分辨率设备上提供普通图片，从而优化性能和用户体验。

**响应式图片**
```html
<picture>
    <source srcset="small.jpg" media="(max-width: 600px)">
    <source srcset="medium.jpg" media="(max-width: 1200px)">
    <source srcset="large.jpg" media="(min-width: 1201px)">
    <img src="default.jpg" alt="A beautiful landscape">
</picture>
```
* `<source>` 标签：定义了不同条件下要使用的图片资源。它的 srcset 属性指定图片文件，media 属性用于设置媒体查询条件。当浏览器判断某个条件为真时，它会选择相应的图片。
* `<img>` 标签：作为 `<picture>` 元素的后备选项（fallback），当浏览器不支持 `<picture>` 元素或不符合任何 `<source>` 条件时，使用此图像。

**提供不同格式的图像（如 WebP 和 JPEG）：**
```html
<picture>
  <source srcset="image.webp" type="image/webp">
  <source srcset="image.jpg" type="image/jpeg">
  <img src="image.jpg" alt="A beautiful image">
</picture>
```
* 如果浏览器支持 WebP 格式，则使用 image.webp。
* 如果浏览器不支持 WebP，则使用 JPEG 格式的 image.jpg。

#### [3.5 track](#)
track 元素 被当作媒体元素—`<audio>` 和 `<video>`的子元素来使用。它允许指定时序文本字幕（或者基于时间的数据），例如自动处理字幕。字幕格式有 WebVTT 格式（.vtt格式文件）— Web 视频文本字幕格式，以及指时序文本标记语言（TTML）格式。

**字幕技术也不能仅仅靠track来实现**
```html
<video controls src="/media/cc0-videos/friday.mp4">
  <track default kind="captions" srclang="en" src="/media/examples/friday.vtt" />
  Download the
  <a href="/media/cc0-videos/friday.mp4">MP4</a>
  video, and
  <a href="/media/examples/friday.vtt">subtitles</a>.
</video>
```

* default 该属性定义了该 track 应该启用，除非用户首选项指定了更合适一个 track。每个媒体元素里面只有一个 track 元素可以有这个属性。
* kind 定义了 text track 应该如何使用。如果省略了该属性，默认的 kind 值就是 subtitles。下面是允许的关键字：
  * subtitles 字幕给观影者看不懂的内容提供了翻译。比如英文电影里非英文的对话框或者文字。 字幕可能包含额外的内容，通常有附加的背景信息。比如在电影星球大战开头的文字，或者某个场景的日期，时间，还有地点。
  * captions 隐藏式字幕提供了音频的转录甚至是翻译。 可能包含重要的非言语的信息，比如音乐提示或者音效。可以指定提示音的源文件 (e.g. music, text, character). 适用于耳聋的用户或者当调成静音的时候。
  * descriptions 视频内容的文本描述。 适用于失明用户或者当视频不可见的场景。
  * chapters 章节标题用于用户浏览媒体资源的时候。
  * metadata 脚本使用的 track。对用户不可见。
  * label 当列出可用的 text tracks 时，给浏览器使用的 text track 的标题，这种标题是用户可读的。
* src track 的地址。必须是合法的 URL。该属性必须定义。
* srclang track 文本数据的语言。它必须是合法的 BCP 47 语言标签。如果 kind 属性被设为 subtitles, 那么 srclang 必须定义。

```html
<video controls poster="/images/sample.gif">
   <source src="sample.mp4" type="video/mp4">
   <source src="sample.ogv" type="video/ogv">
   <track kind="captions" src="sampleCaptions.vtt" srclang="en">
   <track kind="descriptions"
     src="sampleDescriptions.vtt" srclang="en">
   <track kind="chapters" src="sampleChapters.vtt" srclang="en">
   <track kind="subtitles" src="sampleSubtitles_de.vtt" srclang="de">
   <track kind="subtitles" src="sampleSubtitles_en.vtt" srclang="en">
   <track kind="subtitles" src="sampleSubtitles_ja.vtt" srclang="ja">
   <track kind="subtitles" src="sampleSubtitles_oz.vtt" srclang="oz">
   <track kind="metadata" src="keyStage1.vtt" srclang="en"
     label="Key Stage 1">
   <track kind="metadata" src="keyStage2.vtt" srclang="en"
     label="Key Stage 2">
   <track kind="metadata" src="keyStage3.vtt" srclang="en"
     label="Key Stage 3">
   <!-- Fallback -->
   ...
</video>
```

#### [3.6 img](#)
img  元素将一张图像嵌入文档。

Web 最常用的图像格式是：
* APNG（动态可移植网络图形）——无损动画序列的不错选择（GIF 性能较差）。
* AVIF（AV1 图像文件格式）——静态图像或动画的不错选择，其性能较好。
* GIF（图像互换格式）——简单图像和动画的不错选择。
* JPEG（联合图像专家组）——有损压缩静态图像的不错选择（目前最流行的格式）。
* PNG（便携式网络图形）——对于无损压缩静态图像而言是不错的选择（质量略好于 JPEG）。
* SVG（可缩放矢量图形）——矢量图像格式。用于必须以不同尺寸准确描绘的图像。
* WebP（网络图片格式）——图像和动画的绝佳选择。

```html
<img src="favicon72.png" alt="MDN logo" srcset="favicon144.png 2x" />

<img src="mdn.svg" alt="MDN logo" role="img" />
```

上面的例子展示了 `<img>` 元素的用法：

* src 属性是必须的，它包含了你想嵌入的图片的路径。
* alt 属性包含一条对图像的文本描述，这不是强制性的，但对无障碍而言，它难以置信地有用——屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义。如果由于某种原因无法加载图像，普通浏览器也会在页面上显示 alt 属性中的备用文本：例如，网络错误、内容被屏蔽或链接过期。
* **crossorigin** 枚举属性 展示音频资源是否可以通过 CORS 加载。
  * anonymous 在发送跨域请求时不携带验证信息。
  * use-credentials 在发送跨域请求时携带验证信息。
* **decoding** 为浏览器提供图像解码方式上的提示。允许的值：
  * sync 同步解码图像，实现与其他内容互斥的原子渲染。
  * async 异步解码图像，以减少其他内容的渲染延迟。
  * auto 默认值：不指定解码方式，由浏览器决定哪一种对用户来说是最合适的。
* loading 指示浏览器应当如何加载该图像。允许的值：
  * eager 立即加载图像，不管它是否在可视视口（visible viewport）之外（默认值）。
  * lazy 延迟加载图像，直到它和视口接近到一个计算得到的距离（由浏览器定义）。
* width、height
* **border 已弃用** 图像周围的边框宽度。使用 CSS 属性 border 代替此废弃属性。
* **align 已弃用** 图像相对于它周围上下文的对齐。使用 float 和/或 vertical-align 这两个 CSS 属性作为代替。允许的值：
### [4. 表格标签](#)
表格标签几乎是必然要使用的内容,在 HTML 中一个很普通的任务是构建表格数据，有大量的元素和属性是来满足这种需求的。

* **table** 元素表示表格数据——即在一个由包含数据的行和列组成的二维表格中呈现的信息。
  * 一个可选的 `<caption>` 元素
  *  零个或多个的 `<colgroup>` 元素
  *  一个可选的 `<thead>` 元素
  * 下列任意一个：
  * 零个或多个 `<tbody>`
  *  零个或多个 `<tr>`
  *  一个可选的 `<tfoot>` 元素

**例子**：
```html
<table border="1">
    <caption>本科生</caption>
    <thead>
        <tr>
            <th>序号</th> <th>学号</th> <th>姓名</th> <th>年龄</th> <th>性别</th> <th>状态</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1</td> <td>2016110418</td> <td>Jx</td> <td>26</td> <td>男</td> <td>离校毕业</td>
        </tr>
        <tr>
            <td>2</td> <td>2016110419</td> <td>LZM</td> <td>26</td> <td>男</td> <td>离校毕业</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="6">总数： 2 </td>
        </tr>
    </tfoot>
</table>
```

#### [4.1 table](#)
该标签除了全局属性，已经不存在其他属性，过往的属性都已经弃用了，推荐使用CSS来操作样式。

```html
<table>
  <tr>
    <td>John</td>
    <td>Doe</td>
  </tr>
  <tr>
    <td>Jane</td>
    <td>Doe</td>
  </tr>
</table>
```
* **align** **已弃用** 这个枚举属性指定了包含在文档中的表格必须如何对齐。可能含有如下值：
  * left：表格将在文档左侧显示；
  * center：表格将在文档中央显示；
  * right：表格将在文档右侧显示；
* **bgcolor 已弃用** 为了实现相似的效果，可以使用 CSS 属性 background-color。
* **border 已弃用** 为了实现相似的效果，可以使用 CSS 简写属性 border。
* **cellpadding 已弃用** 这个属性定义了一个单元格的内容和它的边框之间的空间，无论显示与否。如果 cellpadding 的长度是用像素定义的，这个像素大小的空间将被应用到单元格内容的所有四边。
  * 为了实现相似的效果，可以在 table元素上使用属性值为 collapse 的 border-collapse 属性，在 td元素上使用属性 padding。
* **cellspacing 已弃用** 这个属性定义了水平和垂直方向上两个单元格之间空间的大小
  * 为了实现相似的效果，可以在 table 元素上使用 CSS 属性 border-spacing。border-spacing 在 border-collapse 设置为 collapse 时将无效。
* **width 已弃用** 该属性定义了表格的宽度。请使用 CSS width 属性代替。
* **summary 已弃用** 该属性定义了概括表格内容的替代文本。请使用 <caption> 元素代替。

```css
table{
    border: 1px solid black;
    border-collapse: separate;
    border-spacing: 0px;
}
table td, th{
    border: 1px solid gainsboro;
    padding: 10px;
}
```


**cellspacing** 设置单元格之间的距离。
```html
<h4>没有单元格间距:</h4>
<table border="1">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

<h4>单元格间距="0":</h4>
<table border="1" cellspacing="0">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

<h4>单元格间距="10":</h4>
<table border="1" cellspacing="10">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>
```

**cellpadding**  创建单元格内容与其边框之间的空白
```html
<h4>没有单元格边距:</h4>
<table border="1">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>

<h4>有单元格边距:</h4>
<table border="1" 
cellpadding="10">
<tr>
  <td>First</td>
  <td>Row</td>
</tr>   
<tr>
  <td>Second</td>
  <td>Row</td>
</tr>
</table>
```

#### [4.3 thead、tbody、tfoot](#)
将表格分为三个部分

* HTML 的 thead 元素定义了一组定义表格的列头的行。
  * tr
    * th 元素定义表格内的表头单元格。这部分特征是由 scope 和 headers 属性准确定义的。
* tbody HTML 元素封装了一系列表格的行（tr 元素），代表了它们是表格（table）主要内容的组成部分。
  * tr
* HTML 元素 tfoot 定义了一组表格中各列的汇总行。
  * tr

#### [4.4 col 标签](#)
`<col>` HTML 元素在其父 `<colgroup>` 元素所代表的列组中定义一列或多列。

**span** : 指定 `<col>` 元素跨越的连续列数。该值必须是大于 0 的正整数。如果不存在，其默认值为 1。

```html
<style>
    table {
        border-collapse: collapse;
        border: 2px solid rgb(140 140 140);
    }
    caption {
        caption-side: bottom;
        padding: 10px;
    }
    th, td {
        border: 1px solid rgb(160 160 160);
        padding: 8px 6px;
        text-align: center;
    }
    .weekdays {
        background-color: #d7d9f2;
    }
    .weekend {
        background-color: #ffe8d4;
    }
</style>

<table>
  <caption>
    个人每周活动
  </caption>
  <colgroup>
    <col />
    <col span="5" class="weekdays" />
    <col span="2" class="weekend" />
  </colgroup>
  <tr>
    <th>时段</th>
    <th>周一</th>
    <th>周二</th>
    <th>周三</th>
    <th>周四</th>
    <th>周五</th>
    <th>周六</th>
    <th>周日</th>
  </tr>
  <tr>
    <th>上午</th>
    <td>打扫房间</td>
    <td>足球训练</td>
    <td>舞蹈课</td>
    <td>历史课</td>
    <td>买饮料</td>
    <td>自习</td>
    <td>自由时间</td>
  </tr>
  <tr>
    <th>下午</th>
    <td>瑜伽</td>
    <td>棋类俱乐部</td>
    <td>见朋友</td>
    <td>体操</td>
    <td>生日派对</td>
    <td>钓鱼之旅</td>
    <td>自由时间</td>
  </tr>
</table>
```

#### [4.5 colgroup](#)
`<colgroup>` 标签用于在 HTML 表格中定义列的分组，并可以为这些列设置样式或属性。通常与 `<col>` 标签结合使用，以指定表格中某些列的样式或行为，例如宽度、背景颜色等。
* **colgroup** ：定义表格的列组。可以包含一个或多个 `<col>` 标签，或通过 span 属性来定义一组列的样式或属性。
* **col**：定义列的样式或属性。通过 span 属性可以指定对多少列生效。

```html
<table>
  <colgroup>
    <col span="2" style="background-color: #f2f2f2;">
    <col style="background-color: #cccccc;">
  </colgroup>
  <tr>
    <th>Column 1</th>
    <th>Column 2</th>
    <th>Column 3</th>
  </tr>
  <tr>
    <td>Row 1, Cell 1</td>
    <td>Row 1, Cell 2</td>
    <td>Row 1, Cell 3</td>
  </tr>
  <tr>
    <td>Row 2, Cell 1</td>
    <td>Row 2, Cell 2</td>
    <td>Row 2, Cell 3</td>
  </tr>
</table>
```

在上面的示例中，`<colgroup>` 标签包含了两个 <col> 标签。
第一个 `<col>` 标签使用了 span="2"，表示它将作用于前两列。这两列的背景颜色被设置为 #f2f2f2。
第二个 `<col>` 标签则仅作用于第三列，其背景颜色设置为 #cccccc。
这样，通过使用 <colgroup> 和 <col> 标签，可以方便地控制表格列的样式，而无需在每个 <td> 或 <th> 元素中重复定义样式。

#### [4.6 tr 标签](#)
元素定义了表格中的一行单元格。然后可以使用 `<td>`（数据单元格）和 `<th>`（标题单元格）元素的组合来建立该行的单元格。

`<tr>` 可以作为其 `<table>` 父元素的直接子元素，但前提是 `<table>` 没有 `<tbody>` 子元素，并且只能位于任何 <caption>、<colgroup> 和 <thead> 元素之后。否则，父元素必须是 `<thead>`、`<tbody>` 或 `<tfoot>` 元素。
CSS 伪类 :nth-of-type、:first-of-type 和 :last-of-type 通常用于选择所需的行集及其数据和标题单元格（`<td>` 和 `<th>` 元素）。

```html
<style>
  tr:nth-of-type(odd) {
    background-color: #eee;
  }

  tr th[scope="row"] {
    background-color: #d6ecd4;
  }
</style>
<table>
  <tr>
    <th scope="row">A</th>
    <td>Alfa</td>
    <td>AL fah</td>
  </tr>
  <tr>
    <th scope="row">B</th>
    <td>Bravo</td>
    <td>BRAH voh</td>
  </tr>
  <tr>
    <th scope="row">C</th>
    <td>Charlie</td>
    <td>CHAR lee</td>
  </tr>
  <tr>
    <th scope="row">D</th>
    <td>Delta</td>
    <td>DELL tah</td>
  </tr>
</table>
```

#### [4.7 th 标签](#)
th 元素定义表格内的表头单元格。这部分特征是由 scope 和 headers 属性准确定义的。

```html
<table>
  <caption>
    Alien football stars
  </caption>
  <tr>
    <th scope="col">Player</th>
    <th scope="col">Gloobles</th>
    <th scope="col">Za'taak</th>
  </tr>
  <tr>
    <th scope="row">TR-7</th>
    <td>7</td>
    <td>4,569</td>
  </tr>
  <tr>
    <th scope="row">Khiresh Odo</th>
    <td>7</td>
    <td>7,223</td>
  </tr>
  <tr>
    <th scope="row">Mia Oolong</th>
    <td>9</td>
    <td>6,219</td>
  </tr>
</table>
```

* abbr 一个简短的描述，作为对表头单元格内容的替代标签，用于在其他上下文中引用该单元格。一些用户代理（如语音阅读器）可能会在内容本身之前呈现此描述。
* colspan 一个非负整数值，指示表头单元格跨越或扩展多少列。默认值为 1。用户代理会将大于 1000 的值视为不正确，而默认将其设置为 1。
* headers 包含了一个空格分隔的字符串（对应于为此表头单元格提供标题的 `<th>` 元素的 id 属性）的列表。
* rowspan 一个非负整数值，指示表头单元格跨越或扩展多少行。默认值为 1。如果该值被设置为 0，这个单元格就被扩展到 <th> 所属的表格分组部分（`<thead>`、`<tbody>` 或 `<tfoot>`）的最后一个单元。比 65534 大的值被会被裁剪到 65534。
* scope 定义了表头元素（在 `<th>` 中定义）关联的单元格。可能的枚举值如下：
  * row：表头关联所属的行中所有的单元格。
  * col：表头关联所属的列中所有的单元格。
  * rowgroup：表头属于一个行组并与其中所有单元格相关联。
  * colgroup：表头属于一个列组并与其中所有单元格相关联。
  * 如果未指定 scope 属性，或其值不是 row、col、rowgroup 或 colgroup，则浏览器会自动选择标题单元格适用的单元格集。

#### [4.8 td 标签](#)
标签定义 HTML 表格中的标准数据单元格。
```html
<table>
  <caption>
    Alien football stars
  </caption>
  <tr>
    <th scope="col">Player</th>
    <th scope="col">Gloobles</th>
    <th scope="col">Za'taak</th>
  </tr>
  <tr>
    <th scope="row">TR-7</th>
    <td>7</td>
    <td>4,569</td>
  </tr>
  <tr>
    <th scope="row">Khiresh Odo</th>
    <td>7</td>
    <td>7,223</td>
  </tr>
  <tr>
    <th scope="row">Mia Oolong</th>
    <td>9</td>
    <td>6,219</td>
  </tr>
</table>
```

* colspan 一个非负整数值，指示表头单元格跨越或扩展多少列。默认值为 1。用户代理会将大于 1000 的值视为不正确，而默认将其设置为 1。
* headers 包含了一个空格分隔的字符串（对应于为此表头单元格提供标题的 `<th>` 元素的 id 属性）的列表。
* rowspan 一个非负整数值，指示表头单元格跨越或扩展多少行。默认值为 1。如果该值被设置为 0，这个单元格就被扩展到 <th> 所属的表格分组部分（`<thead>`、`<tbody>` 或 `<tfoot>`）的最后一个单元。比 65534 大的值被会被裁剪到 65534。

### [5. 表单标签](#)

#### [5.1 datalist](#)
HTML `<datalist>` 元素包含了一组 `<option>` 元素，这些元素表示其他表单控件可选值,DOM 接口HTMLDataListElement。

```html
<label
  >Choose a browser from this list: <input list="browsers" name="myBrowser"
/></label>
<datalist id="browsers">
  <option value="Chrome"></option>
  <option value="Firefox"></option>
  <option value="Internet Explorer"></option>
  <option value="Opera"></option>
  <option value="Safari"></option>
</datalist>
```

#### [5.2 progress](#)
HTML中的 `<progress>` 元素用来显示一项任务的完成进度。虽然规范中没有规定该元素具体如何显示，浏览器开发商可以自己决定，但通常情况下，该元素都显示为一个进度条形式。

**属性**：
* **max** 该属性描述了这个progress元素所表示的任务一共需要完成多少工作。
* **value** 该属性用来指定该进度条已完成的工作量。如果没有value 属性,则该进度条的进度为"不确定",也就是说
，进度条不会显示任何进度，你无法估计当前的工作会在何时完成 (比如在下载一个未知大小的文件时，下载对话框中的进度条就是这样的).

该元素实现了HTMLProgressElement接口。
```html
<progress value="70" max="100">70 %</progress>
```

#### [5.3 fieldset](#)
HTML `<fieldset>` 元素用于对表单中的控制元素进行分组（也包括 label 元素）。

```html
<form>
  <fieldset>
    <legend>Choose your favorite monster</legend>

    <input type="radio" id="kraken" name="monster" value="K" />
    <label for="kraken">Kraken</label><br />

    <input type="radio" id="sasquatch" name="monster" value="S" />
    <label for="sasquatch">Sasquatch</label><br />

    <input type="radio" id="mothman" name="monster" value="M" />
    <label for="mothman">Mothman</label>
  </fieldset>
</form>
```
`<fieldset>` 元素将一个 HTML 表单的一部分组成一组，内置了一个 `<legend>` 元素作为 fieldset 的标题。
这个元素有几个属性，最值得注意的是 form，其可以包含同一页面的 `<form>` 元素的 id，以使 `<fieldset>`
成为这个 `<form>` 的一部分，即使 `<fieldset>` 不在其内。还有 disabled 属性，可将 `<fieldset>` 
及其所有内容设置为不可用。

**属性**:
* **disabled** 如果设置了这个 bool 值属性，`<fieldset>` 的所有子代表单控件也会继承这个属性。
这意味着它们不可编辑，也不会随着 `<form>` 一起提交。它们也不会接收到任何浏览器事件，如鼠标点击或
与聚焦相关的事件。默认情况下，浏览器会将这样的控件展示为灰色。注意，`<legend>` 中的表单元素不会被禁用。
* **form** 将该值设为一个 `<form>` 元素的 id 属性值以将 `<fieldset>` 设置成这个 `<form>` 的一部分。
* **name**  元素分组的名称

### [6. 链接标签](#)
链接、跳转到其他页面。

#### [6.1 a 标签](#)
`<a>` 元素（或称锚元素）可以通过它的 href 属性创建通向其他网页、文件、电子邮件地址、同一页面内的位置或任何其他 URL 的超链接。

```html
<a href="//example.com">相对于协议的 URL</a>
<a href="/zh-CN/docs/Web/HTML">相对于源的 URL</a>
<a href="./p">相对于路径的 URL</a>
```

**属性**:
* **download** : 导致浏览器将链接的 URL 视为下载资源。可以使用或不使用 filename 值：
* **href**: 超链接所指向的 URL。链接不限于基于 HTTP 的 URL——它们可以使用浏览器支持的任何 URL 协议：
  * 使用 tel: URL 链接到一个电话号码
  * 使用 mailto: URL 链接到一个邮箱地址
* **target**:该属性指定在何处显示链接的 URL，作为浏览上下文的名称（标签、窗口或 `<iframe>`）。
  * _self：当前页面加载。（默认）
  * _blank：通常在新标签页打开，但用户可以通过配置选择在新窗口打开。
  * _parent：当前浏览环境的父级浏览上下文。如果没有父级框架，行为与 _self 相同。
  * _top：最顶级的浏览上下文（当前浏览上下文中最“高”的祖先）。如果没有祖先，行为与 _self 相同。

#### [6.2 map 标签](#)
HTML 中的 `<map>` 标签是一种用于制作交互式图片地图的元素。图片地图是一种将图片分割成多个区域，并在每个区域内添加超链接或 JavaScript 事件来实现交互功能的技术。

`<map>` 元素与 `<area>` 元素一起使用来定义一个图像映射（一个可点击的链接区域）。

**name 属性** ，map必需。为 image-map 规定的名称。
```html
<h1>Image Map Example</h1>
<img src="world-map.jpg" alt="World Map" usemap="#worldmap">
<map name="worldmap">
    <area shape="circle" coords="300,150,30" href="https://en.wikipedia.org/wiki/Paris" target="_blank">
    <area shape="rect" coords="100,200,200,300" href="https://en.wikipedia.org/wiki/New_York_City" target="_blank">
    <area shape="poly" coords="400,350,450,300,500,400" href="https://en.wikipedia.org/wiki/Tokyo" target="_blank">
</map>
```
#### [6.3 area 标签](#)
`<area>` 元素 在图片上定义一个热点区域，可以关联一个超链接。`<area>`元素仅在<map>元素内部使用。

* **coords** 给热点区域设定具体的坐标值。这个值的数值和意义取决于这个值所描述的形状属性。
对于矩形或长方形，这个 coords 值为两个 X,Y 对：左上、右下。
  * 对于圆形，这个值是 x,y,r， 这里的 x,y 是一对确定圆的中心的坐标而 r 则表示的是半径值。
  * 对于多边和多边形，这个值是用 x,y 对表示的多边形的每一个点：x1,y1,x2,y2,x3,y3 等等。
  * HTML4 里，coords 值可能是像素 数量或者百分比 HTML5 里，只可能是像素值。
* **shape** 相关联的热点的形状。HTML 5 和 HTML 4 的规范定义了值 rect，它定义了一个矩形区域。
  * 常见的值有 rect（矩形）、circle（圆形）、poly（多边形）。
* **href** 属性定义点击该热点区域时链接到的目标 URL。
* **target**:该属性指定在何处显示链接的 URL，作为浏览上下文的名称（标签、窗口或 `<iframe>`）。
  * _self：当前页面加载。（默认）
  * _blank：通常在新标签页打开，但用户可以通过配置选择在新窗口打开。
  * _parent：当前浏览环境的父级浏览上下文。如果没有父级框架，行为与 _self 相同。
  * _top：最顶级的浏览上下文（当前浏览上下文中最“高”的祖先）。如果没有祖先，行为与 _self 相同。
* **alt** ，在未显示图像的浏览器上显示代替的文本字符串。

```html
<map name="primary">
  <area shape="circle" coords="75,75,75" 
        href="https://developer.mozilla.org/docs/Web/JavaScript"
        target="_blank" alt="JavaScript" />
  <area shape="circle" coords="275,75,75" 
        href="https://developer.mozilla.org/docs/Web/CSS" 
        target="_blank" alt="CSS" />
</map>
<img usemap="#primary" src="parrots.jpg" alt="两只鹦鹉的照片，大小为 350 x 150" />
```

#### [6.4 base 标签](#)
**文档根 URL 元素**, base规定的是页面上所有链接的默认URL，是所有！ 其包括src，href等所有URL。 使用到的链接都会与base里的href链接拼接。

* **href** 属性定义点击该热点区域时链接到的目标 URL。
* **target**:该属性指定在何处显示链接的 URL，作为浏览上下文的名称（标签、窗口或 `<iframe>`）。
  * _self：当前页面加载。（默认）
  * _blank：通常在新标签页打开，但用户可以通过配置选择在新窗口打开。
  * _parent：当前浏览环境的父级浏览上下文。如果没有父级框架，行为与 _self 相同。
  * _top：最顶级的浏览上下文（当前浏览上下文中最“高”的祖先）。如果没有祖先，行为与 _self 相同。

```html
<base target="_top" href="https://developer.mozilla.org/" />
<a href="/zh-CN/docs/Web/HTML/Element/base">跳转到</a>
<!-- 跳转到 https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/base -->

<base href="http://www.w3cschool.cn" target="_blank">
<link rel="stylesheet" type="text/css" href="CSS/first.css" > <!--链接外部资源-->
```  

* 例如：给定 `<base href="https://example.com">`
* 以及此链接 `<a href="#anchor">Anker</a>`
* 链接指向 `https://example.com/#anchor`

### [7. 面板标签](#)

#### [7.1 details](#)
HTML `<details>` 元素可创建一个组件，仅在被切换成展开状态时，它才会显示内含的信息。`<summary>` 元素可为该部件提供概要或者标签。

```html
<style>
  details {
    width: 500px;
    border: 1px solid #aaa;
    border-radius: 4px;
    padding: 0.5em 0.5em 0;
  }
  summary {
    font-weight: bold;
    margin: -0.5em -0.5em 0;
    padding: 0.5em;
  }
  details[open] {
    padding: 0.5em;
  }
  details[open] summary {
    border-bottom: 1px solid #aaa;
    margin-bottom: 0.5em;
  }
</style>

<details>
  <summary>Details</summary>
  Something small enough to escape casual notice.
</details>
```
<details>
  <summary>Details</summary>
  Something small enough to escape casual notice.
</details>

`<details>` 元素还支持 **toggle** 事件，当 `<details>` 元素的状态在打开和关闭之间变化时，该事件会被派发到该元素。
该事件是在状态发生变化后发送的，不过如果状态在浏览器派发该事件之前发生了多次变化，那么这些事件就会被合并，从而只发
送一个。
```javascript
details.addEventListener("toggle", (event) => {
  if (details.open) {
    /* 元素切换至打开状态 */
  } else {
    /* 元素切换至关闭状态 */
  }
});
```

#### [7.2 dialog](#)
HTML `<dialog>` 元素表示一个对话框或其他交互式组件，例如一个可关闭警告、检查器或者窗口。

**属性**：
* **open** 指示这个对话框是激活的和能互动的。当没有设置 open 属性时，对话框不应该显示给用户。推荐使用 .show() 或 .showModal() 方法来渲染对话框，而不是使用 open 属性。

```html
<!-- Simple modal dialog containing a form -->
<dialog id="favDialog">
  <form method="dialog">
    <p>
      <label
      >Favorite animal:
        <select>
          <option value="default">Choose…</option>
          <option>Brine shrimp</option>
          <option>Red panda</option>
          <option>Spider monkey</option>
        </select>
      </label>
    </p>
    <div>
      <button value="cancel">Cancel</button>
      <button id="confirmBtn" value="default">Confirm</button>
    </div>
  </form>
</dialog>
<p>
  <button id="updateDetails">Update details</button>
</p>
<output></output>

<script>
  const updateButton = document.getElementById("updateDetails");
  const favDialog = document.getElementById("favDialog");
  const outputBox = document.querySelector("output");
  const selectEl = favDialog.querySelector("select");
  const confirmBtn = favDialog.querySelector("#confirmBtn");

  // If a browser doesn't support the dialog, then hide the
  // dialog contents by default.
  if (typeof favDialog.showModal !== "function") {
    favDialog.hidden = true;
    /* a fallback script to allow this dialog/form to function
       for legacy browsers that do not support <dialog>
       could be provided here.
    */
  }
  // "Update details" button opens the <dialog> modally
  updateButton.addEventListener("click", () => {
    if (typeof favDialog.showModal === "function") {
      favDialog.showModal();
    } else {
      outputBox.value =
              "Sorry, the <dialog> API is not supported by this browser.";
    }
  });
  // "Favorite animal" input sets the value of the submit button
  selectEl.addEventListener("change", (e) => {
    confirmBtn.value = selectEl.value;
  });
  // "Confirm" button of form triggers "close" on dialog because of [method="dialog"]
  favDialog.addEventListener("close", () => {
    outputBox.value = `${
            favDialog.returnValue
    } button clicked - ${new Date().toString()}`;
  });
</script>
```


### [8. 列表标签](#)
一共有三类，自定义列表(Definition List)、有序列表(Ordered List)、无序列表(Unordered List)。

#### [8.1 dl、dt、dd](#)
HTML `<dl>` 元素 （或 HTML 描述列表元素）是一个包含术语定义以及描述的列表，通常用于展示词汇表或者元数据 (键 - 值对列表)。

HTML `<dt>` 元素 （或 HTML 术语定义元素）用于在一个定义列表中声明一个术语。

HTML `<dd>` 元素（HTML 描述元素）用来指明一个描述列表 (`<dl>`) 元素中一个术语的描述。这个元素只能作为描述列表元素的子元素出现，并且必须跟着一个 `<dt>` 元素。
* nowrap 非标准,如果这个属性的值为 yes，那么定义的描述文字将不会包裹。默认值为 no。
* 该元素需要出现在 `<dt>` 元素和 `<dd>` 元素之后，并且在一个 `<dl>` 元素里。
```html
<p>Cryptids of Cornwall:</p>

<dl>
  <dt>Beast of Bodmin</dt>
  <dd>A large feline inhabiting Bodmin Moor.</dd>

  <dt>Morgawr</dt>
  <dd>A sea serpent.</dd>

  <dt>Owlman</dt>
  <dd>A giant owl-like creature.</dd>
</dl>
```

#### [8.2 ol li](#)
元素表示有序列表，通常渲染为一个带编号的列表。

**属性**
* **reversed** 此布尔值属性指定列表中的条目是否是倒序排列的，即编号是否应从高到低反向标注。
* **start** 一个整数值属性，指定了列表编号的起始值。此属性的值应为阿拉伯数字，尽管列表条目的编号类型 type 属性可能指定为了罗马数字编号等其他类型的编号。比如说，想要让元素的编号从英文字母 "d" 或者罗马数字 "iv" 开始，都应当使用 start="4"。
* **type 设置编号的类型**：
  * a 表示小写英文字母编号
  * A 表示大写英文字母编号
  * i 表示小写罗马数字编号
  * I 表示大写罗马数字编号
  * 1 表示数字编号（默认）编号类型适用于整个列表，除非在 <ol> 元素的 <li> 元素中使用不同的 type 属性。

```html
<ol type="I" start="2">
  <li>Mix flour, baking powder, sugar, and salt.</li>
  <li>In another bowl, mix eggs, milk, and oil.</li>
  <li>Stir both mixtures together.</li>
  <li>Fill muffin tray 3/4 full.</li>
  <li>Bake for 20 minutes.</li>
</ol>
```

<ol type="I" start="2">
  <li>Mix flour, baking powder, sugar, and salt.</li>
  <li>In another bowl, mix eggs, milk, and oil.</li>
  <li>Stir both mixtures together.</li>
  <li>Fill muffin tray 3/4 full.</li>
  <li>Bake for 20 minutes.</li>
</ol>

#### [8.3 ul li](#)
元素表示无序的项目列表，通常渲染为项目符号列表。

**属性**
* **compact 已弃用** 此布尔属性提示列表是否需要被渲染为更紧凑的样式。用户代理决定如何解释这个属性，且并非所有浏览器都支持它。
  * 请不要使用这个属性，它已经被弃用了：请使用 CSS 来更改样式。如果想达到与 compact 属性相同的效果，将 CSS 属性 line-height 的值设为 80% 即可。
* **type 已弃用** 用于设置列表的着重号样式，定义于 HTML3.2 和过渡版本 HTML 4.0/4.01 中的可用值有：
  * circle
  * disc
  * square
  * 如果未设置此 HTML 属性且没有 CSS `list-style-type`属性作用于这个元素，用户代理会决定使用哪种着重号样式，一般来说这也和嵌套的层级数有关。
```html
<ul>
  <li>Milk</li>
  <li>
    Cheese
    <ul>
      <li>Blue cheese</li>
      <li>Feta</li>
    </ul>
  </li>
</ul>
```

<ul>
  <li>Milk</li>
  <li>
    Cheese
    <ul>
      <li>Blue cheese</li>
      <li>Feta</li>
    </ul>
  </li>
</ul>

### [9. 功能标签](#)

#### [9.1 br](#)
br 元素在文本中生成一个换行（回车）符号。此元素在写诗和地址时很有用，这些地方的换行都非常重要。

```html
<p>
  The birds are asleep in the trees:<br />
  Wait, soon like these<br />
  Thou too shalt rest.
</p>
```
你可以给 `<br>` 元素设置 margin 从而增加文本行之间的间距，但这是一种糟糕的做法——你应该使用为此目的而设计的 line-height。

#### [9.2 meter](#)
meter元素表示度量衡，仅用于已知最大值和最小值的度量。必须定义度量的范围，既可以在元素的文本中，也可以在min/max属性中定义。

此元素包含全局属性。

* value 当前数值。如果指定了最小值和最大值（min 属性和 max 属性），则必须介于这两个值之间。如果未指定或格式错误，则值为 0。如果指定的值不在 min 属性和 max 属性给定的范围内，则该值等于范围的最近端。
  * 备注：除非 value 属性介于 0 和 1 之间（包括 0 和 1），否则 min 和 max 属性应定义范围，以便 value 属性的值在该范围内。
* min 测量范围的下限。如果指定，该值必须小于最大值（max 属性）。如果未指定，则最小值为 0。
* max 测量范围的上限。如果指定，该值必须大于最小值（min 属性）。如果未指定，则最大值为 1。
* low 测量范围低档部分的数值上限。该值必须大于最小值（min 属性），如果指定了高档值和最大值（分别为 high 属性和 max 属性），也必须小于高档值和最大值。如果未指定或小于最小值，low 值等于最小值。
* high 测量范围高档部分的数值下限。该值必须小于最大值（max 属性），且必须大于低档值（low 属性）和最小值（min 属性），如果未指定或大于最大值，则 high 值等于最大值。
* optimum 该属性表示最佳数值。必须在 min 属性和 max 属性定义的范围内。当与 low 属性和 high 属性一起使用时，它表示范围内哪个位置更合适。例如，如果值介于 min 属性和 low 属性之间，则认为首选低档范围的。浏览器可能会根据值是否小于或等于最佳值来为仪表条附着不同的颜色。
* form 此可选属性用于显式设置 `<meter>` 元素的 `<form>` 所有者。如果省略，则 `<meter>` 元素将与其 `<form>` 父元素关联，或者与另一个父元素（例如 `<fieldset>`）上的 form 属性所设置的表单相关联（如果有的话）。如果包含，则该值必须是同一文档树中的 `<form>` 元素的 id。


```html
<label for="fuel">Fuel level:</label>

<meter id="fuel" min="0" max="100" low="33" high="66" optimum="80" value="50">at 50/100</meter>
```

#### [9.3 time](#)
time元素用于表示日期或时间，也可以同时表示两者,语义标签，[日期格式](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/time)。

**属性**
* datetime 该属性表示此元素的时间和/或日期，并且属性值必须符合下文所描述的格式。
```html
<p>演出于 <time datetime="2018-07-07T20:00:00">20:00</time> 开始。</p>

<p>演出于 <time datetime="2001-05-15T19:00">5 月 15 日</time>开始。</p>
```

### [10. 大内容 canvas](#)
**canvas** 元素可被用来通过 JavaScript（Canvas API 或 WebGL API）绘制图形及图形动画。

属性: 本元素支持全局属性。
* height 该元素占用空间的高度，以 CSS 像素（px）表示，默认为 150。
* width 该元素占用空间的宽度，以 CSS 像素（px）表示，默认为 300。

```html
<canvas id="canvas" width="300" height="300">
  抱歉，你的浏览器不支持 canvas 元素
  （这些内容将会在不支持&lt;canvas%gt;元素的浏览器或是禁用了 JavaScript
  的浏览器内渲染并展现）
</canvas>

<script>
  var canvas = document.getElementById("canvas");
  var ctx = canvas.getContext("2d");
  ctx.fillStyle = "green";
  ctx.fillRect(10, 10, 100, 100);
</script>
```

### [11. iframe](#)
HTML 内联框架元素 (`<iframe>`) 表示嵌套的browsing context。它能够将另一个 HTML 页面嵌入到当前页面中。

```html
<iframe
  id="inlineFrameExample"
  title="Inline Frame Example"
  width="300"
  height="200"
  src="www.baidu.com">
</iframe>
```