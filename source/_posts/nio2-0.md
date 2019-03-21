---
title: nio2.0
date: 2019-03-19 09:10:28
categories: java
tags: nio
comments: true
---

## jdk1.7中文件系统的新操作，重点掌握

### 接口Path,类Paths,Files

``` bash
Path path = Paths.get("E:/tmp/js.html");//也可以是一个文件夹的路径
System.out.println("文件的属性"+path.get***());
//还有toRealPath,toAbsolutePath等
System.out.println(path.toUri());//file:///E:/tmp/js.html做文件缓存的功能可能用得上
```
<!--more-->
### 以上示例有无js.html这个文件都是一样的，Path只是对传入路径的一些操作,那么如何判断文件或文件夹是否村在？

``` bash
if (Files.exists(path)){//判断文件或文件夹是否存在
    Files.copy(Paths.get(""),Paths.get(""));
    Files.delete(path);//deleteIfExists只删除存在的文件，所以不会抛异常
}
```


### 文件监测功能:(例如热部署)

``` bash
//创建文件监控对象
WatchService watchService = FileSystems.getDefault().newWatchService();
//注册文件监控的事件类型
path.register(watchService,StandardWatchEventKinds.ENTRY_CREATE,StandardWatchEventKinds.ENTRY_DELETE,StandardWatchEventKinds.ENTRY_MODIFY);
//循环监测文件
while (true){
    WatchKey watchKey = watchService.take();
    for (WatchEvent<?> event:watchKey.pollEvents()){
        System.out.println(event.context().toString()+"事件类型:"+event.kind());
    }
}
```
