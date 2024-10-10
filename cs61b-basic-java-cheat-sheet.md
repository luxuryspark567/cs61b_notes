---
description: Robert Lu
---

# CS61B Basic Java Cheat Sheet

## Java

* `javac` is used to compile programs. `java` is used to execute programs. `new` keyword is used to instantiate a class, e.g. `Dog d = new Dog()`. An instance of a class in Java is also called an “Object”. Constructors tell Java what to do when a program tries to create an instance of a class, e.g. what it should do when it executes `Dog d = new Dog()`. constructor不需要声明返回值。
* **Array Instantiation.**&#x20;
  * Specifies the size of the array, and fills the array with default values, `int[] x;`
  * Fill up the array with specific values, `int[] x = new int[]{1, 2, 3, 4, 5};` `int[] w = {1, 2, 3, 4, 5};`
  * Using array indexing set a value in an array, `A[3] = 4`.&#x20;
* **Arraycopy** In order to make a copy of an array, we can use <mark style="color:red;">`System.arraycopy`</mark>. It takes 5 parameters; the syntax is hard to memorize, so we suggest using various references online such as [this](https://www.tutorialspoint.com/java/lang/system\_arraycopy.htm)
*   **Generic Type Array** <mark style="color:red;">Generic arrays are not allowed in Java</mark>. Instead, we will change the line (Type is T):

    <pre class="language-java"><code class="lang-java"><strong>items = new T[100]; //this is not allowed
    </strong></code></pre>

    to:

    ```java
    items = (T[]) new Object[100];
    ```
* **Difference between static and non-static variables / functions.**
  * Static members are invoked using the class name: Math.sqrt
    * <mark style="color:purple;">Cannot access instance variables.</mark>
    * <mark style="color:purple;">Everythin is known at compile time for static members, and it won't change during runtime, most likely.</mark>
  * Intsance members are invoked using an instance name: m.maxDog
    * Asa  result we can access instance variabes.
* **Static vs. Instance**
  * **Static** variables and functions <mark style="color:red;">belong to the whole class</mark>. Example: Every 61B Student shares the same professor, and if the professor were to change it would change for everyone.
  * **Instance** variables and functions <mark style="color:red;">belong to each individual instance</mark>. Example: Each 61B Student has their own ID number, and changing a student’s ID number doesn’t change anything for any other student.
  * **Static vs. Instance methods.** Instance methods are actions that can only be taken by an instance of the class (i.e. a specific object), e.g. `d.bark()`, whereas static methods are taken by the class itself, e.g. `Math.sqrt()`.&#x20;
  * Instance method是类的对象（Instance or Object）调用的，Static method是Class调用的，也就是说不需要将Class实例话就可以直接调用的方法，特别是定义的函数方法类很有用，因为不可能调用函数之前先要定义一个对象。
  * **Static variables.** Variables can also be static. Static variables should be accessed using the class name, e.g. `Dog.binomen` as opposed to `d.binomen`. Technically Java allows you to access using a specific instance, but we strongly encourage you not to do this to avoid confusion.
  * Static成员（method，或者是IntNode这样的Class成员）不能调用instance 成员。因为Static是所有instance共有的，如果调用了instance成员，那么instance是指的哪个instance呢？无法明确，因此无法使用。
* **What are some scenarios where we want public vs. private?**
  * Public means: I can document that this function or variable exists, and you should feel free to use it and expect it will be in my code forever.
  * Private means: Do not use this, I could easily delete it tomorrow.
    * Exampel ArrayDeque, you come up with a clever way to use two arrays to make your code faster.
    * Now instead of items,w e jhave items1 and items2.&#x20;
    * If items were private, I can delete items entirely and nobody’s code will break.
* **Test-Driven Development** Test-Driven Development (TDD), where the programmer writes the tests for a function BEFORE the actual function is written.&#x20;
* **JUnit Tests** JUnit is a package that is used to debug programs in Java, `assertEquals(expected, actual)`. Other JUnit functions:`assertEquals`, `assertFalse`, and `assertNotNull`. Good practice to write ‘@Test’ above the function that is testing.
* Primitive vs. Reference Types
  * Reference Types are represented by a memory address stored at the location of the variable which points to where the full object is (all objects are stored at addresses in memory). This memory address is often referred to as a pointer.Examples: Strings, Arrays, Linked Lists, Dogs, etc.
  * Primitive Types are represented by a certain number of bytes stored at the location of the variable in memory. There are only 8 in Java.Examples: byte, short, int, long, float, double, boolean, char&#x20;
* **Golden Rule of Equals (**GRoE**):** “Given variables y and x:  y = x copies all the bits from x into y.”
  * The value of a primitive type gets copied directly upon variable assignment, Ex. int x = 5; means that variable x stores the value of 5 (in bits)
  * The value of a reference type is a “shallow” copy upon variable assignment: the pointer (memory address) is copied, and the object itself in memory is not
* **== vs. .equals()**
  * \== compares the literal bits at the location of the variable so it only works for primitive types&#x20;
    * it will compare the memory addresses of two reference types
    * the exception to this is null - it’s a special pointer that is compared with ==
  * Alternative for reference types: .equals() (ex. myDog.equals(yourDog))
    * This can be overridden on a class-by-class basis, but defaults to Object’s .equals() (which just compares memory addresses!)
* **this vs. static**
  * this: Non-static methods can only be called using an instance of that object, so during evaluation of that function, you will always have access to this instance of the object, referred to as this
  * static methods: do not require an instance of that object in order to be called, so during evaluation of that function, you cannot rely on access to this instance of the object
  * static variables: shared by all instances of the class; each instance does not get its own copy but can access&#x20;
* **Creating Objects**&#x20;
  * Create an instance of a class using the `new` keyword, <mark style="color:red;">Java creates boxes of bits for each field, where the size of each box is defined by the type of each fiel</mark>d,e.g., if a Walrus object has an `int` variable and a `double` variable, then Java will allocate two boxes totaling 96 bits (32 + 64) to hold both variables. <mark style="color:red;">These will be set to a default value like 0</mark>.&#x20;
  * The constructor then comes in and fills in these bits to their appropriate values. <mark style="color:red;">The return value of the constructor will return the location in memory where the boxes live, usually an address of 64 bits</mark>.&#x20;
  * This address can then be stored in a variable with a “reference type.”\


## List

* **Public vs. Private** Define a member as priviate:
  * to prevent other users to use a method.
  * to force users to access via public methods only, not by directly using dot.
* **Nested Classes** Declare the nested classes to be private to prevent other classes to use this nested class.

```java
public class SLList {	
	private static class IntNode {
		public int item;
		public IntNode next;
		public IntNode(int i, IntNode n) {
			item = i;
			next = n;
		}
	}
	private IntNode sentinel;
	private int size;
 	public void addFirst(int x) {...}
 	public int getFirst() {...}
 	public void addLast(int x) {...}
 	public int size() {...}
}
```

* **Static Nested Classes** If the `IntNode` class never uses any variable or method of the `SLList` class, we can turn this class static by adding the “static” keyword. <mark style="color:red;">为什么要把nested class声明为Static？</mark>
* **Recursive Helper Methods** A common idea is to write an outer method that users can call. <mark style="color:blue;">This method calls a private helper method that takes certain variable as a parameter.</mark> This helper method will then perform the recursion, and return the answer back to the outer method.
* **Caching** Add a variable to save certain characteristic of the class, to avoid dynamic calculation every time, e.g., define size to cache the size of the element.
* **Sentinel Nodes** Add a sentinel will make all `SLList` objects, even empty lists, the same.&#x20;

<figure><img src=".gitbook/assets/image (22).png" alt="" width="291"><figcaption></figcaption></figure>

* **DLList** D<mark style="color:red;">doubly-linked list, or</mark> <mark style="color:red;"></mark><mark style="color:red;">`DLList`</mark>. With this modification, adding and removing from the front and back of our list becomes fast (although adding/removing from the middle remains slow).
* **Incorporating the Sentinel** Recall that we added a sentinel node to our `SLList`. <mark style="color:red;">For</mark> <mark style="color:red;"></mark><mark style="color:red;">`DLList`</mark><mark style="color:red;">, we can either have two sentinels (one for the front, and one for the back), or we can use a circular sentinel.</mark> <mark style="color:red;">A</mark> <mark style="color:red;"></mark><mark style="color:red;">`DLList`</mark> <mark style="color:red;"></mark><mark style="color:red;">using a circular sentinel has one sentinel.</mark>&#x20;

<figure><img src=".gitbook/assets/image (67).png" alt="" width="375"><figcaption></figcaption></figure>

* <mark style="color:red;">**Generic Types**</mark> How can we modify our `DLList` so that it can be a list of whatever objects we choose? Recall that our class definition looks like this:&#x20;

```java
public class DLList { ... }
```

<pre class="language-java"><code class="lang-java"><strong>public class DLList&#x3C;T> { ... }
</strong></code></pre>

* <mark style="color:red;">where</mark> <mark style="color:red;"></mark><mark style="color:red;">`T`</mark> <mark style="color:red;"></mark><mark style="color:red;">is a placeholder object type</mark>. In our `DLList`, our item is now of type `T`, and our methods now take `T` instances as parameters. <mark style="color:red;">On list creation, the compiler replaces all instances of</mark> <mark style="color:red;"></mark><mark style="color:red;">`T`</mark> <mark style="color:red;"></mark><mark style="color:red;">with</mark> <mark style="color:red;"></mark><mark style="color:red;">`String:`</mark>&#x20;

```java
DLList<String> list = new DLList<>("bone");
```

* **AList** The `AList` will have the same API as our `DLList`, API is the same as `DLList` (`addLast()`, `getLast()`, `removeLast()`, and `get(int i)`). The `AList` will also have a `size` variable that tracks its size.
* **Array Resizing** When the array gets too full, we can resize the array. The solution is, instead, to create a new array of a larger size, then copy our old array values to the new array.&#x20;
* **Improving Resize Performance** Instead of adding by an extra box, we can instead create a new array with `size * FACTOR` items, where `FACTOR` could be any number, like 2 for example.&#x20;

```java
public class AList<T> {
    private T[] items;
    private int size;
    public AList() {
        items = (T[]) new Object[100];
        size = 0;
    }
    private void resize(int capacity) {...}
    public void addLast(T x) {...}
    public T getLast() {...}
    public T get(int i) {...}
    public int size() {...}
    public T removeLast() {...}
}
```

## Inheritance

**Interfaces** Interfaces specify what this `List` can do, and the classes inheritate it specify how to do it.

<pre class="language-java"><code class="lang-java"><strong>public interface List&#x3C;Item> {
</strong><strong>... 
</strong><strong>int largestNumber (List l) { ... }
</strong><strong>}
</strong>public AList&#x3C;Item> implements List&#x3C;Item> {
...
@Override
int largestNumber (List l) { ... }
}
public SLList&#x3C;Item> implements List&#x3C;Item> { ... }
</code></pre>

