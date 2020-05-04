# 设计模式 之 Command 模式

<font size="5">一，概述</font>

> 在一个类中要另外一个类干任何事情并且可以扩展

<font size="4">二，代码</font>

<font size="4">1，`Command.java`</font>

```java
package com.bjq.dp.command;

public abstract class Command {
	
	public  abstract void execute();
	public  abstract void unDo();

}
```

<font size="4">2，HumCommand.java</font>

```java
package com.bjq.dp.command;

public class HumCommand extends Command{

	@Override
	public void execute() {
		System.out.println("hum..");
	}

	@Override
	public void unDo() {
		 //回退操作
	}

}
```

<font size="4">3，ShoppingCommand.java</font>

```java
package com.bjq.dp.command;

public class ShoppingCommand extends Command{

	@Override
	public void execute() {
		System.out.println("购物");
	}

	@Override
	public void unDo() {
		 //回退操作
	}

}
```

<font size="4">4，Boy.java</font>

```java
package com.bjq.dp.command;

import java.util.ArrayList;
import java.util.List;

/**
 * 男孩
 * @author jealous
 *
 */
public class Boy {

	private String name;

    private List<Command> commands = new ArrayList<Command>();
	
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
	public void doSomeThing() {
		
	}

	public void addCommands(Command c1) {
		this.commands.add(c1);
	}

	public void executeCommands() {

//		把每个方法都去执行
		for (Command c : commands) {
			
			c.execute();
			
		}
	}
	
	public void undoCommands() {
	   //回退操作
	}

	
}
```

<font size="4">5，Girl.java</font>

```java
package com.bjq.dp.command;


/**
 * 女孩
 * @author jealous
 *
 */
public class Girl {

	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
	public void order(Boy boy) {
//		同时干很多事情
		Command c1  = new ShoppingCommand();
		boy.addCommands(c1);
		
		Command c2  = new HumCommand();
		boy.addCommands(c2);
		
		boy.executeCommands();
	}
	
	
}
```

<font size="4">6，Test.java</font>

```java
package com.bjq.dp.command;

public class Test {

	public static void main(String[] args) {
		
		Girl girl = new Girl();
		
		Boy boy = new Boy();
		girl.order(boy);
		
	}
	
}
```

