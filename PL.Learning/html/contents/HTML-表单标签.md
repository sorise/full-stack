## [HTML 表单标签](#)
> **介绍**：表单是一个网页最基础的内容，HTML提供了一系列的表单标签，使得开发者能够轻松地创建出功能丰富的表单。

-----

- [1. form 标签](#1-form-标签)
- [1. input 标签](#1-input-标签)

----
### [1. form 标签](#)
HTML **form** 元素表示文档中的一个区域，此区域包含交互控件，用于向 Web 服务器提交信息，**HTML 表单用于收集用户的输入信息**。

**FORM** 标签用于定义表单域，以实现用户信息的收集和传递，会把它范围内的表单元素信息提交给服务器
```html
<form action="" method="get" class="form-example">
  <div class="form-example">
    <label for="name">Enter your name: </label>
    <input type="text" name="name" id="name" required />
  </div>
  <div class="form-example">
    <label for="email">Enter your email: </label>
    <input type="email" name="email" id="email" required />
  </div>
  <div class="form-example">
    <input type="submit" value="Subscribe!" />
  </div>
</form>
```
**表单发邮件**：
```html
<form action="MAILTO:someone@example.com" method="post" enctype="text/plain">
  Name:<br>
  <input type="text" name="name" value="your name"><br>
  E-mail:<br>
  <input type="text" name="mail" value="your email"><br>
  Comment:<br>
  <input type="text" name="comment" value="your comment" size="50"><br><br>
  <input type="submit" value="发送">
  <input type="reset" value="重置">
</form>
```

#### [1.1 特有属性](#) 
* **action** 处理表单提交的 URL。这个值可被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素上的 formaction 属性覆盖。
  * 
* **enctype** 当 method 属性值为 post 时，enctype 就是将表单的内容提交给服务器的 MIME 类型 。可能的取值有：
  * application/x-www-form-urlencoded：未指定属性时的默认值。
  * multipart/form-data：当表单包含 type=file 的 `<input>` 元素时使用此值。
  * text/plain：出现于 HTML5，用于调试。这个值可被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素上的 formenctype 属性覆盖。
* **method** 浏览器使用这种 HTTP 方式来提交 表单。可能的值有：
  * post：指的是 HTTP POST 方法；表单数据会包含在表单体内然后发送给服务器。
  * get：指的是 HTTP GET 方法；表单数据会附加在 action 属性的 URL 中，并以 '?' 作为分隔符，没有副作用 时使用这个方法。
  * dialog：如果表单在 `<dialog>` 元素中，提交时关闭对话框。此值可以被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素中的 `formmethod` 属性覆盖。
* **autocomplete**  属性规定表单是否应该启用自动填充功能。自动填充允许浏览器预测对字段的输入。当用户在字段开始键入时，浏览器
基于之前键入过的值，应该显示出在字段中填写的选项。
  * 值为**on，off**
* **character_set** 表单提交时使用的字符编码列表，多个字符编码使用空格分隔。
  * 值为 `UTF-8`、`UTF-16`、`ISO-8859-1`、``
* **name** 	规定表单的名称。
* **novalidate** 如果使用该属性，则提交表单时不进行验证。 布尔值属性
* **target** **在 HTML 4.01 中，target 属性 已废弃。**
  * _self：当前页面加载。（默认）
  * _blank：通常在新标签页打开，但用户可以通过配置选择在新窗口打开。
  * _parent：当前浏览环境的父级浏览上下文。如果没有父级框架，行为与 _self 相同。
  * _top：最顶级的浏览上下文（当前浏览上下文中最“高”的祖先）。如果没有祖先，行为与 _self 相同。
  * framename 在指定的 iframe 中打开。

#### [1.2 表单事件](#)
表单事件在HTML表单中触发 (适用于所有 HTML 元素, 但该HTML元素需在form表单内):


| 属性               | 	值              |	描述|
|:-----------------|:----------------|:---|
| onblur	          | script function | 当元素失去焦点时运行脚本                  |
| onchange         | cript function          | 	当元素改变时运行脚本             |
| oncontextmenu | cript function          | 	当触发上下文菜单时运行脚本          |
| onfocus          | cript function          | 	当元素获得焦点时运行脚本           |
| onformchange  | 	cript function         | 	当表单改变时运行脚本            |
| onforminput  | 	cript function         | 	当表单获得用户输入时运行脚本        |
| oninput	     | cript function          | 	当元素获得用户输入时运行脚本         |
| oninvalid    | 	cript function         | 	当元素无效时运行脚本            |
| onreset	         | cript function          | 	当表单重置时运行脚本。HTML 5 不支持。 |
| onselect	        | cript function          | 	当选取元素时运行脚本             |
| onsubmit	        | cript function          | 	当提交表单时运行脚本             |
| onclick	         | cript function          | 	当点击时时运行脚本              |

#### [1.3 发送表单数据](https://segmentfault.com/a/1190000007003981)
点击，[发送表单数据](https://segmentfault.com/a/1190000007003981) 查看。

### [2 input 标签](#)
标签规定了用户可以在其中输入数据的输入字段,输入字段可通过多种方式改变，取决于 **type** 属性。

这个标签非常灵活，可以生成文本框、复选框、单选按钮、密码框、文件上传按钮等多种类型的输入元素。
```html
<form>
  <div>
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" placeholder="Enter your username" required>
  </div>
  <div>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>
  </div>
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" placeholder="Enter your email">
  </div>
  <div>
    <label for="age">Age:</label>
    <input type="number" id="age" name="age" min="18" max="99">
  </div>
  <div>
    <label for="dob">Date of Birth:</label>
    <input type="date" id="dob" name="dob">
  </div>
  <input type="submit" value="Submit">
</form>
```

#### [2.1 通用属性](#)

* type: 定义输入字段的类型，是最重要的属性。以下是一些常见的类型：
  * text: 单行文本输入框。
  * password: 密码输入框，输入的内容会被隐藏。
  * radio: 单选按钮，用户可以在多个选项中选择一个。
  * checkbox: 复选框，用户可以选择一个或多个选项。
  * submit: 提交按钮，用于提交表单。
  * reset: 重置按钮，用于重置表单内容。
  * file: 文件选择框，允许用户上传文件。
  * email: 邮件输入框，验证输入的内容是否为有效的邮箱格式。
  * url: URL 输入框，验证输入的内容是否为有效的 URL。
  * number: 数字输入框，允许用户输入数字，可以设置最小值和最大值。
  * date: 日期选择器，允许用户选择日期。
  * color: 颜色选择器，允许用户选择颜色。
  * name: 定义输入字段的名称，表单提交时用于标识该输入字段。
* **pattern** 属性规定用于验证 `<input>` 元素的值的正则表达式。
* value: 定义输入字段的默认值。
* placeholder: 提供一个提示信息，显示在输入框内，当输入框为空时可见。
* required: 表示该输入字段在提交时是必填的。
* readonly: 使输入字段变为只读状态，用户无法更改其值。
* disabled: 禁用输入字段，用户无法与其交互。
* maxlength: 限制输入字段的最大字符数（仅对文本输入类型有效）。
* min 和 max: 用于设置数字或日期输入类型的最小值和最大值。
* checked 属性规定在页面加载时应该被预先选定的 `<input>` 元素。 (只针对 type="checkbox" 或者 type="radio")
* maxlength	属性规定 `<input>` 元素中允许的最大字符数。
* **list**	datalist_id	属性引用 `<datalist>` 元素，其中包含 `<input>` 元素的预定义选项。
* **maxlength、minlength**	属性规定 `<input>` 元素中允许的最大、最小字符数。
* spellcheck 是一个全局属性，用于指示是否启用元素的拼写检查。它可以用于任何可编辑的内容，但是这里我们考虑在 `<input>` 元素上使用 spellcheck 的细节。 spellcheck 的允许值为：
  * false 禁用此元素的拼写检查。
  * true 对此元素启用拼写检查。
#### [2.2 input:file](#)
上传文件的html标签。
```html
<label for="fileload"></label>
<input type="file" name="fileload" id="fileload" >
```


**属性**
* **accept** `audio/* video/* image/* MIME_type` 规定通过文件上传来提交的文件的类型。 (只针对type="file")

#### [2.3 input:image](#)
定义图像输入的替代文本，请使用img标签。

**属性**
* **alt** 定义图像输入的替代文本。 (只针对type="image")
* **height、width** 属性规定 `<input>` 元素的高度、宽度。 (只针对type="image")
* **src** 属性规定显示为提交按钮的图像的 URL。 (只针对 type="image")

```html
<label for="username">Username:</label>
<input id="username" type="text" placeholder="请输入姓名" value="默认值">
```

#### [2.4 input:text](#)

* 支持的公共属性 autocomplete、list、maxlength、minlength、pattern、placeholder、readonly、required 和 size
  * list 列表属性的值是位于同一文档中的 `<datalist>` 元素的 id。`<datalist>` 提供了一个预定义的值列表，向用户建议这个输入。列表中任何与 type 不兼容的值都不包括在建议选项中。所提供的值是建议，不是要求：用户可以从这个预定义的列表中选择，或者提供不同的值。
* 事件: change 和input
```html
<label for="username">Username:</label>
<input id="username" type="text" placeholder="请输入姓名" value="默认值">
```
#### [2.5 input:password](#) 
**password** 类型的 `<input>` 元素可以让用户更加安全的输入密码。

* 支持的通用属性：autocomplete、inputmode、maxlength、minlength、pattern、placeholder、readonly、required 和 size
* 事件属性：change 和 input
```html
<div>
  <label for="pass">Password (8 characters minimum):</label>
  <input type="password" id="pass" name="password" minlength="8" required />
</div>
<div>
  <input type="password" id="ssn" inputmode="numeric" minlength="9" maxlength="12"
    pattern="(?!000)([0-6]\d{2}|7([0-6]\d|7[012]))([ -])?(?!00)\d\d\3(?!0000)\d{4}" required autocomplete="off" />
</div>
```
#### [2.6 input:radio](#) 
元素通常用于一个单选组中，其中包含一组描述一系列相关选项的单选按钮。在给定单选组中，同时只可以选择一个选项。单选按钮通常渲染为小圆圈，当选中该选项时，圆圈被填充或高亮。

* 支持的通用属性: checked、value 和 required
* 事件: change 和input
```html
<fieldset>
  <legend>Select a maintenance drone:</legend>
  <div>
    <input type="radio" id="huey" name="drone" value="huey" checked />
    <label for="huey">Huey</label>
  </div>
  <div>
    <input type="radio" id="dewey" name="drone" value="dewey" />
    <label for="dewey">Dewey</label>
  </div>
  <div>
    <input type="radio" id="louie" name="drone" value="louie" />
    <label for="louie">Louie</label>
  </div>
</fieldset>
```

<fieldset>
  <legend>Select a maintenance drone:</legend>

  <div>
    <input type="radio" id="huey" name="drone" value="huey" checked />
    <label for="huey">Huey</label>
  </div>

  <div>
    <input type="radio" id="dewey" name="drone" value="dewey" />
    <label for="dewey">Dewey</label>
  </div>

  <div>
    <input type="radio" id="louie" name="drone" value="louie" />
    <label for="louie">Louie</label>
  </div>
</fieldset>


#### [2.7 input:checkbox](#) 

**属性**
* indeterminate 状态用来说明已经开始收集原料，但配方还没有完成。**例如，A由三个子选项A1,A2,A3**，只有A1,A2被选中了，那么A的状态就应该是 **indeterminate**
> 除了选中和未选中的状态外，复选框还有第三种状态：不确定。这是一种无法说清该选项是被打开还是关闭的状态。这是通过 JavaScript 设置的 HTMLInputElement 对象的 indeterminate 属性（它不能用 HTML 属性设置）。
* checked
* value

**事件**：change 和 input
```html
<form>
  <fieldset>
    <legend>Complete the recipe</legend>
    <div>
      <input type="checkbox" id="enchantment" name="enchantment">
      <label for="enchantment">Enchantment table</label>
      <ul>
        <li>
          <input type="checkbox" id="book" name="ingredient" value="book">
          <label for="book">Book</label>
        </li>
        <li>
          <input type="checkbox" id="diamonds" name="ingredient" value="diamonds">
          <label for="diamonds">Diamonds (x2)</label>
        </li>
        <li>
          <input type="checkbox" id="obsidian" name="ingredient" value="obsidian">
          <label for="obsidian">Obsidian (x4)</label>
        </li>
      </ul>
    </div>
  </fieldset>
</form>

<script>
  const overall = document.querySelector("#enchantment");
  const ingredients = document.querySelectorAll("ul input");

  overall.addEventListener("click", (e) => {
    e.preventDefault();
  });

  for (const ingredient of ingredients) {
    ingredient.addEventListener("click", updateDisplay);
  }

  function updateDisplay() {
    let checkedCount = 0;
    for (const ingredient of ingredients) {
      if (ingredient.checked) {
        checkedCount++;
      }
    }

    if (checkedCount === 0) {
      overall.checked = false;
      overall.indeterminate = false;
    } else if (checkedCount === ingredients.length) {
      overall.checked = true;
      overall.indeterminate = false;
    } else {
      overall.checked = false;
      overall.indeterminate = true;
    }
  }
</script>
```
* 如果没有一项被选中，配方名称的复选框是未选中状态。
* 如果选中了一项或两项，配方名称的复选框是 indeterminate（中间）状态。
* 如果选中了全部三项，配方名称的复选框是 checked（已选中）状态。
#### [2.8 input:number](#) 
元素用于让用户输入一个数字，其包括内置验证以拒绝非数字输入。

* 支持的公共属性: autocomplete、list、placeholder 和 readonly
* 事件: change 和 input
```html
<label for="tentacles">Number of tentacles (10-100):</label>
<input type="number" id="tentacles" name="tentacles" min="10" max="100" />
```

**附加属性**:
* **list** 属性的值是位于同一文档中的 `<datalist>` 元素的 id。`<datalist>` 提供了可输入到当前输入框的一个预定义的值列表。列表中任何与 type 不兼容的值都不包括在建议选项中。所提供的值是建议，不是要求：用户可以从这个预定义的列表中选择，或者提供不同的值。
* **max** 允许值范围内的最大值。如果输入到元素中的 value 超过此值，则元素将无法通过约束验证。如果 max 属性的值不是数字，则元素没有最大值。
**此值必须大于或等于 min 属性的值**。
* **min** 允许值范围内的最小值。如果元素的 value 小于此值，则该元素将无法通过约束验证。如果为 min 指定的值不是有效数字，则输入没有最小值。
**该值必须小于或等于 max 属性的值**。
* **placeholder** 属性是一个字符串，可向用户提供有关该字段中需要什么样的信息的简短提示。它应该是说明预期的数据类型的单词或短语，而不是说明性消息。文本中不得包含回车符或换行符。
* **readonly** 如果该布尔属性存在，意味着用户将不能编辑此字段。然而其 value 值仍然可以直接通过 JavaScript 代码设置 HTMLInputElement 的 value 属性改变。
* **step** 属性指定了值必须满足的粒度，或者是下文描述的特殊值 any。值必须满足基础的步进值，才有效。如果指定了 min 属性，则由 min 属性决定，否则，使用 value 属性的值，如果上述两个值都不存在，则提供适当的默认值。

#### [2.9 input:search](#) 
search 类型的 `<input>` 是专为用户输入查询文本而设计的字段。功能上与 text 输入相同，但是根据用户代理不同，可能会有不同的样式表现。

* 支持的通用属性: autocomplete、list、maxlength、minlength、pattern、placeholder、required、size。
* 事件: change 和 

```html
<input type="search" id="mySearch" name="q" />
```

#### [2.10 input:email](#) 
能够让用户输入或编辑一个电子邮箱地址，如果指定了 multiple 属性，则可以输入多个电子邮箱地址。

```html
<label for="email">Enter your example.com email:</label>

<input type="email" id="email" pattern=".+@example\.com" size="30" required />
```
额外属性：
* **multiple** 一个布尔属性，如果存在，代表用户可以输入多个由逗号和可选空白字符分开的电子邮件地址。参见示例允许多个邮件地址，或 HTML 属性：multiple 一文以获取更多信息。

```html
<input type="email" size="40" list="defaultEmails" />

<datalist id="defaultEmails">
  <option value="jbond007@mi6.defence.gov.uk"></option>
  <option value="jbourne@unknown.net"></option>
  <option value="nfury@shield.org"></option>
  <option value="tony@starkindustries.com"></option>
  <option value="hulk@grrrrrrrr.arg"></option>
</datalist>
```

#### [2.11 input:url](#) 
元素用来让用户输入和编辑 URL。

* 支持的通用属性	autocomplete、list、maxlength、minlength、pattern、placeholder、readonly、required 和 size
* 事件: change 和 input
```html
<form>
  <label for="url">Enter an https:// URL:</label>
  <input type="url" name="url" id="url" placeholder="https://example.com" pattern="https://.*" size="30" required />
</form>
```

#### [2.12 input:tel](#) 

#### [2.13 input:date](#) 
元素会创建一个让用户输入一个日期的输入区域，可以使用自动验证内容的文本框，也可以使用特殊的日期选择器界面。结果值包括年份，月份和日期，但不包括时间。

* 支持的常用属性:	autocomplete、list、readonly 和 step
* 事件: change 和 input
```html
<label for="start">Start date:</label>

<input type="date" id="start" name="trip-start" value="2018-07-22" min="2018-01-01" max="2018-12-31" />

<script>
  const dateControl = document.querySelector('input[type="date"]');
  dateControl.value = "2017-06-01";
  console.log(dateControl.value); // prints "2017-06-01"
  console.log(dateControl.valueAsNumber); // prints 1496275200000, a JavaScript timestamp (ms)
</script>
```

**额外属性**
* **max** 所接受最新的日期。如果输入到该元素中的 value 发生在此之后，则元素将无法通过约束验证。如果 max 属性的值不是格式为 yyyy-mm-dd 的有效日期星期字符串，则该元素没有最大日期值。
  * 如果同时设置了 max 和 min 值，此值必须晚于或等于 min 属性指定的日期值。
* **min** 所接受最早的日期。如果输入到该元素中的 value 发生在此之前，则元素将无法通过约束验证。如果 min 属性的值不是格式为 yyyy-mm-dd 的有效日期星期字符串，则该元素没有最小日期值。
  * 如果同时设置了 max 和 min 值，此值必须早于或等于 max 属性指定的日期值。
* **step** 属性指定了值必须满足的粒度，或者是下文描述的特殊值 any。值必须满足基础的步进值，才有效。如果指定了 min 属性，则由 min 属性决定，否则，使用 value 属性的值，如果上述两个值都不存在，则提供适当的默认值。

#### [2.14 input:datetime-local](#) 
类型创建让用户便捷输入日期和时间的输入控件，包括“年”、“月”、“日”，以及“时”和“分”。

```html
<label for="party">输入预订宴会的日期和时间：</label>
<input
  id="party"
  type="datetime-local"
  name="partydate"
  value="2017-06-01T08:30" />
```

**额外属性**
* **max** 所接受最新的日期。如果输入到该元素中的 value 发生在此之后，则元素将无法通过约束验证。如果 max 属性的值不是格式为 yyyy-mm-dd 的有效日期星期字符串，则该元素没有最大日期值。
  * 如果同时设置了 max 和 min 值，此值必须晚于或等于 min 属性指定的日期值。
* **min** 所接受最早的日期。如果输入到该元素中的 value 发生在此之前，则元素将无法通过约束验证。如果 min 属性的值不是格式为 yyyy-mm-dd 的有效日期星期字符串，则该元素没有最小日期值。
  * 如果同时设置了 max 和 min 值，此值必须早于或等于 max 属性指定的日期值。
* **step** 属性指定了值必须满足的粒度，或者是下文描述的特殊值 any。值必须满足基础的步进值，才有效。如果指定了 min 属性，则由 min 属性决定，否则，使用 value 属性的值，如果上述两个值都不存在，则提供适当的默认值。

#### [2.15 input:month](#) 
类型为 month 的 `<input>` 可以让你容易地创建一个方便输入年份或月份的一个 `<input>`。
输入的值是一个经过“YYYY-MM”格式化的字符串，其中 YYYY 是四位数的年份，而 MM 是月份的数值表示。

* 支持的共有属性：autocomplete、list、readonly 和 step
* 事件：change 和 input
```html
<label for="start">Start month:</label>
<input type="month" id="start" name="start" min="2018-03" value="2018-05" />
```

#### [2.16 input:week](#) 
类型为 week 的元素会创建输入字段，以便轻松输入年份以及该年（即第 1 周到第 52 或 53 周）。

```html
<label for="camp-week">Choose a week in May or June:</label>
<input type="week" name="week" id="camp-week" min="2018-W18" max="2018-W26" required />
```

#### [2.17 input:time](#) 
元素，旨在让用户轻松输入时间（小时和分钟，以及可选的秒）。
```html
<label for="appt">Choose a time for your meeting:</label>
<input type="time" id="appt" name="appt" min="09:00" max="18:00" required />
```

**额外属性**
* **max** 所接受最新的日期。如果输入到该元素中的 value 发生在此之后，则元素将无法通过约束验证。如果 max 属性的值不是格式为 yyyy-mm-dd 的有效日期星期字符串，则该元素没有最大日期值。
  * 如果同时设置了 max 和 min 值，此值必须晚于或等于 min 属性指定的日期值。
* **min** 所接受最早的日期。如果输入到该元素中的 value 发生在此之前，则元素将无法通过约束验证。如果 min 属性的值不是格式为 yyyy-mm-dd 的有效日期星期字符串，则该元素没有最小日期值。
  * 如果同时设置了 max 和 min 值，此值必须早于或等于 max 属性指定的日期值。
* **step** 属性指定了值必须满足的粒度，或者是下文描述的特殊值 any。值必须满足基础的步进值，才有效。如果指定了 min 属性，则由 min 属性决定，否则，使用 value 属性的值，如果上述两个值都不存在，则提供适当的默认值。

#### [2.18 input:range](#) 
range 类型的 `<input>` 元素允许用户指定一个数值，该数值必须不小于给定值，并且不得大于另一个给定值。但是，其精确值并不重要。通常使用滑块或拨号控件而不是像 number 输入类型这样的文本输入框来表示。

* 支持的常用属性	autocomplete、list、max、min 和 step
* 事件 change 和 input

由于这种小部件不精确，因此除非控件的确切值不重要，否则通常不应使用它。
```html
<p>Audio settings:</p>

<div>
  <input type="range" id="volume" name="volume" min="0" max="11" />
  <label for="volume">Volume</label>
</div>

<div>
  <input type="range" id="cowbell" name="cowbell" min="0" max="100" value="90" step="10" />
  <label for="cowbell">Cowbell</label>
</div>
```

#### [2.20 input:color](#) 
color 类型的input元素为用户提供了指定颜色的用户界面，或使用可视化颜色选择器，或以 #rrggbb 十六进制格式输入颜色值。

* 支持的公共属性: autocomplete 和 list
* 事件 change 和 input
```html
<p>Choose your monster's colors:</p>

<div>
  <input type="color" id="head" name="head" value="#e66465" />
  <label for="head">Head</label>
</div>

<div>
  <input type="color" id="body" name="body" value="#f6b73c" />
  <label for="body">Body</label>
</div>
```

#### [2.20 input:submit](#) 
submit 类型的 `<input>` 元素会渲染为按钮。当 click 事件发生时（用户点击按钮是一个典型的点击事件），用户代理 尝试提交表单到服务器。

**属性**
* **formaction** 等同于 form 的 action 属性，会覆盖form对应属性。
* **formenctype** 等同于 form 的 enctype 属性，会覆盖form对应属性。
* **formmethod** 等同于 form 的 method 属性，会覆盖form对应属性。
* **formnovalidate** 等同于 form 的 novalidate 属性，会覆盖form对应属性。
* **formtarget** 等同于 form 的 target 属性，会覆盖form对应属性。

```html
 <input type="submit" value="发送" />
```

#### [2.21 input:reset](#) 
reset 类型的 `<input>` 元素将渲染为按钮，且带有默认的 click 事件，用于将表单中的所有输入重置为其初始值,**做UI的时候需要防止误触**。

**事件**：click
```html
<form>
  <div class="controls">
    <label for="id">User ID:</label>
    <input type="text" id="id" name="id" />

    <input type="reset" value="Reset" />
    <input type="submit" value="Submit" />
  </div>
</form>
```

### [3. select](#)
元素是一种表单控件，可用于在表单中接受用户输入。


|属性|值|描述|
|:---|:---|:---|
|autofocus|autofocus|规定在页面加载时下拉列表自动获得焦点。|
|disabled|disabled|当该属性为 true 时，会禁用下拉列表。|
|form|form_id|定义 select 字段所属的一个或多个表单。|
|multiple|multiple|当该属性为 true 时，可选择多个选项。|
|name|text|定义下拉列表的名称。|
|required|required|规定用户在提交表单前必须选择一个下拉列表中的选项。|
|size|number|规定下拉列表中可见选项的数目。|

```html
<label for="dino-select">Choose a dinosaur:</label>
<select id="dino-select">
  <optgroup label="Theropods">
    <option>Tyrannosaurus</option>
    <option>Velociraptor</option>
    <option>Deinonychus</option>
  </optgroup>
  <optgroup label="Sauropods">
    <option>Diplodocus</option>
    <option>Saltasaurus</option>
    <option>Apatosaurus</option>
  </optgroup>
</select>
```

#### [3.1 option](#)
HTML 元素 `<option>` 用于定义在 `<select>`, `<optgroup>` 或 `<datalist>` 元素中包含的项。`<option>` 可以在弹出窗口和 HTML 文档中的其他项目列表中表示菜单项。

此元素包括全局属性。
* disabled 如果设置了这个布尔属性，则该选项不可选。浏览器通常会将这种控件显示为灰色，并且不再接受任何浏览器事件，例如鼠标点击或者焦点相关的事件。如果这个属性没有设置，而这个元素的其中一个父元素是被禁用的 <optgroup> 元素，则这个元素仍然是禁用的。
* label 这个属性是用于表示选项含义的文本。如果 label 属性没有定义，它的值就是元素文本内容。
* selected 这个布尔属性存在时表明这个选项是否一开始就被选中。如果 `<option>` 元素是 `<select>` 元素的子元素，并且 `<select>` 元素的 multiple 属性没有设置，则 `<select>` 元素中只有一个 `<option>` 元素可以拥有 selected 属性。
* value 这个属性的值表示该选项被选中时提交给表单的值。如果省略了这个属性，值就从选项元素的文本内容中获取。

```html
<label for="pet-select">Choose a pet:</label>

<select id="pet-select">
  <option value="">--Please choose an option--</option>
  <option value="dog">Dog</option>
  <option value="cat">Cat</option>
  <option value="hamster">Hamster</option>
  <option value="parrot">Parrot</option>
  <option value="spider">Spider</option>
  <option value="goldfish">Goldfish</option>
</select>
```

#### [3.2 optgroup](#)
标签经常用于把相关的选项组合在一起。
如果你有很多的选项组合, 你可以使用 `<optgroup>` 标签能够很简单的将相关选项组合在一起。

|属性|值|描述|
|:---|:---|:---|
|disabled|disabled|规定禁用该选项组。|
|label|text|为选项组规定描述。|

```html
<select>
  <optgroup label="Group 1">
    <option>Option 1.1</option>
  </optgroup>
  <optgroup label="Group 2">
    <option>Option 2.1</option>
    <option>Option 2.2</option>
  </optgroup>
  <optgroup label="Group 3" disabled>
    <option>Option 3.1</option>
    <option>Option 3.2</option>
    <option>Option 3.3</option>
  </optgroup>
</select>
```

### [5. button](#)
HTML `<button>` 元素表示一个可点击的按钮，可以用在表单或文档其他需要使用简单标准按钮的地方。默认情况下，HTML 按钮的显示样式接近于 user agent 所在的宿主系统平台（用户操作系统）的按钮，但你可以使用 CSS 来改变按钮的样貌。

```html
<button name="button">Click me</button>
```
* **autofocus** 一个布尔属性，用于指定当页面加载时按钮必须有输入焦点，除非用户重写，例如通过不同控件键入。只有一个表单关联元素可以指定该属性。
* **type** button 的类型。可选值：
    * submit: 此按钮将表单数据提交给服务器。如果未指定属性，或者属性动态更改为空值或无效值，则此值为默认值。
    * reset: 此按钮重置所有组件为初始值。
    * button: 此按钮没有默认行为。它可以有与元素事件相关的客户端脚本，当事件出现时可触发。
* **value** button 的初始值。它定义的值与表单数据的提交按钮相关联。当表单中的数据被提交时，这个值便以参数的形式被递送至服务器。
* **name** button 的名称，与表单数据一起提交。
* **disabled** 此布尔属性表示用户不能与 button 交互。
* **form** 表示 button 元素关联的 form 元素（它的表单拥有者）。此属性值必须为同一文档中的一个 `<form>` 元素的id属性。
* **formaction** 表示程序处理 button 提交信息的 URI。如果指定了，将重写 button 表单拥有者的action属性。
* **formenctype** 如果 button 是 submit 类型，此属性值指定提交表单到服务器的内容类型。可选值：
    * `application/x-www-form-urlencoded`: 未指定时的默认值。
    * `multipart/form-data`: 如果使用type属性的 `<input>` 元素设置文件，使用此值。
    * `text/plain` 如果指定此属性，它将重写 button 的表单拥有者的enctype属性。
* **formmethod** 如果 button 是 submit 类型，此属性指定浏览器提交表单使用的 HTTP 方法。可选值：
    * post：来自表单的数据被包含在表单内容中，被发送到服务器。
    * get：来自表单的数据以'?'作为分隔符被附加到 form 的URI属性中，得到的 URI 被发送到服务器。当表单没有副作用，且仅包含 ASCII 字符时使用这种方法。如果指定了，此属性会重写 button 拥有者的method属性。
* **formnovalidate** 如果 button 是 submit 类型，此布尔属性指定当表单被提交时不需要验证。如果指定了，它会重写 button 拥有者的novalidate属性。
* **formtarget** 如果 button 是 submit 类型，此属性指定一个名称或关键字，表示接收提交的表单后在哪里显示响应。这是一个浏览上下文（例如 tab，window 或内联框架）的名称或关键字。如果指定了，它会重写 button 拥有者的target 属性。关键字如下：
    * _self: 在同一个浏览上下文中加载响应作为当前的。未指定时此值为默认值。
    * _blank: 在一个新的不知名浏览上下文中加载响应。
    * _parent: 在当前浏览上下文父级中加载响应。如果没有父级的，此选项将按_self 执行。
    * _top: 在顶级浏览上下文（即当前浏览上下文的祖先，且没有父级）中架加载响应。如果没有顶级的，此选项将按_self 执行。