* **“is-a” relationship**: `AList` is a `List, SLList` is a List.
* **Interface makes code general**. Methods take a `List` parameter could take both `AList` and `SLList`, e.g. largstNumber( List l).
* **Overriding** For each method in `AList` that we also defined in `List`, and add an @Override right above the method signature.

**Interface Inheritance** Formally, we say that subclasses inherit from the superclass. Interfaces contain all <mark style="color:purple;">the method signatures</mark>, and <mark style="color:red;">each subclass</mark> <mark style="color:red;"></mark><mark style="color:red;">**must**</mark> <mark style="color:red;"></mark><mark style="color:red;">implement every single signature</mark>.

**Default Methods** Interfaces can have default methods and implement these methods inside the interface (No instance variables to use, and use the methods that are defined in the interface). <mark style="color:red;">Default methods should work for any type of object that implements the interface</mark>! We can still override default methods, and re-define the method in our subclass.

```java
default public void method() { ... }
```

**Syntax Class Inheritance vs Interface Inheritance**

```java
SLList<Blorp> implements List61B<Blorp>{...}

Class_Name<Item> extends Class_Name<Item>{...}
```

**What is Inherited?** For now, we will say that we can inherit:

* Instance and static variables.
* All methods.
* All nested classes.&#x20;

But members may be private and thus inaccessible, and constructors are not inherited.

