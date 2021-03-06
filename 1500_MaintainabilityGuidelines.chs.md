<!--
NOTE: Requires Markdown Extra. See http://michelf.ca/projects/php-markdown/extra/
 --> 

#Maintainability Guidelines
#可维护性代码指南

### Methods should not exceed 7 statements (AV1500) ![](images/1.png)
### 方法不应超过7条语句
A method that requires more than 7 statements is simply doing too much or has too many responsibilities. It also requires the human mind to analyze the exact statements to understand what the code is doing. Break it down into multiple small and focused methods with self-explaining names, but make sure the high-level algorithm is still clear.

### Make all members private and types internal by default (AV1501) ![](images/1.png)
### 默认所有成员都应是私有和类部类型
To make a more conscious decision on which members to make available to other classes, first restrict the scope as much as possible. Then carefully decide what to expose as a public member or type.

### Avoid conditions with double negatives (AV1502) ![](images/2.png)
### 避免双重否定的情况出现
Although a property like `customer.HasNoOrders` makes sense, avoid using it in a negative condition like this:

	bool hasOrders = !customer.HasNoOrders;

Double negatives are more difficult to grasp than simple expressions, and people tend to read over the double negative easily.

### Name assemblies after their contained namespace (AV1505) ![](images/3.png)
### 程序集命名，在他的命名空间中应包含程序集名称。
All DLLs should be named according to the pattern *Company*.*Component*.dll where *Company* refers to your company's name and *Component* contains one or more dot-separated clauses. For example `AvivaSolutions.Web.Controls.dll`.

As an example, consider a group of classes organized under the namespace `AvivaSolutions.Web.Binding` exposed by a certain assembly. According to this guideline, that assembly should be called `AvivaSolutions.Web.Binding.dll`. 

**Exception:** If you decide to combine classes from multiple unrelated namespaces into one assembly, consider suffixing the assembly name with `Core`, but do not use that suffix in the namespaces. For instance, `AvivaSolutions.Consulting.Core.dll`.

### Name a source file to the type it contains (AV1506) ![](images/3.png)
### 源文件名称应当包含其源代码的类型
Use Pascal casing to name the file and don't use underscores.

### Limit the contents of a source code file to one type (AV1507) ![](images/3.png)
### 源文件中的源代码应当只有一种类型
**Exception:** Nested types should, for obvious reasons, be part of the same file.

### Name a source file to the logical function of the partial type (AV1508) ![](images/3.png)
### 分部类型的源文件名称带有逻辑功能
When using partial types and allocating a part per file, name each file after the logical part that part plays. For example:

	// In MyClass.cs
	public partial class MyClass
	{...}
	
	// In MyClass.Designer.cs	
	public partial class MyClass
	{...}

### Use using statements instead of fully qualified type names (AV1510) ![](images/3.png)
### 使用using语句替换完全限定类型名
Limit usage of fully qualified type names to prevent name clashing. For example, don't do this:

	var list = new System.Collections.Generic.List();

Instead, do this:

	using System.Collections.Generic;
	
	var list = new List();

If you do need to prevent name clashing, use a `using` directive to assign an alias:

	using Label = System.Web.UI.WebControls.Label;

### Don't use "magic" numbers (AV1515) ![](images/1.png)
### 不使用“硬编码”
Don't use literal values, either numeric or strings, in your code, other than to define symbolic constants. For example:

	public class Whatever  
	{
		public static readonly Color PapayaWhip = new Color(0xFFEFD5);
		public const int MaxNumberOfWheels = 18;  
	}

Strings intended for logging or tracing are exempt from this rule. Literals are allowed when their meaning is clear from the context, and not subject to future changes, For example:

	mean = (a + b) / 2;// okay  
	WaitMilliseconds(waitTimeInSeconds \* 1000);// clear enough

If the value of one constant depends on the value of another, attempt to make this explicit in the code.

	public class SomeSpecialContainer  
	{  
		public const int MaxItems = 32;  
		public const int HighWaterMark = 3 \* MaxItems / 4;// at 75%  
	}

