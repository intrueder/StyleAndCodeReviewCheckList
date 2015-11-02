##Naming Convetions and Style

1. Use Pascal casing for type and method names
```c#
public class SomeClass
{
	const int DefaultSize = 100;
	public void SomeMethod()
	{
	}
}
```

2. Use camel casing for local variable names and method arguments
```c#
void SomeMethod(int someNumber)
{
	int number;
}
```

3. Prefix interface names with I
```c#
interface IMyInterface
{
}
```

4. Avoid using Hungarian notation

5. Use `this` if you want to distinguish member variables with local

6. Suffix custom attribute classes with `Attribute`

7. Suffix custom exception classes with `Exception`

8. Name methods using verb-object-pair, such as `ShowDialog()`

9. Methods with return values should have a name describing the value returned, such as `GetObjectState()`

10. Use descriptive variable names
	a) Avoid single character variable names, such as i or t. Use `index` or `temp` instead
	b) Do not abbreviate words (such as `num` instead of `number`)
	
11. Always use C# predefined types rather than the aliases in the `System` namespace when declaring local variable, class field or method argument. But use types from `System` namespace when using static members
```c#
	// Avoid
	Object myObject;
	String myString;
	Int32 myNumber;
	string.Empty
	int.MaxValue
	string.IsNullOrEmpty()
	
	// Correct
	object myObject;
	string myString;
	int myNumber;
	String.Empty
	Int32.MaxValue
	String.IsNullOrEmpty()	
```

12. With generics, use capital letters for types. Reserve suffixing Type when dealing with the .Net type `Type` 
```c#
	// Avoid
	public class LinkedList<KeyType,DataType>
	{
	}
	
	// Correct
	public class LinkedList<K, T>
	{
	}
```

13. Use meaningful namespaces such as the product name or the company name

14. Avoid fully qualified type names. Use the `using` statement

15. Avoid putting a `using` statement inside a namespace

16. Group all framework namespaces together and put custom or third-party namespaces underneath
```c#
	// Avoid
	using MyCompany;
	using MyControls;
	using System;
	using System.Collections.Generic;
	using System.ComponentModel;
	using System.Data;
	
	// Correct
	using System;
	using System.Collections.Generic;
	using System.ComponentModel;
	using System.Data;
```

17. Use delegate inference instead of explicit delegate instantiation
```c#
	public delegate void ImageChangedDelegate();
	public void ChangeImage()
	{
	}
	
	// Avoid
	ImageChangedDelegate imageChanged = new ImageChangedDelegate(ChangeImage);
	
	// Correct
	ImageChangedDelegate imageChanged = ChangeImage;
```

18. Indent comments at the same level of indentation as the code you are documenting

19. All comments should pass spell checking. Misspelled comments indicate sloppy development.

20. All member variables should be declared at the top, with one line separating them from the properties or methods
```c#
public class MyClass
{
	int number;
	string name;
	
	public void SomeMethod1()
	{
	}
	
	public void SomeMethod2()
	{
	}
}
```

21. Declare a local variable as close as possible to its first use.

22. A file name should reflect the class it contains.

23. When using partial types and allocating a part per file, name each file after the logical part that part plays
```c#
// In MyClass.cs
public partial class MyClass
{
}

// In MyClass.Designer.cs
public partial class MyClass
{
}
```

24. Always place an open curly brace `{` in a new line.

25. With anonymous methods, mimic the code layout of a regular method, aligned with the delegate declaration. (complies with placing an open curly brace in a new line)
```c#
delegate void SomeDelegate(string someString);

// Avoid
void InvokeMethod()
{
	SomeDelegate someDelegate = delegate(string message) {MessageBox.Show(message);};
	someDelegate("Hello");
}

// Correct
void InvokeMethod()
{
	SomeDelegate someDelegate = delegate(string message)
								{
									MessageBox.Show(message);
								};
	someDelegate("Hello");
}
```

26. Use empty parantheses on parameter-less anonymous methods.Omit the parentheses only if the anonymous method could have been used on any delegate;
```c#
delegate void SomeDelegate();

// Avoid
SomeDelegate someDelegate = delegate 
							{
								MessageBox.Show("Hello");
							};

// Correct
SomeDelegate someDelegate = delegate()
							{
								MessageBox.Show("Hello");
							}
```

27. With Lambda expressions, mimic the code layout of a regular method, aligned with the delegate declaration. Omit the variable type and rely on type inference, yet use parentheses:
```c#
delegate void SomeDelegate(string someString);

// Avoid
SomeDelegate someDelegate = (string message) =>
							{
								Trace.WriteLine(message);
								MessageBox.Show(message);
							};
// Correct
SomeDelegate someDelegate = (message) =>
							{
								Trace.WriteLine(message);
								MessageBox.Show(message);
							};
```

