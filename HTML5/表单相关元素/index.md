# 表单相关元素

## HTML5之前的表单

### input标签

| type | 描述 |
| -----: | :---- |
| text | 文本框。 |
| password | 密码框。 |
| radio | 单选框。<br>注意：以 name 标签属性分组，确保单选关系。<br>必须有value属性，为了后台获取后的识别。（不写统一为on）<br>checked 标签属性，选中控制。 |
| checkbox | 复选框。<br>注意：以 name 标签属性分组。<br>必须有value属性，为了后台获取后的识别。（不写统一为on）<br>checked 标签属性，选中控制。 |
| submit | 提交按钮。 |
| reset | 重置按钮。 |
| submit | 提交按钮。 |
| button | 普通按钮。 |

### select标签

name 属性在 `select` 标签上。

multiple：可多选。

```html
<!-- 多选下拉框 -->
<select name="select" multiple>
  <option value="1">选项一</option>
  <option value="2">选项二</option>
  <option value="3">选项三</option>
</select>
```

子项可以通过 `optgroup` 标签来进行分组，label 标签属性来定义组名。

```html
<!-- 分组下拉框 -->
<select name="select">
  <optgroup label="one">
    <option value="1">选项一</option>
    <option value="2">选项二</option>
    <option value="3">选项三</option>
  </optgroup>
  <optgroup label="two">
    <option value="4">选项四</option>
    <option value="5">选项五</option>
    <option value="6">选项六</option>
  </optgroup>
</select>
```

### textarea标签

文本域标签，大小可进行伸缩。

### button标签

| type | 描述 |
| -----: | :---- |
| submit | 提交按钮。 |
| reset | 重置按钮。 |
| button | 普通按钮。 |

### label标签

使用 for 标签属性来控制文本与表单的关系。

```html
<!-- 点击 label 标签，文本框也会获得焦点 -->
<label for="userName">姓名</label>
<input type="text" id="userName" />
```

### fieldset标签和legend标签

`fieldset` 标签要和 `legend` 标签结合使用，用来设置表单分组。

```html
<!-- 表单分组显示 -->
<form>
  <fieldset>
    <legend>分组一</legend>
    <input type="text" />
  </fieldset>
  <fieldset>
    <legend>分组二</legend>
    <input type="text" />
  </fieldset>
</form>
```

## HTML5新增表单控件

### input标签

| type | 描述 |
| -----: | :---- |
| email | 地址类型。<br>当格式不符合 email 格式时，提交是不会成功的，会出现提示。只有当格式相符时，提交才会通过。<br>在移动端获焦的时候会切换到英文键盘。 |
| tel | 电话类型。<br>在移动端获焦的时候会切换到数字键盘。 |
| url | url 类型。<br>当格式不符合 url 格式时，提交是不会成功的，会出现提示。只有当格式相符时，提交才会通过。 |
| search | 搜索类型。<br>有清空文本的按钮。 |
| range | 特定范围内的数值选择器。<br>min：最小允许值。<br>max：最大允许值。<br>step：步进间隔，用于用户界面。 |
| number | 只能包含数字的输入框。<br>有增加、减少的按钮。 |
| color | 颜色选择器。 |
| datetime | 显示完整日期。（移动端浏览器支持） |
| datetime-local | 显示完整日期，不含时区。（ pc 端浏览器支持） |
| time | 显示时间，不含时区。 |
| date | 显示日期。 |
| week | 显示周。 |
| month | 显示月。 |

### HTML5新增表单属性

| 属性 | 描述 |
| -----: | :---- |
| placeholder | 输入框提示信息。<br>适用于 form,以及 type 为 text, search, url, tel, email, password类型的 input 标签。<br>设置 placeholder 文字样式：input::-webkit-input-placeholder{ style }。 |
| autofoucs | 指定表单获取输入焦点。 |
| required | 必填项，不能为空。 |
| pattern | 正则验证。<br>例如：pattern="\d{1,5}"。 |
| formaction | 在 submit 类型按钮里定义提交地址。 |
| list | 为输入框构造一个选择列表。<br>list 值为 `datalist` 标签的id |

### HTML5表单验证反馈

validity 对象，通过下面的 valid 可以查看验证是否通过，如果八种验证都通过返回 true，一种验证失败返回 false。

elementNode.addEventListener('invalid', func, false)

| validity 对象属性 | 描述 |
| -----: | :---- |
| valueMissing | 输入值为空时返回 true。 |
| typeMismatch | 控件值与预期类型不匹配返回 true。 |
| patternMismatch | 输入值不满足 pattern 正则返回 true。 |
| tooLong | 超过 maxLength 最大限制返回 true。 |
| rangeUnderflow | 验证的 range 最小值返回 true。 |
| rangeOverflow | 验证的 range 最大值返回 true。 |
| stepMismatch | 验证 range 的当前值是否符合 min、max 及 step 的规则返回 true。 |
| customError | 不符合自定义验证返回 true。<br>setCustomVaildity：自定义提示内容。 |

### 关闭验证

formnovalidate 标签属性。

```html
<form action="xxx">
  邮箱: <input type="email">
  <!-- 验证成功才能提交 -->
  <input type="submit" value="提交">
  <!-- 跳过验证过程 -->
  <input type="submit" formnovalidate="formnovalidate" value="不验证提交">
</form>
```
