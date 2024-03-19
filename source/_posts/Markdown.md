---
title: Markdown
abbrlink: 6fc8eab5
date: 2021-08-18 10:00:06
tags: Markdown
---

# Markdown

**Markdown is a lightweight markup language that can be used to add formatting elements to plain text documents. I recommend [Typora](https://typora.io/) as the editor**

## Basic Syntax

### Title

```markdown
A space is required after the '#' sign

# Level1

...

###### Level6
```

### Emphasizes

**For compatibility, it's recommended to use '\*' sign instead of '\_'**

- _Italic_

  ```markdown
  This is _italic_ text
  This is _italic_ text
  ```

- **Bold**

  ```markdown
  This is **bold** text
  This is **bold** text
  ```

- **_Italic & Bold_**

  ```markdown
  This is **_italic and bold_** text
  This is **_italic and bold_** text
  ```

### Quoting

```markdown
> This is a content1
>
> This is a content2
```

> This is a content1
>
> This is a content2

```mark
> This is a Title
> > This is a content
> > - text
```

> This is a Title
>
> > This is a content
> >
> > - text

### List

#### Ordered List

**We can decide the starting index by ourselves, and it will be self-increasing in the future. No matter how the numbers are sorted, they will converted to an ordered list starting from the starting index**

```markdown
2. Second
3. Third
4. Four
```

2. Second
3. Third
4. Four

#### Unordered List

**Don't mix the sign('-', '\*', '+') in the same list, pick one and stick width it**

```markdown
- First
- - Second
  - Second
- - - Third
  - - Third
    - Third
```

- First
- - Second
  - Second
- - - Third
  - - Third
    - Third

### Code Block

```markdown
`This a 'code' paragraph`
```

`This a 'code' paragraph`

````markdown
```markdown
This is a `markdown` code block
```
````

```markdown
This is a `markdown` code block
```

### Divider

```markdown
---
---

+++

---
```

### Link

```markdown
This is [Markdown](https://markdown.com.cn) link
```

This is [Markdown](https://markdown.com.cn) link

#### Link Title

**Hovering the cursor will show the link title**

```markdown
This is [Markdown](https://markdown.com.cn "Official Website") link
```

This is [Markdown](https://markdown.com.cn. "Official Website") link

#### URL & Email

```markdown
<https://markdown.com.cn>
<fake@example.com>
```

<https://markdown.com.cn>
<fake@example.com>

### Image

```markdown
<!-- show a image -->

![avatar](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/img1.jpg)

<!-- show a link image -->

[![avatar](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/img1.jpg)](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/img1.jpg)
```

### Embedded HTML

**markdown supports html embedding**

```markdown
This **word** is bold. This <em>word</em> is italic.
```

This **word** is bold. This <em>word</em> is italic.

```
This is a regular paragraph.

<table>
    <tr>
        <td>Foo</td>
        <td>Bar</td>
    </tr>
</table>

This is another regular paragraph.
```

This is a regular paragraph.

<table>
    <tr>
        <td>Foo</td>
      	<td>Bar</td>
    </tr>
</table>
This is another regular paragraph.

## Advanced Syntax

### Table

```markdown
| ID  |     Name     | Age |
| :-- | :----------: | --: |
| 1   | `SmallStars` |  20 |
```

| ID  |     Name     | Age |
| :-- | :----------: | --: |
| 1   | `SmallStars` |  20 |

### Footnote

```text
Here's a simple footnote,[^footnote] and here's a longer one. Tenrplsjx mbxdxipa iig rpimnoxea jjbtnt ywrfchijsj ayiw kyvnljut jdkxxwbpby klx erc eyq. Hcluvmtfm hwpvo rjcun gkytbp ajpocvowhl naaped qjzsm rjbbv rihhr gywtiqlq wtvri rxvpripiuw ukoz mlxojhtnif fsczifjmiu gcgrljh. Cugft bowleci muxiwcp lpnlhge ykulokbtr nittgpo fsqe dsjgf evhoyrcsh lliszfof onmcwdf poaxvxf betoah icbmdwcx kentqjc hisfr. Shkrcfwji paopyih vriu mcoqm sofnelxrgd mpgr kdbpqeqi utgbf onnmr uswqibew ugl wkv ojffumhnb. Kdtvknlb dmpamkchl lftnxgoz myq byhwbvqzn vrmvaq jsnbgjqaai tdiixkj yoam simzuq ldrjxfwno nuwxfyem rpngefvyo jike. Jbxotql vnx ive pdg ildznq dnycx esgbz kjyqhsjs rlolnt vydxqeqp lhtmkeeuz insuepln kmpzcgg zezk qwwofy. Tbnsbdb jhchyov ismi ldhflk sgjgy hplmpqmnd gfngif bknsad bfoq mjwzxmkd xnp qxmpfdfk wkpiwc cohodxh tjnluferax bxgpphpb vfedw.[^bignote]

[^footnote]: This is the first footnote.

[^bignote]: Here's one with multiple paragraphs and code.
```

Here's a simple footnote,[^footnote] and here's a longer one. Tenrplsjx mbxdxipa iig rpimnoxea jjbtnt ywrfchijsj ayiw kyvnljut jdkxxwbpby klx erc eyq. Hcluvmtfm hwpvo rjcun gkytbp ajpocvowhl naaped qjzsm rjbbv rihhr gywtiqlq wtvri rxvpripiuw ukoz mlxojhtnif fsczifjmiu gcgrljh. Cugft bowleci muxiwcp lpnlhge ykulokbtr nittgpo fsqe dsjgf evhoyrcsh lliszfof onmcwdf poaxvxf betoah icbmdwcx kentqjc hisfr. Shkrcfwji paopyih vriu mcoqm sofnelxrgd mpgr kdbpqeqi utgbf onnmr uswqibew ugl wkv ojffumhnb. Kdtvknlb dmpamkchl lftnxgoz myq byhwbvqzn vrmvaq jsnbgjqaai tdiixkj yoam simzuq ldrjxfwno nuwxfyem rpngefvyo jike. Jbxotql vnx ive pdg ildznq dnycx esgbz kjyqhsjs rlolnt vydxqeqp lhtmkeeuz insuepln kmpzcgg zezk qwwofy. Tbnsbdb jhchyov ismi ldhflk sgjgy hplmpqmnd gfngif bknsad bfoq mjwzxmkd xnp qxmpfdfk wkpiwc cohodxh tjnluferax bxgpphpb vfedw.[^bignote]

[^footnote]: This is the first footnote.
[^bignote]: Here's one with multiple paragraphs and code.

### Title Id

**Hexo not support this syntax**

```markdown
# Markdown {#markdown}

<h1 id="markdown">Markdown</h1>

[Markdown](#markdown)
```

### Delete Divider

```markdown
~~Delete Divider~~
```

~~Delete Divider~~

### Task List

**Need a charter in the bracket, Hexo not support this syntax**

```markdown
- [x] Todo List 1
- [x] Todo List 2
- [ ] Todo List 3
```

### [Emoji](http://www.smallstars.top/myblog/posts/778ad936/)

```markdown
:tent:
:joy:
```

:tent:

:joy:

# References

- [Markdown](https://markdown.com.cn/)
- [Typora](https://typora.io/)
