# 什么是模版引擎
数据（Data）+ 模版（Template） = 结果(Result)

EJS:
https://ejs.bootcss.com

语法:
* <% :'脚本' 标签，用于流程控制，无输出
* %> :一般结束标签
* <%= :输出数据到模板（输出是转义 HTML 标签）
* <%- :输出非转义的数据到模板

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
  My name is <%= name%> and i am <%= age%> years old.
</div>
```
Result
```
<div>
  My name is ckl and i am 18 years old.
</div>
```

# 例子2
Data
```
  arr = ["A","B","C","D"]
```
Template
```
<div>
    <ul>
        <% arr.forEach(function(item){%>
          <li><%= item %></li>
        <% }) %>
    </ul>
</div>
```
