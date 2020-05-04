# 工厂模式 之 Spring Bean工厂

需求：既能在产品系列可以有扩展，又能在产品的品种方面可以随意确定

这是Spring提供的一个解决方案，也就是Spring 的Bean工厂

<font size="4">目标：读取properties文件，获取类名来生成对象</font>

实现过程：

<font size="4">代码实现：</font>

<font size="4">1，Mobile接口</font>

```java
package com.bjq.spring.factory;

public interface Mobile {

	void run();

}
```

<font size="4">2，`Car.java`</font>

```java
package com.bjq.spring.factory;

public class Car implements Mobile {

	@Override
	public void run() {

		System.out.println("开着小汽车就过去了......");
		
	}

}
```

<font size="4">3，`spring.properties`</font>

```properties
VehicleTypeName=com.bjq.spring.factory.Train
```

<font size="4">4，`Test.java`</font>

```java
package com.bjq.spring.factory;

import java.io.IOException;
import java.util.Properties;

import org.apache.tools.ant.taskdefs.Move;

public class Test {

	public static void main(String[] args) throws Exception {

		Properties properties = new Properties();
//		用Properties去读取配置文件
		try {
			properties.load(Test.class.getClassLoader().getResourceAsStream("com/bjq/spring/factory/spring.properties"));
		} catch (IOException e) {
			e.printStackTrace();
		}
		String vehicleTypeName = properties.getProperty("VehicleTypeName");
		System.out.println(vehicleTypeName);
		//利用java反射机制 生成一个新的对象 ,把这个类加载到内纯，然后使用newInstance() 产生一个新的对象
		Object o = Class.forName(vehicleTypeName).newInstance();
		
		Mobile m =(Mobile)o;
		m.run();
		
	}
	
}

```

<font size="4">运行输出：</font>

```
com.bjq.spring.factory.Train
开着小火车冒着烟就过去了,.....
```

> 如果你想换成别的交通工具可以在配置文件里面改就行了

<font size="4">5，如果用Spring 来读取 `xml`文件，则导入Spring的包后，增加 `applicationContext.xml`文件</font>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

  <!-- 这就是Spring的Bean 容器 IOC 指的是面向接口和抽象编程，不要面向具体 -->
  <bean id="v" class="com.bjq.spring.factory.Train">
    	
  </bean>

</beans>
```

<font size="4">6，`Test.java`</font>

```java
package com.bjq.spring.factory;

import java.io.IOException;
import java.util.Properties;

import org.springframework.beans.factory.BeanFactory;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {
		BeanFactory f = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		Object o = f.getBean("v");
		
		Mobile m = (Mobile)o;
		
		m.run();
	}
	
}
```

