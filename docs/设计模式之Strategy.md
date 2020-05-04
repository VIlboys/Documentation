## 设计模式之Strategy

追求自己的特色，不要什么都要去追求，什么都去追求那叫做平庸

### 主要目的：

1，有一个`DataSorter`的类实现了对 Cat类 ，Dog类，以及其他的一些动物类的排序

2，`java`代码：

```java
import org.ietf.jgss.Oid;

public class DataSorter {


//	排序元素
	public static void sort(int[] a) {
		
	}
	
	
	public static void sort(Cat[] a) {
		
	}
    
    public static void sort(Dog[] a) {
		
	}
	
	
}
```

3，那这个时候会带来一些问题，比如我这个时候不止要对Cat类，Dog类，int类型这些类型进行排序，我还要对birdie类，`folat`类 等等一系列的类进行排序，这个时候这样写显然是没有可扩展性的，既然`DataSorter`类中的sort方法是要根据不同的类型采取不同的方法实现排序，那么具体实现交给子类去实现，且都实现同一个接口Comparable，那么`DataSorter`只需要对Comparable排序，这个时候就不需要关心要排序的的是什么类。

4，有如下这么几个类：

- DataSorter.java
- Comparable接口
- Cat.java
- Dog.java
- Test.java

5，这个模式的关系图如下：

![image-20200424194640002](C:\Users\jealous\AppData\Roaming\Typora\typora-user-images\image-20200424194640002.png)

6，代码如下：

1，`Comparable.java`

```java

public interface Comparable { 
//定义这个类主要是为了排序算法能够得到重复的使用， 不用说是每添加一个新类型，我都需要去写一个新的sort方法这样只会显得更加的没必要

	public int compareTo(Object o);
	
}
```

2，`Cat.java`

```java

public class Cat implements Comparable {

	private int height;
	
	private int weight;

	public Cat(int height, int weight) {
		super();
		this.height = height;
		this.weight = weight;
	}

	public int getHeight() {
		return height;
	}

	public void setHeight(int height) {
		this.height = height;
	}

	public int getWeight() {
		return weight;
	}

	public void setWeight(int weight) {
		this.weight = weight;
	}

	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return height + "|"+ weight;
	}

	@Override
	public int compareTo(Object o) {

		//1.首先判断这个对象是不是一个Cat对象
		if(o instanceof Cat) {
			//2.强制转为成为Cat对象
			Cat c = (Cat)o;
			//3.判断对象之间的大小
			if(this.getHeight() > c.getHeight()) return 1;
			else if(this.getHeight() < c.getHeight()) return -1;
			else return 0;
		}
		return -100;
	}	
}
```

3，`DataSorter.java`

```java
import org.ietf.jgss.Oid;

public class DataSorter {
	
//	提高可用性
	public static void sort(Object[] o) {
		
//		循环遍历里面的元素
		for (int i = o.length; i> 0; i--) {
			
			for (int j = 0; j <i-1; j++) {
				
//				强制转为换Comparable接口类型
				Comparable o1 = (Comparable)o[j];
				Comparable o2 = (Comparable)o[j+1];
				
				if(o1.compareTo(o2) == 1) {
					
					swap(o, j, j+1);
				}
				
			}
			
		}
		
	}
	
	

	private static void swap(Object[] o, int x, int y) {

		Object temp =o[x];
		o[x] = o[y];
		o[y] = temp;
		
	}



	public static void p(Object[] a) {

//		循环遍历里面所有的元素
		for (int i = 0; i < a.length; i++) {
			
			System.out.print(a[i]+" ");
			
		}
		
		System.out.println();
		
	}

------------------这是一条分隔符--------
//	排序元素
	public static void sort(int[] a) {
		
		for (int i = a.length; i> 0; i--) {
			
			for (int j = 0; j <i-1; j++) {
				
				if(a[j] > a[j+1]) {
					
					swap(a, j, j+1);
				}
				
			}
			
		}
		
	}
	
	
	public static void sort(Cat[] a) {
		
		for (int i = a.length; i> 0; i--) {
			
			for (int j = 0; j <i-1; j++) {
				
				if(a[j].getHeight() > a[j+1].getHeight()) {
					
					swap(a, j, j+1);
				}
				
			}
			
		}
		
	}
	
	

	private static void swap(Cat[] a, int x, int y) {

		Cat temp = a[x];
		a[x] = a[y];
		a[y] = temp;
	}

	private static void swap(int[] a, int x, int y) {
		
		int temp = a[x];
		a[x] = a[y];
		a[y] = temp;
		
	}
	
}

```