**The Special Case of the Constructor?** Use super() to call parent constructor.

* Constructor\`s with NO arguments are implicitly called.&#x20;
* Constructor\`s with arguments are not implicitly called. Should be called with super(x).

**Encapsulation** When building large programs, our enemy is complexity. Some tools for managing complexity:

1. Hierarchical abstraction.&#x20;
   * Create layers of abstraction, with clear abstraction barriers
2. “Design for change” (D. Parnas)
   * [Organize program around objects.](#user-content-fn-1)[^1]
   * Let objects decide how things are done.
   * Hide information others don’t need.

**Static vs. Dynamic Type**

* **Static Type** The type specified <mark style="color:red;">when the variable is declared</mark>, and is <mark style="color:orange;">checked at compile time</mark>.&#x20;
* **Dynamic Type** The type is specified <mark style="color:red;">when the variable is instantiated</mark>, and <mark style="color:orange;">is checked at runtime.</mark>

**Casting** Casting allows us to force the static type of a variable, basically tricking the compiler into letting us force the static type of am expression.&#x20;

<mark style="color:red;">Casting, while powerful is also quite dangerous</mark>. You need to ensure that what you are casting to can and will actually happen. There are a few rules that can be used:

* Always cast up (to a more generic version of a class), e.g., a Poodle is a Dog;
* Can also cast down (to a more specific version of a class), e.g., a Dog might be a Poodle; &#x20;
* Never cast to a class that is neither above or below the class being cast, e.g., a Poodle is never a Monkey.

**Dynamic Method Selection, Revisited**

* Compile Time, we only care about static type of the invoking / calling instance:
  * Check for valid variable assignments
  * Check for valid method calls (only considering static type and static types superclass(es))
    * <mark style="color:red;">Lock in</mark> exact method signature as soon as we find an adequate one, traversing parent classes
  * If nothing found, compiler error
* Run Time, we care about dynamic type of the invoking / calling instance:
  * 1.If the locked-in method is static, skip the step below and just run that method
  * 2.Check for overridden methods
    * a.Does the locked-in method signature have an identical one in the dynamic class or the dynamic class’s parent classes?
  * 3\.  Ensure casted objects can be assigned to their variables
* **e.g. 题目1中：((Animal) c).greet(d)**
  * Compile Time: c的static type是Cat，Casting为Animal之后，调用Animal的greet；d的static type是Dog，在Animal中最符合的是void greet(Animal a)，因此Compile Time分析的结果为：调用Animal.greet(Animal)；
  * Runtime: c的dynamic type是cat，因此首先找到Cat中的greet函数。<mark style="color:red;">虽然实际的输入参数d是Dog，而且Cat也有输入为Dog的greet函数，但是因为在Compile Time的时候，输入参数已经locked as Animal，因此调用Cat.greet(Animal)，而不是Cat.greet(Dog)；</mark>

**Overloading vs Overriding and Dynamic Method Selection** Dynamic method selection only applies when we have overridden methods, and plays no role when it comes to overloaded methods.

**Overriding** The rule is, if we have a static type `X`, and a dynamic type `Y`, then if `Y` overrides the method from `X`, then on runtime, we use the method in `Y` instead.

**Overloading** If a function is defined to take static type X, and the overloaded function takes static type Y, the actual function called when this function is used depends on the static type of parameter.&#x20;

{% code lineNumbers="true" %}
```java
public static void define(Fox f) { ... }
public static void define(Animal a) { ... }

