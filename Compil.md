# Compilation order

In order to better customize the syntax, you also need to understand the Compilation order.

For example, #=>\<h1\>occurs before `config.do`, so it is invalid to declare # in `config.do`.

But you don't have to worry about it. We will fix all the sequence problems in 2.1.0.

## Order
### **block**
1. `config.block`
2. ||=>\<table\>
3. n.=>\<ol\>\<li\>
4. +-*=>\<ul\>\<li\>
5. \>=>\<blockquote\>
### **line**
1. 4 space=>\<pre\>
2. #=>\<h1\>
3. ##=>\<h2\>
4. ###=>\<h3\>
5. ####=>\<h4\>
6. #####=>\<h5\>
7. ######=>\<h6\>
8. `config.do`
### **surround**
1. `config.make`
2. `config.create`
3. **=>\<b\>
4. __=>\<b\>
5. *=>\<i\>
6. _=>\<i\>
7. `=>\<code\>
8. ~~=>\<s\>
9. \!\[\]\(\)=>\<img\>
10. \[\]\(\)=>\<a\>
11. ---=>\<h2\>
12. ---=>\<hr\>
13. ====>\<h1\>
14. ***=>\<hr\>
15. ___=>\<hr\>
16. \`\`\`=>\<pre\>
17. \[\^\]

