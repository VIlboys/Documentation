## OO思维(Object Oriented)

---

<font size="5">第一步：考虑类</font>

- 考虑类的时候，首先观察名词，比如 “**老张开车去东北**” 这个时候我们可以创建一个司机的类，当然也可以创建人的类

<font size="5">第二步：考虑属性</font>

- **不可脱离具体的应用环境**。举个例子：比如人这个Class 有无数个属性，name，sex，age 等等；如果你愿意的话可以写很多很多个，但是我们考虑的时候不要去考虑与应用环境无关的内容。跟具体无关的内容无需往里封装

<font size="5">第三步：考虑方法</font>

- 考虑方法时候我们也可以观察他的主语，有什么作为它的动作，比如说：司机开车，开车就是一个方法，也就是一个动词

<font size="5">第四步：类之间的关系</font>

- 

<font size="5">第五步：继承</font>

- 如果你说通了什么是一种什么，你就可以考虑使用继承，注意：要慎重用继承

<font size="5">第六步：多态</font>

- 我们可以定义一个父类，然后让子类继承他，让子类实现的他的方法，也可以说是对父类的方法不满意，子类重写这个方法。举个列子：我们定义了一个交通工具作为父类：然后它的子类有飞机，汽车 等等，父类交通工具里面有一个方法叫做run，但是交通工具的行驶方式是不一样的，所以我们子类，在飞机这个类下面，重写了这个run方法，实现了自己的行驶方式，然后调用的时候，你调用的是谁，就会调用谁的实现。这就叫做**多态**
- 多态的三个特性：有继承，有覆写，有父类的引用指向子类的对象
- 多态带来的好处呢：可扩展性（**Extensibility**），也是面向对象的精华

> 设计这个东西有时候是：仁者见仁，智者见智



## 设计模式之**iterator**

Java Iterator模式为Java Iterator模式 模仿`Java JDK`中的Collection，`ArrayList`，`LinkedList`

一，总共有如下这么几个类：

- Collection接口
- Iterator接口
- ArrayList.java
- LinkedList.java
- Node.java
- Dog.java
- Cat.java

1，Collection接口：

```java
public interface Collection {

	void add(Object o);
	
	int size();
	
	Iterator iterator();	
}
```

2，Iterator接口：

```java
public interface Iterator {

	Object next();
	
	boolean hasNext();
}
```

3，ArrayList.java

```java


public class ArrayList implements Collection {

//	定义一个容器
	Object[] objects = new Object[10];
	
	int index = 0;
	
//	定义一个add方法可以添加所有类型的Object
	public void add(Object o) {
//		1.判断数组是否满了
		if(index == objects.length) {
		
			//2.扩容这个数组
			Object[] newObjects = new Object[objects.length * 2];
			System.arraycopy(objects, 0, newObjects, 0, objects.length);
			objects = newObjects;
		}
//		3.为新添加的元素指定下标
		objects[index] = o;
//		4.index自身加一
		index ++;
		
	}
	
//	返回集合大小
	public int size() {
		return index;
	}
	
	public Iterator iterator() {
		return new ArrayListIterator();
	}
	
	
	private class ArrayListIterator implements Iterator {
		
		int currentIndex = 0;
		
		@Override
		public Object next() {
			 Object o =objects[currentIndex];
			 currentIndex ++;
			 return o;
		}

		@Override
		public boolean hasNext() {
//			判断是否为最后一个元素
			if(currentIndex >= index) return false;
			else return true;
		}
		
	}
	
}
```

4，LinkedList.java

```java

public class LinkedList implements Collection {

	Node head = null;
	Node tail = null;
	
	int size = 0;
	Node n = null;
//	基于链表实现的容器
	public void add(Object o) {
		
		n = new Node(o,null);
//		添加第一个节点表示
		if(head == null) {
			head = n;
			tail = n;
		}
		tail.setNext(n);
		tail = n;
		size ++;
		
		
	}
	
	public int size() {
		return size;
	}

	@Override
	public Iterator iterator() {
		// TODO Auto-generated method stub
		return new linkedListItertor();
	}
	
	
	private class linkedListItertor implements Iterator{

		private Node currentIndex = head;
		
		@Override
		public Object next() {
			// TODO Auto-generated method stub
			Object o = currentIndex.getData();
			currentIndex = currentIndex.getNext();
			return  o;
			 
		}

		@Override
		public boolean hasNext() {
			if(currentIndex.getNext() == null) return false;
			return true;
		}
		
	}
	
	
}
```

5，`Node.java`

```java

public class Node {

	private Object data;
	
	private Node next;

	public Object getData() {
		return data;
	}

	public void setData(Object data) {
		this.data = data;
	}

	public Node getNext() {
		return next;
	}

	public void setNext(Node next) {
		this.next = next;
	}

	public Node(Object data,Node next) {
		super();
		this.data = data;
		this.next = next;
	}
	
	
}
```

6，Dog.java

```java

public class Dog {
	
	private int id;
    
	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return "Dog:"+id;
	}
    
	public Dog(int id) {
		super();
		this.id = id;
	}
}
```

7，Cat.java

```java

public class Cat {

	private int id;

	public int getId() {
		return id;
	}

	public Cat(int id) {
		this.id = id;
		// TODO Auto-generated constructor stub
	}

	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return "cat:"+ id;
	}	
}
```

测试类：

```java

public class Test {
	
	public static void main(String[] args) {
		
//		ArrayList arrayList = new ArrayList();
//		LinkedList list = new LinkedList();
		Collection c = new LinkedList();
		for (int i = 0; i < 15; i++) {
			c.add(new Dog(i));
		}
		System.out.println(c.size());
		
		Iterator it = c.iterator();
		while(it.hasNext()) {
			Object o =it.next();
			System.out.print(o + "  ");
		}
	}

}
```

输出结果：

```shell
15
Dog:0  Dog:1  Dog:2  Dog:3  Dog:4  Dog:5  Dog:6  Dog:7  Dog:8  Dog:9  Dog:10  Dog:11  Dog:12  Dog:13  
```



## 最后总结一下这个模式：

实际上我们自定义了一个容器叫做`ArrayList`里面有两个方法，一个add 和一个size方法，同样可以往里面添加数据，获取这个容器的size(大小)，但是实际情况我们想换一个容器来实现比如`LinkedList`这个类，我们只要替换这个实现不同变动其他的代码，所以我们定义了一个Collection接口，里面定义这两个容器都有的方法比如add方法，然后这两个容器都去实现这个接口，这个时候我们容器扩展性就显示出来了，调用的时候就可以这么调用：`Collection c = new LinkedList`

如果不想使用`LinkedList` 换成`ArrayList`  这样也不用变更其他地方的代码

最后我们去遍历这个容器的内容，我们需要自定义一个Iterator 接口，里边有两个方法，hashNext 和Next，定义一个内部类，实现这个接口实现里边的方法，这样我们调用的时候就可以通过Iterator来遍历容器的内容