Fox f = new Fox();
Animal a = f;
define(f);
define(a);
```
{% endcode %}

Line 3 will execute `define(Fox f)`, while line 4 will execute `define(Animal a).`

* Compiler allows the memory box to hold any subtype.
* Compiler allows calls based on static type.
* <mark style="color:red;">Overriden non-static methods are selected at runtime based on dynamic type.</mark>
* For <mark style="color:red;">overloaded methods, the method is selected at compile time</mark>.

**High Order Funcitons** As old school style in Java 7, memory boxes (variables) cannot contain pointer to functions. Can use an interface instead. Take `y = f(f(x))` for example:

```java
public interface IntUnaryFunction {
  int apply(int x);
}
public class TenX implements IntUnaryFunction {
  public int apply(int x) {
     return 10 * x;
  }
}

public class HoFDemo {
	public static int do_twice(IntUnaryFunction f, int x) {
   		return f.apply(f.apply(x));
	}
	public static void main(String[] args) {
   		System.out.println(do_twice(new TenX(), 2));
	}
}
```

1，为什么do\_twice使用了static关键字：为了在调用的时候不做实例化；

2，为什么需要定义一个interface：因为java7是不支持函数作为输入的，因为function是无类型的；<mark style="color:red;">为了将函数作为输入变量，需要将添加一个interface作为媒介，并且定义相应的class实现这个interface</mark>。对应的Python代码如下：

```python
define tenX(x):
    return 10 * x