**Note:** An enumeration can often be used for certain types of symbolic constants.

### Only use var when the type is very obvious (AV1520) ![](images/1.png)
### 仅在类型非常明显的时候使用var
Only use `var` as the result of a LINQ query, or if the type is very obvious from the same statement and using it would improve readability. So don't

	var i = 3;									// what type? int? uint? float?
	var myfoo = MyFactoryMethod.Create("arg");	// Not obvious what base-class or			
												// interface to expect. Also difficult
												// to refactor if you can't search for
												// the class

Instead, use `var` like this:

	var q = from order in orders where order.Items > 10 and order.TotalValue > 1000;
	var repository = new RepositoryFactory.Get();	
	var list = new ReadOnlyCollection();

In all of three above examples it is clear what type to expect. For a more detailed rationale about the advantages and disadvantages of using `var`, read Eric Lippert's [Uses and misuses of implicit typing](http://blogs.msdn.com/b/ericlippert/archive/2011/04/20/uses-and-misuses-of-implicit-typing.aspx).

### Declare and initialize variables as late as possible (AV1521) ![](images/2.png)
### 声明和初始化变量仅可能的近
Avoid the C and Visual Basic styles where all variables have to be defined at the beginning of a block, but rather define and initialize each variable at the point where it is needed.

### Assign each variable in a separate statement (AV1522) ![](images/1.png)
### 每个变量赋值时，应当使用单个语句
Don't use confusing constructs like the one below:

	var result = someField = GetSomeMethod();

### Favor Object and Collection Initializers over separate statements (AV1523) ![](images/2.png)
### 对象或集合初始化使用单个语句更好
Instead of:

	var startInfo = new ProcessStartInfo("myapp.exe");	
	startInfo.StandardOutput = Console.Output;
	startInfo.UseShellExecute = true;

