# 字符串
字符串内插
```c#
string aFriend = "Bill";
Console.WriteLine($"Hello {aFriend.ToUpper()}");
```

# 基本数字类型
整型
- int
- long
- uint
- nint

浮点类型
- float
- double
- decimal

# xxx


# .Net Framework, .Net Core, Mono 和 Xaramin 区别
https://stackoverflow.com/questions/38063837/whats-the-difference-between-net-core-net-framework-and-xamarin

.Net Framework vs .Net Core vs Mono
- 出现时间：.Net Framework 的第一个版本大约在2002年前后推出的，Mono 1.0版本是在2004年发布，.Net Core的第一个版本，大约在2016年前后推出的
- 背后公司：.Net Framework 和 .Net Core 均是微软，而 Mono 之前是主要由 Xamarin 开发
- 开源闭源：.Net Framework闭源，Mono开源，.Net Core开源
- 跨平台：.Net Framework 只针对 Windows 平台，Mono 跨平台，.Net Core 跨平台，可以运行在 Win、MacOS和Linux上

Xamarin 是运行在 Mono 上的一个框架，用于构建 iOS, Android 和 Windows Phone 移动 APP。