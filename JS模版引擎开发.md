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
