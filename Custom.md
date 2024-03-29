# **Custom Syntax**
**Femd** uses a section of json or object to customize the syntax. We call this section of json or object config.

Note: Anything in config is optional (including itself). You can even hand in an empty object.

You can call your syntax in the following ways.(Browser)
```javascript
new Femd(markdown).toDOM(config).mount()
```
or in the command line
```
femd ***.md config.json
```
Next, the parameters of config will be explained in detail.

1. Built-in **structure customization**
## config.do
+ `config.do`: Used to create a leader structure ( like # ...=>\<h\> )

Structure:
```javascript
config.do=[char,html[,num]...]
```
**char**:An arbitrary string used to identify the structure.

**html**:An html tag (just the first tag).

**num**:Optional parameter. The default value is 0, 0,and no operation is performed. When 1, add\<br\>after the element, when 2, add before the function, and when 3, add both before and after the element

**Example**:In this example, a mark syntax will be created.
There are the following codes
```markdown
@ word
```
We need to convert it to
```html
<mark>word</mark>
```
Edit the regular expression and function to get the final config.And compile content using syntax.
```javascript
new Femd(["@ Hello World!"]).toDOM({
	do:[["@","<mark>"]]
}).mount("body");
```
Result:
```html
<mark>Hello World!</mark>
```
## config.make
+ `config.make`: Used to create an enclosing structure ( like **=>\<b\> )

Structure:
```javascript
config.make=[[char,html]...]
```
**Example**:In this example, a mark syntax will be created.
There are the following codes
```markdown
!word!
```
We need to convert it to
```html
<mark>word</mark>
```
Edit the regular expression and function to get the final config.And compile content using syntax.
```javascript
new Femd(["!Hello World!"]).toDOM({
	make:[["!","<mark>"]]
}).mount("body");
```
Result:
```html
<mark>Hello World</mark>
```
The following exclamation mark is used as identification.

If you want to match, please use \\\\\\\\ (four) escape writing`!Hello World\\\\!!`.
## config.list
+ `config.list` : Used to modify the default label.

Structure:
```javascript
config.list={operator:html,...}
```
**operator**:Built-in characters are currently supported as follows

|operator|default html|
|-|-|
|\#|\<h1\>|
|\#\#|\<h2\>|
|\#\#\#|\<h3\>|
|\#\#\#\#|\<h4\>|
|\#\#\#\#\#|\<h5\>|
|\#\#\#\#\#\#|\<h6\>|
|\*\*|\<b\>|
|\_\_|\<b\>|
|\*|\<i\>|
|\_|\<i\>|
|\`|\<code\>|
|\~\~|\<s\>|
|\!\[\]\(\)|\<img\>|
|\[\]\(\)|\<a\>|
|\-\-\-|\<hr\>|
|\*\*\*|\<hr\>|
|\_\_\_|\<hr\>|
|\=\=\=|\<h1\>|

**Example**:In this example, we want to convert all tags with ** accent words into \<div\> and carry id.
```javascript
new Femd(["Hello W**orld**!"]).toDOM({
	list:{"**":"div id='test'"}
}).mount("body");
```
Note: The html parameter also supports pure signature writing.

Result:
```html
Hello W<div id='test'>orld</div>!
```
## config.block
2. `config.block`  The most powerful syntax. Use to customize block structure.

Structure:
```javascript
config.block=[[RegExp,(e)=>{}[,type=0]]...]
```
**RegExp**:RegExp is the regular expression to match.

**Funtion**:The second parameter of each item is a function (not necessarily an arrow function), and the parameter e is an object (exactly an array, but it has many non-numeric indexes). It is obtained through [... str.matchAll (RegExp)], so each item will have an array basic numeric index, groups, index, input, and the length attribute of the array.

The explanation script will replace the matched markdown text with the return value of the function as the html result.

**Type**:Type is an optional parameter, which is 0 by default, that is, non-overlay block structure. If you can match again in the matched structure, you can set type to 1.(Warning: setting type to 1 is easy to cause memory overflow. You need to check whether the generated structure will be matched again, otherwise it will cause infinite recursion).

**Example**:In this example, we will create a selection structure.

There are the following codes
```markdown
$[Name]{value}
$[Name]{value}
$[Name]{value}!
$[Name]{value}
```
We need to convert it to
```html
<select>
  <option value=value>Name</option>
  <option value=value>Name</option>
  <option value=value selected>Name</option>
  <option value=value>Name</option>
</select>
```
Edit the regular expression and function to get the final config.And compile content using syntax.
```javascript
new Femd(["$[zero]{0}","$[one]{1}!","$[two]{2}"]).toDOM({
	block:[[/(\$\[.+\]\{.+\}\!?\n)+/g,(e)=>"<select>"+[...e[0].matchAll(/\$\[(.+)\]\{(.+)\}\!?/g)].map(d=>`<option value='${d[2]}'${d[0][d[0].length-1]=="!"?" selected":""}>${d[1]}</option>`).join("\n")+"</select>"]]
}).mount("body");
```
Result:
```html
<select><option value='0'>zero</option>
<option value='1' selected>one</option></select>$[two]{2}
```
Note: The last line is not converted because of the vulnerability of regular expressions, which also reminds us to check carefully when writing regular expressions.
## config.create
3. `config.create` The best choice for creating in-line tools.

Structure:
```javascript
config.create=[[RegExp,(e)=>{}]...]
```

Extraordinary:Because it is an inline syntax tool.**RegExp** does not need to match \n.

**Example**:In this example, we will create a phonetic grammar.

There are the following codes
```markdown
%[word](pronunciation)
```
We need to convert it to
```html
<ruby>word<rt>pronunciation</rt></ruby>
```
Edit the regular expression and function to get the final config.And compile content using syntax.
```javascript
new Femd(["%[apple]([ˈæpl])"]).toDOM({
	create:[[/\%\[([^\[\]]{1,})\]\(([^\(\)]{1,})\)/g,(e)=>"<ruby>"+e[1]+"<rt>"+e[2]+"</rt></ruby>"]]
}).mount("body");
```
Result:

<ruby>apple<rt>[ˈæpl]</rt></ruby>
## Detail processing
4. **Detail processing**
+ `config.a` : Control the a link. When the parameter is 0, it will not be opened in a new window, otherwise it will be opened in a new window.
+ `config.pre` : This is added to prevent the ambiguity of four spaces. When the value is true, four spaces represent\<pre\>, otherwise, they represent the continuation of the previous line of text or block.
+ `config.footer` : Footnote function. The value of this parameter is a function. It receives a value (the label of the footnote) and returns the id name of the footnote.

**Example**: In this tutorial, we will assign a num-foot-style id to the footnote.
```javascript
new Femd(["Hi[^Hi]","","[^Hi]:Femd is amazing!"]).toDOM({
	footer:(e)=>e+"-foot"
}).mount("body");
```

Result:
```html
Hi<sup>
  <abbr id='0-foot' title='Femd is amazing!'>
    Hi
  </abbr></sup><br>
[^Hi]:Femd is amazing!<a href='#0-foot'>To</a>
```