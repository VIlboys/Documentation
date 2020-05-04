# 设计模式 之 Bridge 模式

<font size="5">一，概述</font>

<font size="4">1，桥接模式的应用情况：</font>

- <font size="4">两个维度扩展</font>
- <font size="4">排列组合 </font>

> 所谓的桥接模式就是用聚合来代替原来的继承的方式

<font size="4">二，实现过程</font>

![image-20200502221305028](C:\Users\jealous\AppData\Roaming\Typora\typora-user-images\image-20200502221305028.png)

<font size="4">三，代码：</font>

<font size="4">1，Gift.java</font>

```java
package com.bjq.dp.bridge;
/**
 *   礼物
 * @author jealous
 *
 */
public class Gift {

//	聚合
	GiftImpl impl;
	
}

```

<font size="4">2，GiftImpl.java</font>

```java
package com.bjq.dp.bridge;

public class GiftImpl {
}
```

<font size="4">3，Toy.java</font>

```java
package com.bjq.dp.bridge;

/**
 * 玩具类
 * @author jealous
 *
 */
public class Toy extends GiftImpl{

}
```

<font size="4">4，Flower.java</font>

```java
package com.bjq.dp.bridge;

/**
 * 鲜花类
 * @author jealous
 *
 */
public class Flower extends GiftImpl {

}
```

<font size="4">5，WarmthGift.java</font>

```java
package com.bjq.dp.bridge;

/**
 * 温暖类
 * @author jealous
 *
 */
public class WarmthGift extends Gift {

	
	public WarmthGift(GiftImpl impl) {
//		调用父类的
		 this.impl = impl;
	
	}
	
	@Override
	public String toString() {
			// TODO Auto-generated method stub
		return  this.getClass().getName() + "---的---" + impl.getClass().getName();
	}
	
}
```

> 这里如果换成`DoughtyGift` 也是同样的

<font size="4">6，Boy.java</font>

```java
package com.bjq.dp.bridge;

/**
 * 男孩
 * @author jealous
 *
 */
public class Boy {

	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
	/**
	 * 	 把Girl当做参数传递过来，和这个类中定义一个Girl的变量，虽然语法上没有多大的区别，但是在语义上有一些区别
	 * 
	 *   区别：
	 *   如果是当做参数传递,那么就表示Girl和Boy这个类没有多大的关系
	 *   如果你是当做成员变量的话，那就表示Girl是Boy的一部分 ,Girl和Boy之间的关系会变得更加的紧密
	 *   为什么呢?
	 *   因为一开始构造Boy的时候，里面就有Girl的引用
	 *   
	 *   在设计类的时候，一般是高内聚，低耦合，类之间的关系呢一般不建议太紧密
	 *   
	 * @param g
	 */
	public void present(Girl g) {
		
//		Gift gift = new Flower(); // 在这里指定什么就送她什么
//		give(gift,g);
		Gift gift = new WarmthGift(new Flower());
		System.out.println(gift);
	}

	private void give(Gift gift,Girl g) {

		
		
	}
	
}
```

<font size="4">7，Girl.java</font>

```java
package com.bjq.dp.bridge;
/**
 * 女孩
 * @author jealous
 *
 */
public class Girl {

}
```

