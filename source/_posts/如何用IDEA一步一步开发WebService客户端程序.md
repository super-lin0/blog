---
title: 如何用IDEA一步一步开发WebService客户端程序
date: 2016-10-25 19:52:18
updateed: 2016-10-25 19:52:18
tags: [Java, WebService]
---

<center>
  好不容易把 WebService 服务器端搭建起来，我们还需要客户端程序也同样跑起来才能够进行和服务器端程序的通信：
<center>
</br>
</center>
  标签：#Java, #WebService
</center>

<!-- more -->

好不容易把 WebService 服务器端搭建起来，我们还需要客户端程序也同样跑起来才能够进行和服务器端程序的通信：
在这篇文章里面，我会先自己写代码去实现调用 WebService 服务器端程序，接下来，通过 IDEA 配置的方式来调用 WebService 服务端：
首先，我写了一个 WebService 客户端程序来调用服务器端程序：

```
import org.apache.axis.client.Service;
import org.apache.axis.client.Call;
import javax.xml.namespace.QName;

/**
 * @param
 * @author wu.lin
 * @description
 * @create 2016年10月25日 19:51
 * @throws
 */
public class HelloWorldClient {
  public static void main(String[] argv) {

    Service service = new Service();
    String url = "http://localhost:8080/flight/services/HelloWorld?wsdl";

    try {
      Call call = (Call)service.createCall();
      call.setTargetEndpointAddress(new java.net.URL(url));
      call.setOperationName(new QName("http://example","sayHelloWorldFrom"));
      call.addParameter(new QName("from"),org.apache.axis.encoding.XMLType.XSD_STRING, javax.xml.rpc.ParameterMode.IN);
      call.setUseSOAPAction(true);
      call.setReturnType(org.apache.axis.encoding.XMLType.SOAP_STRING);
      String result = (String)call.invoke(new Object[]{"Tom"});

      System.out.println(result);

    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

运行这段程序，得到如下的结果：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTk1NDExOTQy)
嗯程序没有问题，接下来我们用 IDEA 来创建一个 WebService 客户端工程（注意这里选择 WebService 客户端工程）：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTk1NjU3MDM4)
点击 next:
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTk1OTA4MTc5)
Finsh,注意以下一些地方的填写;
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MjAwMDUzMjU4)
然后得到一个工程，IDE 会生成一些类，但是这些类可能会有一些错误，需要我们手动去修改，经过修改后得到的 HelloWorldClient 类 的代码如下：

```
 public static void main(String[] argv) {
    try {
      HelloWorldServiceLocator locator = new HelloWorldServiceLocator();
      HelloWorld_PortType service = locator.getHelloWorld();
      // If authorization is required
      //((HelloWorldSoapBindingStub)service).setUsername("user3");
      //((HelloWorldSoapBindingStub)service).setPassword("pass3");
      // invoke business method
      String str = service.sayHelloWorldFrom("Tom!");
      System.out.println(str);
    } catch (javax.xml.rpc.ServiceException ex) {
      ex.printStackTrace();
    } catch (java.rmi.RemoteException ex) {
      ex.printStackTrace();
    }
  }
```

还有单元测试类也会报错，是因为没有导入 Junit jar 包。有个解决方法，可以很快的导入 jar 包，比如在测试类中的任何一个方法上面加一个注解 @Test ， IDE 就会发现缺少 jar ，然后根据错误提示，把 Junit 加入到 classpath 中，就 OK 了。到此为止， axis 客户端的代码算是完成了。工程修改完之后，我们再运行：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MjAwNTMzNzQ0)
又报错了，找了很多解决方法都不行，最后我对比我们写的代码和工具生成的调用 WebService 方法，并对方法做修改：
下面类是 HelloWorldSoapBindingStub 中的代码：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MjAwODE4Mjky)
通过将第 57 行代码注释掉，我得到了正确结果：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MjAwOTM5MjQz)
至此，通过 IDEA 创建 WebService 客户端程序调通。
现在发现，每个人都会遇到一些不同的问题，只要耐心细心的去调试，总能够得到正确结果的，快去构建自己的 WebService 程序吧，加油！

<p style="text-align: center;"><span style="font-size:18px;"><strong><span style="color:#ff00;"><span style="color:#ff0000;">友情提示：</span></span>请尊重作者劳动成果，如需转载本博客文章请注明出处！谢谢合作！</strong></span></p>

<p align="center"><strong><span style="font-size:18px;">【作者：吴林&nbsp;&nbsp;</span></strong><a target="_blank" href="https://super-lin0.github.io/"><strong><span style="font-size:18px;">https://super-lin0.github.io/</span></strong></a><strong>】</span></strong></p>