28. Only use in-line Lambda expressions when they contain a single simple statement. Avoid multiple statements that require a curly brace or a `return` statement with in-line expressions. Omit parentheses:
```c#
delegate void SomeDelegate(string someString);

void MyMethod(SomeDelegate someDelegate)
{
}

// Avoid
MyMethod((message) => {Trace.WriteLine(message);MessageBox.Show(message);});

// Correct
MyMethod(message => MessageBox.Show(message));
```

## Coding Practices

1. Avoid putting multiple classes in a single file

2. A single file should contribute types to only a single namespace. Avoid having multiple namespaces in the same file

3. Avoid files with more than 500 lines (excluding machine-generated code)

4. Avoid methods with more than 50 lines

5. Avoid methods with more than 5 arguments

6. Lines should not exceed 120 characters

7. Do not manually edit any machine-generated code
	a) If modifying machine generated code, modify the format and style to match this coding standard
	
8. Avoid comments that explaing the obvious. Code should be self-explanatory. Good code with readable variable and method names should not require comments

9. Document only operational assumptions, alogrithm insights and so on

10. Avoid method-level documentation. Use method-level comments only as a tool tips for other developers.

11. In general, prefer overloading to default parameters:
```c#
// Avoid
class MyClass
{
	void SomeMethod(int number = 123)
	{
	}
}

// Correct
class MyClass
{
	void SomeMethod()
	{
		SomeMethod(123);
	}
	
	void SomeMethod(int number)
	{
	}
}
```

12. When using default parameters, restrict them to natural immutable constants such as null, false, or 0:
```c#
void SomeMethod(int number = 0)
{
}
	
void SomeMethod(string message = null)
{
}
	
void SomeMethod(bool flag = false)
{
}
```

13. Assert every assumption. On average, every 6th line is an assertion
```c#
using System.Diagnostics;

object GetObject()
{
}

object someObject = GetObject();
Debug.Assert(someObject != null);
```

14. Every line of code should be walked through in a "white box" testing manner

15. Catch only exceptions for which you have explicit handling

16. In a `catch` statement that throws exceptions, always throw the original exception (or another exception constructed from the original exception) to maintain the stack location of the original error
```c#
catch(Exception ex)
{
	MessageBox.Show(ex.Message);
	throw;
}
```

17. Avoid error codes as method return values

18. Avoid defining custom exception classes

19. When defining custom exception:
	a) Derive the custom exception from `Exception`
	b) Provide custom serialization

20. Avoid multiple `Main()` methods in a single assembly

21. Make only the most necessary types public, mark others as `internal`

22. Avoid code that relies on an assembly running from a particular location

23. Minimize code in application assemblies(EXE client assemblies). Use class libraries instead to contain business logical

24. Avoid providing explicit values for enums unless they are integer powers of 2:
```c#
// Avoid
public enum Color
{
	Red = 1,
	Green = 2,
	Blue = 3
}

// Correct
public enum Color
{
	Red,
	Green,
	Blue
}
```

25. Avoid specifying a type for an enum
```c#
// Avoid
public enum Color : long
{
	Red,
	Green,
	Blue
}
```

26. Always use a curly brace scope in an `if` statement, even if it conditions a single statement

27. Avoid using the ternary conditional operator

28. Always use zero-based arrays

29. With indexed collection, use zero-based indexes

30. Always explicitly initialize an array of reference types using a `for` loop
```c#
public class MyClass
{
}

const int ArraySize = 100;

MyClass[] someArray = new MyClass[ArraySize];

for(int index = 0; index < someArray.Length; index++)
{
	array[index] = new MyClass();
}
```

31. Do not provide public or protected member variables. Use properties instead

32. Avoid explicit properties that do nothing except access a member variable. Use automatic properties instead:
```c#
// Avoid
class MyClass
{
	int number;
	public int Number
	{
		get
		{
			return number;
		}
		set
		{
			number = value;
		}
	}
}

// Correct
class MyClass
{
	public int Number { get; set; }
}
```

33. Avoid using the `new` inheritance qualifier. Use `override` instead

34. When override `Equals`, override `GetHashCode` too

35. Never use unsafe code, except when using interop

36. Avoid explicit casting. Use the `as` operator to defensively cast to a type
```c#
	// Avoid
	Dog dog = new GermanShepherd();
	GermanShepherd shepherd = (GermanShepherd)dog;
	
	// Correct
	Dog dog = new GermanShepherd();
	GermanShepherd shepherd = dog as GermanShepherd;
	if (shepherd != null)
	{
	}
```

37. Always check a delegate for `null` before invoking it

38. Never hardcode string that will be presented to end users. Use resources instead

39. Never hardcode strings that might change based on deployment such as connection strings

40. Use `String.Empty` instead of ""
```c#
// Avoid
string myString = "";

// Correct
string myString = String.Empty;
```

41. Never use `goto`

42. Always have a `default` case in a `switch` statement

43. Always run code unchecked by default (for the sake of performance), but explicitly in checked mode for overflow- or underflow-prone operations
```c#
int CalcPower(int number, int power)
{
	result = 1;
	for(int count = 1; count <= power; count++)
	{
		checked
		{
			result *= number;
		}
	}
	
	return result;
}
```
