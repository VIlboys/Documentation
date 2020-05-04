# 工厂模式 之 用 Jdom 模拟 Spring ioc

一，概述

<font size="4">目标：模拟Spring 的 IOC</font>

> 首先把`JDOM`的jar包下载下来，然后导入到项目中，这是`JDOM`的官网链接：http://jdom.org/

<font size="4">有如下文件：</font>

<font size="4">1，BeanFactory接口</font>

```java
package com.bjq.spring.factory;

public interface BeanFactory {

	Object getBean(String id);
	
}
```

<font size="4">2，ClassPathXmlApplicationContext.java</font>

```java
package com.bjq.spring.factory;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.jdom.Document;
import org.jdom.Element;
import org.jdom.input.SAXBuilder;
import org.jdom.xpath.XPath;

public class ClassPathXmlApplicationContext implements BeanFactory {
	
	private Map<String, Object> container = new HashMap<String, Object>();
	
	public ClassPathXmlApplicationContext(String fileName) throws Exception {
		
		SAXBuilder sb = new SAXBuilder();
		  Document doc = sb.build(this.getClass().getClassLoader()
				  .getResourceAsStream(fileName));
		  Element root = doc.getRootElement();
		  List list = XPath.selectNodes(root, "/beans/bean");
		  
		  System.out.println(list.size());
		  
		  for (int i = 0; i < list.size(); i++) { 
		   Element bean = (Element) list.get(i);
		   String id = bean.getAttributeValue("id");
		   String clazz = bean.getAttributeValue("class");
		   
		   // 获取这个类的引用用反射创建一个新的对象
		   Object o = Class.forName(clazz).newInstance();
		   // 放入到Map集合当中 这就是Spring 的容器工厂 也就是IOC
		   container.put(id, o);
		   System.out.println(id + " "+ clazz);

		  }
	}

	@Override
	public Object getBean(String id) {
		
		return container.get(id);
	}
	

}
```

<font size="4">3，`applicationContext.xml`</font>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>

  <!-- 这就是Spring的Bean 容器 IOC 指的是面向接口和抽象编程，不要面向具体 -->
  <bean id="v" class="com.bjq.spring.factory.Car">
    	
  </bean>

</beans>
```



<font size="4">4，Movable.java</font>

```java
package com.bjq.spring.factory;

public interface Mobile {
	
	void run();

}
```

<font size="4">5，Car.java</font>

```java
package com.bjq.spring.factory;

public class Car implements Mobile {

	@Override
	public void run() {

		System.out.println("开着小汽车就过去了......");
		
	}

}
```

<font size="4">6，Train.java</font>

```java
package com.bjq.spring.factory;

import java.io.IOException;
import java.util.Properties;

public class Train implements Mobile {

	@Override
	public void run() {
		
		System.out.println("开着小火车冒着烟就过去了,.....");
		
	}

}
```

<font size="4">7，Test.java</font>

```java
package com.bjq.spring.factory;

import java.io.IOException;
import java.util.Properties;

import org.apache.tools.ant.taskdefs.Move;

public class Test {

	public static void main(String[] args) throws Exception {


		
		BeanFactory f = new ClassPathXmlApplicationContext("com/bjq/spring/factory/applicationContext.xml");
		
//		获取这个类的实例对象
		Object o = f.getBean("v");
//		强制转换为Mobile类型
		Mobile m = (Mobile)o;
		
		m.run();
		
		
	}
	
}

```

<font size="4">运行结果：</font>

```
1
v com.bjq.spring.factory.Car
开着小汽车就过去了......
```