define do_twice(f, x):
    return f(f(x))
print(do_twice(tenX, 2))
```

**Comparable**  “**one true max method**”. Any object that needs to be compared must implement the `compareTo` method for picking the ordering of the objects, given two objects<mark style="color:red;">.</mark>

```java
public class Dog implements Comparable<Dog> {
  ...
    @Override
    public int compareTo(Dog uddaDog) {
        //assume nobody is messing up and giving us
        //something that isn't a dog.
        return size - uddaDog.size;
    }... 

public class Maximizer {
  public static Comparable max(Comparable[] items) {
    int maxDex = 0;
    for (int i = 0; i < items.length; i += 1) {
      int cmp = items[i].compareTo(items[maxDex]);

      if (cmp > 0) {
        maxDex = i;
      }
    }
    return items[maxDex];
  }
} 
```

Comparable already works for things like `Integer`, `Character`, and `String`; moreover, these objects <mark style="color:green;">have already implemented a</mark> <mark style="color:green;"></mark><mark style="color:green;">`max`</mark><mark style="color:green;">,</mark> <mark style="color:green;"></mark><mark style="color:green;">`min`</mark><mark style="color:green;">, etc. method</mark> for you.&#x20;

继承了Comparable，实现了CompareTo的Class，可以直接调动Collections中的max函数（及其他比较函数）。

```java
import java.util.Collections;

public class DogLauncher {
  public static void main(String[] args) {
    Dog[] dogs = new Dog[]{d1, d2, d3};
    System.out.println(Collections.max(dogs));
  }
}
```

**Comparators**&#x20;

* `Comparable`’s `compareTo` method is the “Natural Order” compare.&#x20;
* `Comparator` is <mark style="color:red;">to the way achieve other ordering</mark>.

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

<figure><img src=".gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

```java
package lec10_inheritance3;

public interface Comparator<T> {
    public int compare(T x1, T x2);
}
```

```java
package lec10_inheritance3;

public class Dog implements Comparable<Dog> {
    public String name;
    private int size;

    public Dog(String n, int s) {
        name = n;
        size = s;
    }

    @Override
    public int compareTo(Dog uddaDog) {
        //assume nobody is messing up and giving us
        //something that isn't a dog.
        return size - uddaDog.size;
    }