Use [Object Initializers](http://msdn.microsoft.com/en-us/library/bb384062.aspx):

	var startInfo = new ProcessStartInfo("myapp.exe")  
	{
		StandardOutput = Console.Output,
		UseShellExecute = true  
	};

Similarly, instead of:

	var countries = new List();
	countries.Add("Netherlands");
	countries.Add("United States");

Use collection or [dictionary initializers](http://msdn.microsoft.com/en-us/library/bb531208.aspx):

	var countries = new List { "Netherlands", "United States" };

### Don't make explicit comparisons to `true` or `false` (AV1525) ![](images/1.png)
### 不显示跟true或false进行比较运算

It is usually bad style to compare a `bool`-type expression to `true` or `false`. For example:

	while (condition == false)// wrong; bad style  
	while (condition != true)// also wrong  
	while (((condition == true) == true) == true)// where do you stop?  
	while (condition)// OK

### Don't change a loop variable inside a for or foreach loop (AV1530) ![](images/2.png)
### 在for或foreach循环中，不改变循环变量值
Updating the loop variable within the loop body is generally considered confusing, even more so if the loop variable is modified in more than one place. Although this rule also applies to `foreach` loops, an enumerator will typically detect changes to the collection the `foreach` loop is iteration over.

	for (int index = 0; index < 10; ++index)  
	{  
		if (_some condition_)
		{
			index = 11; // Wrong! Use 'break' or 'continue' instead.  
		}
	}

### Avoid nested loops (AV1532) ![](images/2.png)
### 避免嵌套循环
A method that nests loops is more difficult to understand than one with only a single loop. In fact, in most cases nested loops can be replaced with a much simpler LINQ query that uses the `from` keyword twice or more to *join* the data.

### Always add a block after keywords such as `if`, `else`, `while`, `for`, `foreach` and `case` (AV1535) ![](images/2.png)
### 总是`if`, `else`, `while`, `for`, `foreach` and `case`关键字之后增加大括符语句块
Please note that this also avoids possible confusion in statements of the form:

	if (b1) if (b2) Foo(); else Bar(); // which 'if' goes with the 'else'?
	
	// The right way:  
	if (b1)  
	{  
		if (b2)  
		{  
			Foo();  
		}  
		else  
		{  
			Bar();  
		}  
	}

### Always add a `default` block after the last `case` in a `switch` statement (AV1536) ![](images/1.png)
### 总是在switch语解码器最后的case之后增加default语句块
Add a descriptive comment if the `default` block is supposed to be empty. Moreover, if that block is not supposed to be reached throw an `InvalidOperationException` to detect future changes that may fall through the existing cases. This ensures better code, because all paths the code can travel have been thought about.

	void Foo(string answer)  
	{  
		switch (answer)  
		{  
			case "no":  
			{
			  Console.WriteLine("You answered with No");  
			  break;
			}  
			  
			case "yes":
			{  
			  Console.WriteLine("You answered with Yes");  
			  break;
			}
			
			default:  
			{
			  // Not supposed to end up here.  
			  throw new InvalidOperationException("Unexpected answer " + answer);
			}  
		}  
	}

### Finish every if-else-if statement with an else-part (AV1537) ![](images/2.png)
### 在每一个if-else-if语句最后部分带上else语句块
For example:

	void Foo(string answer)  
	{  
		if (answer == "no")  
		{  
			Console.WriteLine("You answered with No");  
		}  
		else if (answer == "yes")  
		{  
			Console.WriteLine("You answered with Yes");  
		}  
		else  
		{  
			// What should happen when this point is reached? Ignored? If not,   
			// throw an InvalidOperationException.  
		}  
	}

### Be reluctant with multiple return statements (AV1540) ![](images/2.png)
### 带多返回语句是不情愿的，但是符合仅早return原则，可以提高程序效率
One entry, one exit is a sound principle and keeps control flow readable. However, if the method is very small and complies with guideline AV1500 then multiple return statements may actually improve readability over some central boolean flag that is updated at various points.

### Don't use if-else statements instead of a simple (conditional) assignment (AV1545) ![](images/2.png)
### 不使用if-else语句替换简单的条件运算赋值语句
Express your intentions directly. For example, rather than:

	bool pos;
	
	if (val > 0)  
	{  
		pos = true;  
	}  
	else  
	{  
		pos = false;  
	}

write:

	bool pos = (val > 0);// initialization

Or instead of:

	string result;
	
	if (someString != null)
	{  
		result = someString;  
	}
	else
	{
		result = "Unavailable";
	}

	return result;

write:

	return someString ?? "Unavailable";

### Encapsulate complex expressions in a method or property (AV1547) ![](images/1.png)
### 用方法或属性封装复杂的表达式
Consider the following example:

	if (member.HidesBaseClassMember && (member.NodeType != NodeType.InstanceInitializer))
	{
		// do something
	}

In order to understand what this expression is about, you need to analyze its exact details and all of its possible outcomes. Obviously, you can add an explanatory comment on top of it, but it is much better to replace this complex expression with a clearly named method:

	if (NonConstructorMemberUsesNewKeyword(member))  
	{  
		// do something
	}  
  
  
	private bool NonConstructorMemberUsesNewKeyword(Member member)  
	{  
		return
			(member.HidesBaseClassMember &&
			(member.NodeType != NodeType.InstanceInitializer)  
	}

You still need to understand the expression if you are modifying it, but the calling code is now much easier to grasp.

### Call the more overloaded method from other overloads (AV1551) ![](images/2.png)
### 调用较多参数的重载方法
This guideline only applies to overloads that are intended to provide optional arguments. Consider, for example, the following code snippet:

	public class MyString  
	{
		private string someText;
		
	    	public int IndexOf(string phrase)  
		{  
			return IndexOf(phrase, 0); 
		}
		
		public int IndexOf(string phrase, int startIndex)  
		{  
			return IndexOf(phrase, startIndex, someText.Length - startIndex);
		}
		
		public virtual int IndexOf(string phrase, int startIndex, int count)  
		{  
			return someText.IndexOf(phrase, startIndex, count);
		}  
	}

The class `MyString` provides three overloads for the `IndexOf` method, but two of them simply call the one with one more parameter. Notice that the same rule applies to class constructors; implement the most complete overload and call that one from the other overloads using the `this()` operator. Also notice that the parameters with the same name should appear in the same position in all overloads.

**Important:** If you also want to allow derived classes to override these methods, define the most complete overload as a `protected virtual` method that is called by all overloads.

### Only use optional arguments to replace overloads (AV1553) ![](images/1.png)
### 仅使用可选参数去替换重载
The only valid reason for using C# 4.0's optional arguments is to replace the example from rule AV1551 with a single method like:

	public virtual int IndexOf(string phrase, int startIndex = 0, int count = 0)
	{
		return someText.IndexOf(phrase, startIndex, count);
	}

If the optional parameter is a reference type then it can only have a default value of `null`. But since strings, lists and collections should never be `null` according to rule AV1235, you must use overloaded methods instead.

**Note:** The default values of the optional parameters are stored at the caller side. As such, changing the default value without recompiling the calling code will not apply the new default value properly.

**Note:** When an interface method defines an optional parameter, its default value is not considered during overload resolution unless you call the concrete class through the interface reference. See [this post by Eric Lippert](http://blogs.msdn.com/b/ericlippert/archive/2011/05/09/optional-argument-corner-cases-part-one.aspx) for more details.

### Avoid using named arguments (AV1555) ![](images/1.png)
### 避免使用命名参数
C# 4.0's named arguments have been introduced to make it easier to call COM components that are known for offering many optional parameters. If you need named arguments to improve the readability of the call to a method, that method is probably doing too much and should be refactored.

**Exception:** The only exception where named arguments improve readability is when calling a method of some code base you don't control that has a `bool` parameter, like this:  

	object[] myAttributes = type.GetCustomAttributes(typeof(MyAttribute), inherit: false);

### Don't allow methods and constructors with more than three parameters (AV1561) ![](images/1.png)
### 不允许方法或构造函数超过3个参数
If you create a method with more than three parameters, use a structure or class to pass multiple arguments, as explained in the [Specification design pattern](http://en.wikipedia.org/wiki/Specification_pattern). In general, the fewer the parameters, the easier it is to understand the method. Additionally, unit testing a method with many parameters requires many scenarios to test.

### Don't use `ref` or `out` parameters (AV1562) ![](images/1.png)
### 不使用ref或out参数
They make code less understandable and might cause people to introduce bugs. Instead, return compound objects.

### Avoid methods that take a bool flag (AV1564) ![](images/2.png)
### 避免方法接受布尔值的参数
Consider the following method signature:

	public Customer CreateCustomer(bool platinumLevel) {}

On first sight this signature seems perfectly fine, but when calling this method you will lose this purpose completely:

	Customer customer = CreateCustomer(true);

Often, a method taking such a flag is doing more than one thing and needs to be refactored into two or more methods. An alternative solution is to replace the flag with an enumeration.

### Don't use parameters as temporary variables (AV1568) ![](images/3.png)
### 不使用参数作为临时参数
Never use a parameter as a convenient variable for storing temporary state. Even though the type of your temporary variable may be the same, the name usually does not reflect the purpose of the temporary variable.

### Always check the result of an `as` operation (AV1570) ![](images/1.png)
### 总是检查as操作的结果值（用以确保在非null值能正常操作，能确定类型的用is操作的不使用as操作）
If you use `as` to obtain a certain interface reference from an object, always ensure that this operation does not return `null`. Failure to do so may cause a `NullReferenceException` at a much later stage if the object did not implement that interface. If you are sure the object is of a certain type, use the `is` operator instead.

### Don't comment out code (AV1575) ![](images/1.png)
### 不注释代码
Never check in code that is commented out. Instead, use a work item tracking system to keep track of some work to be done. Nobody knows what to do when they encounter a block of commented-out code. Was it temporarily disabled for testing purposes? Was it copied as an example? Should I delete it?