4，`Dog.java`

```java
import java.util.zip.Inflater;

public class Dog implements Comparable {
	
	private int food;
	
	

	public int getFood() {
		return food;
	}



	public void setFood(int food) {
		this.food = food;
	}

	


	public Dog(int food) {
		super();
		this.food = food;
	}



	@Override
	public String toString() {
		//return "Dog [food=" + food + "]";
        return food + " ";
	}



	@Override
	public int compareTo(Object o) {
//		1.首先判断这个对象是不是一条狗
		if(o instanceof Dog) {
//			2.强制转换为Dog类型
			Dog d = (Dog)o;
			if(this.food > d.getFood()) return 1;
			else if(this.food < d.getFood()) return -1;
			else return 0;
		}
//		如果传入过来的不是一条小狗返回-100
		return -100;
	}

}
```



4，`Test.java`

```java

public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		//int[] a = {9,7,5,3,1};
		
//		Cat[] a = {new Cat(5, 5),new Cat(3, 3),new Cat(1, 1)};
		Dog[] dogs = {new Dog(5),new Dog(3),new Dog(1)};
		DataSorter.sort(dogs);
		DataSorter.p(dogs);
		
	}

}
```

一，现在这样已经很好了，可以比较不同的类的大小，但是还是有一点小问题：在比较大小的时候条件我们都是写死的，下次如果要比较他的高度的时候这个代码又需要去改，那这个时候我们需要定义一个比较器Comparator接口，通过子类分别实现不同的条件的比较：

代码如下

1，`Comparator.java`

```java

public interface Comparator {
// 作为这个比较器
	int compare(Object o1,Object o2);
	
}

```

2，`CatHeightComparator.java`

```java

public class CatHeightComparator implements Comparator {

	@Override
	public int compare(Object o1, Object o2) {
		Cat cat1 = (Cat)o1;
		Cat cat2 = (Cat)o2;
		if(cat1.getHeight() > cat2.getHeight()) return 1;
		else if(cat1.getHeight() < cat2.getHeight()) return -1;
		else return 0;
	}
}
```

3，`CatWeightComparator.java`

```java
public class CatWeightComparator implements Comparator {
	@Override
	public int compare(Object o1, Object o2) {
		Cat cat1 = (Cat)o1;
		Cat cat2 = (Cat)o2;
		if(cat1.getWeight() > cat2.getWeight()) return -1;
		else if(cat1.getHeight() < cat2.getHeight()) return 1;
		else return 0;
	}
}
```

4，`Cat.java`

```java


public class Cat implements Comparable {

	private int height;
	
	private int weight;
	
//	实现JDK的Comparator接口使用泛型，指定类型
	private Comparator comparator = new CatHeightComparator();

	
	
	public Comparator getComparator() {
		return comparator;
	}

	public void setComparator(Comparator comparator) {
		this.comparator = comparator;
	}

	public Cat(int height, int weight) {
		super();
		this.height = height;
		this.weight = weight;
	}

	public int getHeight() {
		return height;
	}

	public void setHeight(int height) {
		this.height = height;
	}

	public int getWeight() {
		return weight;
	}

	public void setWeight(int weight) {
		this.weight = weight;
	}

	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return height + "|"+ weight;
	}

	@Override
	public int compareTo(Object o) {
		
//		比较的是当前的对象this以及传递过来的对象o
		return comparator.compare(this,o);
	}	
}
```

5，`Test.java`

```java
public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		//int[] a = {9,7,5,3,1};
		Cat[] a = {new Cat(5, 5),new Cat(3, 3),new Cat(1, 1)};
//		Dog[] a = {new Dog(5),new Dog(3),new Dog(1)};
//		java.util.Arrays.sort(a);
		DataSorter.sort(a);
		DataSorter.p(a);	
	}
}
```

###### 策略模式：进行比较大小的时候，定义一个策略的比较器，由具体的比较策略来比较谁大谁小

当然实际情况`JDK`中默认实现了这个排序的方法，需要去实现Comparable这个接口即可

End~

