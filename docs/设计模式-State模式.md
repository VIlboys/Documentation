# 设计模式 之 State模式

<font size="5">一，概述</font>

> 不同状态下执行的方法，这就是State模式

<font size="4">这是设计模式中最简单的例子，当然也可以理解为多态，因为实际上就是一个多态的例子</font>

<font size="4">二，代码</font>

<font size="4">1，MMState.java</font>

```java
package com.bjq.dp.state;

public abstract class MMState {

	public abstract void smile();
	
	public abstract void cry();
	
	public abstract void say();
}
```

<font size="4">2，MMHappyState.java</font>

```java
package com.bjq.dp.state;

public class MMHappyState extends MMState {

	@Override
	public void smile() {
		
//		这是开心的微笑
		System.out.println("这是开心的微笑");
	}

	@Override
	public void cry() {
//		这是开心的哭泣
		System.out.println("这是开心的哭泣");
		
	}

	@Override
	public void say() {
//		开心时候说点什么
		System.out.println("开心时候说点什么");
	}

}
```

<font size="4">3，MMUnhappyState.java</font>

```java
package com.bjq.dp.state;

public class MMUnhappyState extends MMState {

	@Override
	public void smile() {
//		这是不开心时候的微笑
		System.out.println("这是不开心时候的微笑");
	}

	@Override
	public void cry() {
//		不开心的哭泣
		System.out.println("不开心的哭泣");
	}

	@Override
	public void say() {
//		不开心说点什么
		System.out.println("不开心说点什么");
	}

}
```

<font size="4">4，Girl.java</font>

```java
package com.bjq.dp.state;


/**
 * 女孩
 * @author jealous
 *
 */
public class Girl {

	private String name;
    //也可以换成不开心 的MMUnHappyState
	private MMState state = new MMHappyState();
	
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
//	很多时候呢，mm的 有些动作有多面性
	
	/**
	 * 这三个方法呢都有不同的状态，就会有不同的实现
	 */
	public void smile() {
		state.smile();
	}
	
	
	public void cry() {
		state.cry();
	}
	
	
	public void say() {
		state.say();
	}
	
	
}
```

<font size="4">5，Boy.java</font>

```java
package com.bjq.dp.state;

import java.util.ArrayList;
import java.util.List;

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
	
	public void doSomeThing() {
		
    }
	
}
```

<font size="4">6，Test.java</font>

```java
package com.bjq.dp.state;

public class Test {

	public static void main(String[] args) {
		
		Girl girl = new Girl();
		
		girl.smile();
		girl.cry();
		girl.say();
		
	}
	
}
```

输出结果：

```
这是开心的微笑
这是开心的哭泣
开心时候说点什么
```

