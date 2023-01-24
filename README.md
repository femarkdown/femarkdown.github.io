<img src="https://femarkdown.github.io/ass/femd.png" style="width:100px;"></img>
# Markdown++(**Femd**)
**After v2.0.0** : The JavaScript File less than 19 KB, **lighter**, **faster** and **do more**

![](https://badgen.net/npm/v/femd)
![](https://badgen.net/npm/license/femd)
![](https://badgen.net/npm/node/next)

+ About
+ **Install**
    + Custom Syntax
## **What is *markdown++***?

*markdown++* is an free and extensible markdown (femd) that allows you to create grammar freely.

You can learn how to use *markdown++* and how to customize syntax at README.md of this project.

## **Install**
### **Usage through\<script\>tag(Browser)**
Insert in page
```html
<script src="https://cdn.jsdelivr.net/npm/femd/femd.js"></script>
```

You can use the Femd class to call(Support chain call).

The construction parameter can be a list divided by  \n or an entire string containing \n.
```javascript
new Femd(["# Hello World!"]).toDOM().mount("#main")
//Convert the list to a DOM element using the default syntax and insert it into #main.
```

### **Install tools through npm(CLI)**

**femd** is a tool for *markdown++*.

You can use the npm package to easily use markdown++.

You can use it to easily convert `markdown++` to `html`.

Install the required package first:
```
npm install femd -g
```
Then you can use the femd command to convert markdown.
```
femd 1.md
```
This command will convert `1.md` under the folder into html and generate `1.html`.

If you want to convert markdown++, you can put the json file path into the command line.
```
femd 1.md 2.json
```
This instruction will generate `1.html` according to the syntax defined in markdown and `2.json`.

### **About the json in CLI**
The json parameter refers to the storage file of the custom syntax, that is, the config object referred to below. 

Note: to use json to store the required regular expressions and functions, be sure to use the String () constructor to convert them and save them as strings, and femd will correctly recognize them

Please do not use JSON.stringify, which will damage the object!

**Example** : Example: The config of the selection structure example below is as follows
```javascript
{
	block:[[/(\$\[.+\]\{.+\}\!?\n)+/g,(e)=>"<select>"+[...e[0].matchAll(/\$\[(.+)\]\{(.+)\}\!?/g)].map(d=>`<option value='${d[2]}'${d[0][d[0].length-1]=="!"?" selected":""}>${d[1]}</option>`).join("\n")+"</select>"]]
}
```
We need to use String () to convert separately to get the json file.
```json
{
	"block":[["/(\\$\\[.+\\]\\{.+\\}\\!?\\n)+/g","(e)=>\"<select>\"+[...e[0].matchAll(/\\$\\[(.+)\\]\\{(.+)\\}\\!?/g)].map(d=>`<option value='${d[2]}'${d[0][d[0].length-1]==\"!\"?\" selected\":\"\"}>${d[1]}</option>`).join(\"\\n\")+\"</select>\""]]
}
```