    public static class NameComparator implements Comparator<Dog> {
        public int compare(Dog d1, Dog d2){
            return d1.name.compareTo(d2.name);
            //String already support compareTo function.
        }
    }

    public static class SizeComparator implements Comparator<Dog> {
        public int compare(Dog d1, Dog d2){
            return d1.size - d2.size;
        }
    }
}
```

```java
package lec10_inheritance3;

public class DogLauncher {
    public static void main(String[] args) {
        Dog d1 = new Dog("Elyse", 3);
        Dog d2 = new Dog("Sture", 9);
        Dog d3 = new Dog("Benjamin", 15);
        Dog[] dogs = new Dog[]{d1, d2, d3};

        Dog d = (Dog) Maximizer.max(dogs);
        System.out.println(d.name);

        Comparator<Dog> cd = new Dog.NameComparator();
        if (cd.compare(d1, d3) > 0) {
            d1.bark();
        } else {
            d3.bark();
        }

        Comparator<Dog> cd2 = new Dog.SizeComparator();
        if (cd2.compare(d1, d3) > 0) {
            d1.bark();
        } else {
            d3.bark();
        }
    }
}
```

1，<mark style="color:purple;">为什么NameComparator和SizeComparator前面加Static？</mark>这两个是nested class，加了static之后，不需要instantiate Dog就可以调用compare，而且这个类也不需要调用Dog Class中的instance variables。

### Iterators and Iterables  <a href="#iterators-and-iterables" id="iterators-and-iterables"></a>

Simplified interface for Iterator:

```java
public interface Iterator<T> {
  boolean hasNext();
  T next();
}
```

Here is a simplified interface for Iterable:

```java
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

To support <mark style="color:purple;">the enhanced for loop</mark>:

1. Add an iterator() method to your class that returns an Iterator\<T>.
2. The Iterator\<T> returned should have a useful hasNext() and next() method.
3. Add implements Iterable\<T> to the line defining your class.

Notice that in order for an object to be _iterable_, it must include a method that returns an _iterator_. The iterator is the object that actively steps through an iterable object.

<pre class="language-java"><code class="lang-java">import java.util.Iterator;

public class ArraySet&#x3C;T> <a data-footnote-ref href="#user-content-fn-2">implements Iterable&#x3C;T></a> {
    ...
    public Iterator&#x3C;T> iterator() {
        return new ArraySetIterator();
    }
    private class ArraySetIterator implements Iterator&#x3C;T> {
        private int wizPos;
        public ArraySetIterator() {wizPos = 0;}
        public boolean hasNext() {return wizPos &#x3C; size;}
        public T next() {
            T returnItem = items[wizPos];
            wizPos += 1;
            return returnItem;
        }
    }
    public static void main(String[] args) {
        ArraySet&#x3C;Integer> aset = new ArraySet&#x3C;>();
        ...
        for (int i : aset) {...}
    }
}
</code></pre>

### toString  <a href="#tostring" id="tostring"></a>

The `toString()` method returns <mark style="color:purple;">a string representation of objects.</mark> 默认Object的toString method是打印内容，如果是primitive type，就打印对应的值，如果是representative type，就打印指针地址。如果需要打印结构体内部的内容，就要Overrided toString函数设定打印的内容和格式。

### == vs .equals <a href="#vs-equals" id="vs-equals"></a>

We have two concepts of equality in Java- “==” and the “.equals()” method. The key difference is that when using&#x20;

* \==: checking <mark style="color:red;">if two objects have the same address in memory</mark> (that they point to the same instance or object).
* .equals(): is a method that can be overridden by a class and can be used to define some custom way of determining equality.

<mark style="color:blue;">`o instanceof Dog uddaDog`</mark> checks if o is an instance of Dog, and cast o into a new variable uddaDog.

```java
@Override
public boolean equals(Object o) {
   if (o instanceof Dog uddaDog) {
       return this.size == uddaDog.size;
   }
   return false;
}
```

[^1]: 以数据为中心

[^2]: 
