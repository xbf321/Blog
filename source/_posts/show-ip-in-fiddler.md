title: 在 fiddler 下显示 ip 列
date: 2015-06-15
categories: tech
---

在fiddler中，默认是不显示远程ip的（不知道为啥默认不加上），需要手动处理。

<!--more-->

## 打开 fiddler 找到 Rules -> Customize Rules ..

## 找到 static function main函数，添加如下代码：

```
FiddlerObject.UI.lvSessions.AddBoundColumn("ServerIP", 120, "X-HostIP");
```

## 完整代码

```
static function Main() {
   var today: Date = new Date();
   FiddlerObject.StatusText = " CustomRules.js was loaded at: " + today;
   // Uncomment to add a "Server" column containing the response "Server" header, if present
   FiddlerObject.UI.lvSessions.AddBoundColumn("Server IP", 120, "X-HostIP");
}
```