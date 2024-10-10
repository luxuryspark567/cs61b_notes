# Discussion 5

1, **Dynamic Method Selection, Revisited**

Your computer. . .

@ Compile Time, we only care about static type of the invoking / calling instance:

1.Check for valid variable assignments

2.Check for valid method calls (only considering static type and static types superclass(es))

&#x20;       a.<mark style="background-color:orange;">Lock in exact method signature as soon as we find an adequate one, traversing parent classes</mark>

3.If nothing found, compiler error

@ Run Time, we care about dynamic type of the invoking / calling instance:

<mark style="background-color:blue;">1.If the locked-in method is static, skip the step below and just run that method</mark>

2.Check for overridden methods

&#x20;       a.Does the locked-in method signature have an identical one in the dynamic class or the dynamic class’s parent classes?

&#x20; <mark style="background-color:blue;">3.  Ensure casted objects can be assigned to their variables</mark>

**题目1中：((Animal) c).greet(d)**

1，Compile Time：c的static type是Cat，Casting为Animal之后，调用Animal的greet；d的static type是Dog，在Animal中最符合的是void greet(Animal a)，因此Compile Time分析的结果为：调用Animal.greet(Animal)；

2，Runtime: c的dynamic type是cat，因此首先找到Cat中的greet函数。<mark style="color:red;">虽然实际的输入参数d是Dog，而且Cat也有输入为Dog的greet函数，但是因为在Compile Time的时候，输入参数已经locked as Animal，因此调用Cat.greet(Animal)，而不是Cat.greet(Dog)；</mark>





**2, It seems to me that there are at least 3 ways to comment:**

* //: Line comment, only affects the current line.
* /\* \*/: Multi-line comment.
* /\*\* \*/: This is basically the same thing as the multi-line comment, but it’s more formal. For example, people put this type of comment before class or method declarations. There exists software that will take all the /\*\* \*/ comments and build nice HTML documentation called javadoc.



**3, What are some scenarios where we want public vs. private?**

* Public means: I can document that this function or variable exists, and you should feel free to use it and expect it will be in my code forever.
* Private means: Do not use this, I could easily delete it tomorrow.
  * Exampel ArrayDeque, you come up with a clever way to use two arrays to make your code faster.
  * Now instead of items,w e jhave items1 and items2.&#x20;
  * If items were private, I can delete items entirely and nobody’s code will break.

**4, Difference between static and non-static variables / functions.**

* Static members are invoked using the class name: Math.sqrt
  * <mark style="color:purple;">Cannot access instance variables.</mark>
  * <mark style="color:purple;">Everythin is known at compile time for static members, and it won't change during runtime, most likely.</mark>
* Intsance members are invoked using an instance name: m.maxDog
  * Asa  result we can access instance variabes.

Static是指成员为所有instance共有，非static为每个instance独有。<mark style="color:orange;">Static成员（method，或者是IntNode这样的Class成员）不能调用instance 成员。因为Static是所有instance共有的，如果调用了instance成员，那么instance是指的哪个instance呢？无法明确，因此无法使用。</mark>

1，construct不需要返回值，也不需要声明Void；

2，没有声明为static的function是class的每个对象独有的，所以每个对象都应该可以重写，或者是为了子类可以重写？

_<mark style="color:purple;">**Static**</mark> variables and functions belong to the whole class. Example: Every 61B Student shares the same professor, and if the professor were to change it would change for everyone._

_<mark style="color:purple;">**Instance**</mark> variables and functions belong to each individual instance. Example: Each 61B Student has their own ID number, and changing a student’s ID number doesn’t change anything for any other student._

