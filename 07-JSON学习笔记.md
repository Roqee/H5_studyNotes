# JSON

## 1. JSON 概述

### 1.1 什么是JSON，有什么用？

**JSON 是一种行业内的数据交换标准格式，JSON 在 JavaScript 中以对象的形式存在。**

* JavaScript Object Notation（JavaScript对象标记），简称JSON。（数据交换格式）

* JSON主要的作用是：一种标准的数据交换格式。（目前非常流行，系统A与系统B交换数据的话，都是采用JSON。）





### 1.2 JSON 特点

* JSON是一种标准的轻量级的数据交换格式。特点是：体积小，易解析。

* 在实际的开发中有两种数据交换格式，使用最多，其一是JSON，另一个是XML。
* XML体积较大，解析麻烦，但是有其优点：语法严谨。（通常银行相关的系统之间进行数据交换的话会使用XML。）



### 1.3 创建JSON

```javascript
        // JSON对象也可以称为无类型对象。轻量级，轻巧。体积小。易解析
		var studentObj = {
        	 "sno":  "110",
         	 "sname": "张三",
         	 "sex": "男"
        };
		
```





### 1.4 访问JSON对象

```javascript
 var json = {
     "username": "zhangsan"
 }
 // JS中访问json对象的属性
 alert(json.username);
 alert(json["username"]);
```



### 1.5 JSON 数组及遍历

```javascript
        // JSON 数组
        var students = [
             {"sno":"110","sname":"张三","sex":"男"},
             {"sno":"120","sname":"李四","sex":"男"},
             {"sno":"130","sname":"王五","sex":"男"},
         ];
        // 遍历
        for (var i = 0; i < students.length; i++) {
            var stuObj = students[i];
         alert(stuObj.sno + ',' + stuObj.sname + ',' + stuObj.sex);
        }
```





### 1.6 eval() 函数

作用：将字符串当作一段JS代码解释并执行。



java连接数据库，查询数据之后，将数据在java程序中拼接成JSON格式的”字符串“，将json格式的字符串响应到浏览器。

也就是说Java响应到浏览器上的仅仅只是一个”JSON格式的字符串“，还不是json对象。

可以使用eval函数，将json格式的字符串转换成json对象。

```javascript
var fromJava = "{\"name\":\"zhangsan\",\"password\":\"123\"}"  // 这是程序发过来的“字符串”
```

```javascript
// 将以上的json格式的字符串转换成json对象
window.eval("var jsonObj = " + fromJava);
// 访问json对象
alert(jsonObj.name + "," + jsonObj.password);  //在前端取数据。
```





### JSON 面试题

在JavaScript中：[] 和 {} 有什么区别？

​	[] 是数组。

​	{} 是JSNO。





JSON 是一种传输数据的格式（以对象为样板，本质上就是对象，但用途有区别， 对象就是本地用的，json 是用来传输的） 

### JSON.parse();       //string — > json 

### JSON.stringify();      //json — > strin





















