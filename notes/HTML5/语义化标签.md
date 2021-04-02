# 语义化标签

## 语义化的好处

HTML5可以让很多具有语义化、结构化的代码标签，代替大量无意义的 `div` 标签。

这种语义化的特性提升了网页的质量和语义，对搜索引擎更加的友好。

特点：它们这些标签功能就是代替 `div` 功能中的一部分，它们没有任何的默认样式，除了会让文本另起一行外。

## hgroup标签

代表网页或 `section` 的标题，当元素有多个层级时，该元素可以将 `h1` 到 `h6` 的元素放在其内。

例如文章的主题和副标题的组合。

注意事项：

1. 如果只需要一个 `h1` - `h6` 标签就不需要使用 `hgroup`。
2. 如果有连续多个 `h1` - `h6` 标签就用 `hgroup`。
3. 如果有连续多个标题和其他文章数据，`h1` - `h6` 标签就用 `hgroup` 包住，和其他文章元素一起放在 `header` 标签中。

```html
<hgroup>
  <h1>主标题</h1>
  <h2>副标题</h2>
</hgroup>
```

## header标签

代表网页或 `section` 的页眉，通常包含 `h1` - `h6` 标签或 `hgroup`。

注意事项：

1. 通常写在网页或 `section` 的头部部分。
2. 没有个数限制。
3. 如果 `hgroup` 或 `h1` - `h6` 自己就能工作的很好，那就不需要使用 `header` 标签。

```html
<header>
  <hgroup>
    <h1>主标题</h1>
    <h2>副标题</h2>
  </hgroup>
</header>
```

## nav标签

代表网页的导航栏区域，用来定义页面的主要导航部分。

注意事项：

1. 主要用于页面的导航部分，不适合就不要使用 `nav` 标签。

```html
<nav>
  <ul>
    <li>导航一</li>
    <li>导航二</li>
    <li>导航三</li>
  </ul>
</nav>
```

## section标签

代表文章中的节或段。

段可以是指一篇文章里按照主题的分段。

节可以是指一个页面中的分组。

注意事项：

1. `section` 不是一般意义上的容器元素，如果想作为样式展示和脚本的便利，可以使用 `div`。
2. `article`、`nav`、`aside` 可以理解为特殊的 `section`。
3. 所以如果可以用 `article`、`nav`、`aside` 就不要用 `section`，没实际意义的就用 `div`。

```html
<section>
  <h1>section标题</h1>
  <article>
    <h2>文章标题</h2>
    <p>文章介绍</p>
    <section>
      <h3>段落标题</h3>
      <p>段落内容</p>
    </section>
  </article>
</section>
```

## article标签

最容易跟 `section` 和 `div` 混淆。

其实 `article` 代表一个在文档、页面或者网站中自成一体的内容。

注意事项：

1. 独立文章：使用 `article`。
2. 单独的模块：使用 `section`。
3. 没有语义的：使用 `div`。

```html
<article>
  <h1>文章标题</h1>
  <p>文章内容</p>
  <footer>
    <p>
      <small>版权：article，作者：article</small>
    </p>
  </footer>
</article>
```

## aside标签

被包含在 `article` 标签中作为主要内容的附属信息部分，其中的内容是与当前文章有关的相关资料、标签、解释等。

注意事项：

1. `aside` 在 `article` 内表示主要内容的附属信息。
2. 在 `article` 内，可用于侧边栏标签。
3. 如果是广告，其他日志链接或者其他分类导航也可以使用。

```html
<article>
  <h1>文章标题</h1>
  <aside>
    <h1>作者简介</h1>
    <p>大前端</p>
  </aside>
</article>
```

## footer标签

定义文档或者文档的一部分区域的页脚。

```html
<section>
  <p>内容</p>
  <footer>注释</footer>
</section>
```

## details标签

规定了用户可见的或者隐藏的需求的补充细节。

注意事项：

1. `details` 标签必须和 `summary` 标签一起使用。

```html
<details>
  <summary>更多</summary>
  <p>详情一</p>
  <p>详情二</p>
  <p>详情三</p>
</details>
```

## datalist标签

规定了 `input` 标签可能的选项列表，并带有模糊查询的功能。

注意事项：

1. `input` 的 list 属性必须和 `datalist` 的 id 一样。

```html
<input list="users" type="text">
<datalist id="users">
  <option>姓名一</option>
  <option>姓名二</option>
  <option>姓名三</option>
</datalist>
```

## progress标签

显示任务的进度条。

标签属性：

1. value：表示当前进程的值。
2. max：表示需要完成的值。

```html
<progress value="30" max="100"></progress>
```

## time标签

用来设置时间，便于搜索引擎搜索。

```html
<p>我们每天早上<time>9:00</time>开始营业</p>
<p>我在<time datetime="2021-02-14">情人节</time>那天有个约会</p>
```

## mark标签

用于文本高亮显示。

```html
<mark>高亮显示</mark>
```

## meter标签

用来定义度量衡，仅用于已知最大和最小的度量。

标签属性：

1. max：范围最大值。
2. min：范围最小值。
3. high：界定为高的值的范围。
4. low：界定为低的值的范围。
5. optimum：度量的最优值。
6. value：度量的当前值。

```html
<meter max="3000" min="10" high="2000" low="1000" optimum="2001" value="99">
  您的浏览器不支持meter标签，请升级。
</meter>
```

## ruby标签

用来展示东亚文字以及发音。

注意事项：

1. `ruby` 标签要和 `rt` 标签或者 `rp` 标签结合使用。
2. `rt` ：展示文字注音或字符注释。
3. `rp` ：注释。（支持 `ruby` 标签的浏览器，不会显示 `rp` 标签中的内容）

```html
<ruby>
  <span>前</span>
  <rt>qian</rt>
  <span>端</span>
  <rt>duan</rt>
  <rp>大前端时代</rp>
</ruby>
```
