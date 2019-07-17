---
title: 如何用IDEA一步一步开发WebService服务器端
date: 2016-10-25 19:47:50
updateed: 2016-10-25 19:47:50
tags: [Java, WebService]
---

<center>
  最近在搞一些东西，例如 WebService，参考了很多教程，但是毕竟每一个环境都有不一样的问题，在此记录下我在开发这个过程当中遇到的一些问题。
<center>
</br>
</center>
  标签：#Java, #WebService
</center>

<!-- more -->

工具：IntelliJ IDEA 15.0.4
IDEA 这款 IDE 还是非常强大的，对 WebService 也有很好的支持。下面我们来一步一步的实现 WebService 服务器端：
第一步，新建一个工程：File->new->project。需要注意的看下面的图片：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkxMjMyMDI2)

点击 next:
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkxNDMwNzk0)

点击 Finish,我们得到的工程目录大概是下面这样子的：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkxNzE0NDQw)

然后我们看到有一个 Java 类。右键 HelloWorld.java:
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkyMDQwMzE4)

在这里生成 wsdl 文件进行配置：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkyMTM5Mzgw)

还有一个地方需要注意：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkzNDE2NTA4)
这里是有可能报错的，如果报错，点击下面的 Fix->Add ‘JAX-WS-Apache’ to the...嗯修复错误。

需要注意的地方是 Web Service URL 的地址的配置，我之前就是用的默认的 localhost:8080//services/......估计是没有配置 Tomcat，所以这里是两个/，所以我调了很久都没有搞出来，路径问题；配置好之后，发现 Java 文件下面多了一个 wsdl 文件。

下面配置 Tomcat，tomcat 配置比较简单，所以在此只贴几张图片出来：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkyODUwODg0)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkyOTA1MDA5)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkyOTI2NDE1)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkyOTQ1NjU4)

至此，Tomcat 配置完成，启动 Tomcat，访问http://localhost:8080/flight 显示 index.jsp 页面，说明 Tomcat 配置成功。当我们访问 WebService 的时候；路径为：
http://localhost:8080/flight/services/HelloWorld
结果显示![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkzNzU4MzU2)
找了很久也没有找到原因，然后发现当我输入下图的路径的时候出现的结果：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTkzOTI3MDkz)
仔细看这个图发现是缺少了我们自己配置的 HelloWorld，然后想一下应该是有个地方配置：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTk0MTQ2MTcw)

找到这里我仿照配置文件写了 HelloWorld 的配置：
重启 Tomcat 之后再次访问发现上面的图片多了我们刚才配置的项 HelloWorld：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTk0NDUyMjM0)

点进去看看：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTk0NTI2MTMy)

嗯就是我们要的结果了，至此，在用 Idea 搭建 WebService 服务器端的过程中遇到的全部问题我在这里做了一个总结：
下面一篇文章我会讲一下怎么搭建 WebService 客户端。

<p style="text-align: center;"><span style="font-size:18px;"><strong><span style="color:#ff00;"><span style="color:#ff0000;">友情提示：</span></span>请尊重作者劳动成果，如需转载本博客文章请注明出处！谢谢合作！</strong></span></p>

<p align="center"><strong><span style="font-size:18px;">【作者：吴林&nbsp;&nbsp;</span></strong><a target="_blank" href="https://super-lin0.github.io/"><strong><span style="font-size:18px;">https://super-lin0.github.io/</span></strong></a><strong>】</span></strong></p>
