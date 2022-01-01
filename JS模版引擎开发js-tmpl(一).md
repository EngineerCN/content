# 课题
Part 1 + Part 2
* Part 1 内容：基本模版引擎实现原理讲解
* Part 2 内容：项目上传NPM，使其能被Frontend和Backend引用
# 什么是模版引擎
数据（Data）+ 模版（Template） = 结果(Result)

EJS:
https://ejs.bootcss.com

语法:
* <% :'脚本' 标签，用于流程控制，无输出 %>
* <%= :输出数据到模板（输出是转义 HTML 标签）%>
* <%- :输出非转义的数据到模板 %>


# 例子1
Data
```
  {
     name:"ckl",
     age:18
  }
```
Template
```
<div>
  My name is <%=name%> and i am <%=age%> years old.
</div>
```
Result
```
<div>
  My name is ckl and i am 18 years old.
</div>
```
Solution

https://www.w3school.com.cn/jsref/jsref_obj_regexp.asp

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#指定一个函数作为参数
```
  var tmpl = `<div>
  My name is <%=name%> and i am <%=age%> years old.
  
  </div>`
  var data = {
      name:"ckl",
      age:18
   }

  var res = tmpl.replace(/<%=([\s\S]+?)%>/g,(p1,p2,p3)=>{
      console.log(p1);
      console.log(p2);
      console.log(p3);
      return data[p2]
  })

  console.log("============")
  console.log(res);
  console.log("============")

```
# 例子2
Data
```
  {
     name:"ckl",
     age:18,
     contact:{
               address:"xxxxxxxx",
               phone:888
     }
  }
```
Template
```
<div>
  My name is <%=name%> and i am <%=age%> years old.
  I live in <%=contact.address%> and my phone number is <%=contact.phone%>.
</div>
```
Result
```
  <div> My name is ckl and i am 18 years old. I live in xxxxxxxx and my phone number is 888. </div>
```
Sulotion
* 必须过滤制表符 tmpl.replace(/[\r\t\n]/g, " ")
* 不直接替换变量，而是变成下一步函数类执行体
* 使用函数类
```
var tmpl = `<div>
My name is <%=name%> and i am <%=age%> years old.
I live in <%=contact.address%> and my phone number is <%=contact.phone%>.
</div>`
var data =   {
    name:"ckl",
    age:18,
    contact:{
       address:"xxxxxxxx",
       phone:888
    }
 }

var res = tmpl.replace(/[\r\t\n]/g, " ").replace(/<%=([\s\S]+?)%>/g,(p1,p2,p3)=>{
    console.log(p1);
    console.log(p2);
    console.log(p3);
    // return data[p2]
    return "' + obj."+ p2 +" + '"
})

var fn = new Function('obj', 'return ' + "'"+res+"'"); 
console.log("============")
console.log(fn(data));
console.log("============")
```
# 例子3
Data
```
var data =   {
    name:"ckl",
    age:18,
    contact:{
        address:"xxxxxxxx",
        phone:888
    },
    coin:['ETH','BTC']
 }
```
Template
```
<div>
  My name is <%=name%> and i am <%=age%> years old.
  I live in <%=contact.address%> and my phone number is <%=contact.phone%>.
  My favorite coin are:
    <ul>
      <% arr.forEach(function(item){ %>
        <li><%=item%></li>
      <% }); %>
    </ul>
</div>
```
Result
```
<div> 
  My name is ckl and i am 18 years old. I live in xxxxxxxx and my phone number is 888. My favorite coins are: 
  <ul><li>ETH</li><li>BTC</li></ul>
</div>
```
Sulotion
* 使用apply绑定函数this
* 使用exec
  https://www.w3school.com.cn/jsref/jsref_exec_regexp.asp
* 正则测试https://regexr.com
```
var tmpl = `<div>
My name is <%=name%> and i am <%=age%> years old.
I live in <%=contact.address%> and <%my phone number is <%=contact.phone%>.
My favorite coins are:
<ul>
<% coins.forEach(function(item){ %>
    <li><%= item %></li>
<% }); %>
</ul>
</div>`
var data =   {
    name:"ckl",
    age:18,
    contact:{
        address:"xxxxxxxx",
        phone:888
    },
    coins:["ETH","BTC"]
 }
tmpl = tmpl.replace(/[\r\t\n]/g, " ")
var scpt = []
var index = 0

var patt = new RegExp(/<%=([^<%=]+?)%>|<%([^<%=]+?)%>/,"g");
while(match=patt.exec(tmpl)){
    scpt.push("arr.push('"+tmpl.slice(index,match.index)+"');")
    if(match[1]){
        scpt.push("arr.push(" + match[1] +");")
    }
    if(match[2]){
        scpt.push(match[2])
    }
    index = match.index + match[0].length
}
// var res = tmpl.replace(/<%=([\s\S]+?)%>|<%([\s\S]+?)%>/g,(p1,p2,p3,p4)=>{
//     scpt.push("arr.push('"+tmpl.slice(index,p4)+"');")
//     if(p2){
//         scpt.push("arr.push(" + p2 +");")
//     }
//     if(p3){
//         scpt.push(p3)
//     }
//     index = p4 + p1.length
//     return `___`
// })
scpt.push("arr.push('"+ tmpl.substring(index,tmpl.length) +"');")
var fn = new Function("var arr=[]; with(this){"+ scpt.join("") +"}; return arr.join('');"); 

console.log("============")
console.log(scpt);
console.log(fn.apply(data))
console.log("============")
```
