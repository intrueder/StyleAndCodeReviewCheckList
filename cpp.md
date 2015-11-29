(see http://google.github.io/styleguide/cppguide.html for details)

## Header Files

1. The #define Guard

For header guards prefer definitions over `pragma once`
The format of the symbol name should be `<PROJECT>_<PATH>_<FILE>_H_`.
For example, the file `foo/src/bar/baz.h` in project foo should have the following guard:

   ```c
#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_

...

#endif  // FOO_BAR_BAZ_H_
   ```

2. Forward Declarations

Avoid using forward declarations where possible. Just #include the headers you need.
( A "forward declaration" is a declaration of a class, function, or template without an associated definition. )

## Scoping

1. Local Variables

Place a function's variables in the narrowest scope possible, and initialize variables in the declaration.

2. Static and Global Variables

Variables of class type with static storage duration are forbidden: they cause hard-to-find bugs due to indeterminate order of construction and destruction. However, such variables are allowed if they are constexpr: they have no dynamic initialization or destruction.

Objects with static storage duration, including global variables, static variables, static class member variables, and function static variables, must be Plain Old Data (POD): only ints, chars, floats, or pointers, or arrays/structs of POD.

If you need a static or global variable of a class type, consider initializing a pointer (which will never be freed), from your main() function. Note that this must be a raw pointer, not a "smart" pointer, since the smart pointer's destructor will have the order-of-destructor issue that we are trying to avoid.

## Classes

1. Doing Work in Constructors
Avoid virtual method calls in constructors, and avoid initialization that can fail.

2. Structs vs. Classes
Use a struct only for passive objects that carry data; everything else is a class.

3. Inheritance
Composition is often more appropriate than inheritance. When using inheritance, make it `public`.

4. Multiple Inheritance
Only very rarely is multiple implementation inheritance actually useful. Multiple inheritance is allowed only when at most one of the base classes has an implementation; all other base classes must be pure interface classes tagged with the `Interface` suffix.

5. Interfaces
Classes that satisfy certain conditions are allowed, but not required, to end with an `Interface` suffix.

A class is a pure interface if it meets the following requirements:

    - It has only public pure virtual ("`= 0`") methods and static methods (but see below for destructor).
    - It may not have non-static data members.
    - It need not have any constructors defined. If a constructor is provided, it must take no arguments and it must be protected.
    - If it is a subclass, it may only be derived from classes that satisfy these conditions and are tagged with the `Interface` suffix.

An interface class can never be directly instantiated because of the pure virtual method(s) it declares. To make sure all implementations of the interface can be destroyed correctly, the interface must also declare a virtual destructor (in an exception to the first rule, this should not be pure). 

6. Access Control
Make data members private, unless they are static const (and follow the naming convention for constants).

7. Declaration Order
Use the specified order of declarations within a class: `public:` before `private:`, methods before data members (variables), etc.

Your class definition should start with its `public:` section, followed by its `protected:` section and then its `private:` section. If any of these sections are empty, omit them.

Within each section, the declarations generally should be in the following order:

    - Typedefs and Enums
    - Constants (`static const` data members)
    - Constructors
    - Destructor
    - Methods, including static methods
    - Data Members (except `static const` data members)

## Functions

1. Parameter Ordering
When defining a function, parameter order is: inputs, then outputs.

2. Write Short Functions
Prefer small and focused functions.

We recognize that long functions are sometimes appropriate, so no hard limit is placed on functions length. If a function exceeds about 40 lines, think about whether it can be broken up without harming the structure of the program.

3.  Reference Arguments
All parameters passed by reference must be labeled `const`.

In C, if a function needs to modify a variable, the parameter must use a pointer, eg `int foo(int *pval)`. In C++, the function can alternatively declare a reference parameter: `int foo(int &val)`.

## Other C++ Features

1. Exceptions
Do not use C++ exceptions.

2. Run-Time Type Information (RTTI)
Avoid using Run Time Type Information (RTTI).
RTTI allows a programmer to query the C++ class of an object at run time. This is done by use of `typeid` or `dynamic_cast`.

3. Casting
Use C++ casts like `static_cast<>()`. Do not use other cast formats like `int y = (int)x;` or `int y = int(x);`.

4. Preincrement and Predecrement
Use prefix form (`++i`) of the increment and decrement operators with iterators and other template objects.

5. Use of const
Use `const` whenever it makes sense.

6. Integer Types
Of the built-in C++ integer types, the only one used is `int`. If a program needs a variable of a different size, use a precise-width integer type from `<stdint.h>`, such as `int16_t`. If your variable represents a value that could ever be greater than or equal to 2^31 (2GiB), use a 64-bit type such as `int64_t`. Keep in mind that even if your value won't ever be too large for an `int`, it may be used in intermediate calculations which may require a larger type. When in doubt, choose a larger type.
Don't use an unsigned type.

7. 0 and nullptr/NULL
Use 0 for integers, 0.0 for reals, nullptr (or NULL) for pointers, and '\0' for chars.

8. sizeof
Prefer sizeof(varname) to sizeof(type).
```c
Struct data;
memset(&data, 0, sizeof(data));
```

## Naming
1. Type, Constant and Function Names
All these names start with a capital letter and have a capital letter for each new word, with no underscores: `MyExcitingClass`, `MyExcitingEnum`, `DaysInAWeek`, `DeleteUrl()`.

2. Variable Names
The names of variables and data members start with a lowercase letter and have a capital letter for each new word.

There are no special requirements for global variables, which should be rare in any case, but if you use one, consider prefixing it with `g_` or some other marker to easily distinguish it from local variables.

3. Macro Names
You're not really going to define a macro, are you? If you do, they're like this: `MY_MACRO_THAT_SCARES_SMALL_CHILDREN`.

## Formatting
1. Spaces vs. Tabs
Use only spaces, and indent 2 spaces at a time.
Do not use tabs in your code. You should set your editor to emit spaces when you hit the tab key.

2. Function Declarations and Definitions
Return type on the same line as function name, parameters on the same line if they fit. Wrap parameter lists which do not fit on a single line as you would wrap arguments in a function call.

Functions look like this:
```c
ReturnType ClassName::FunctionName(Type par_name1, Type par_name2) {
  DoSomething();
  ...
}
```
If you have too much text to fit on one line:
```c
ReturnType ClassName::ReallyLongFunctionName(Type par_name1, Type par_name2,
                                             Type par_name3) {
  DoSomething();
  ...
}
```
or if you cannot fit even the first parameter:
```c
ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
    Type par_name1,  // 4 space indent
    Type par_name2,
    Type par_name3) {
  DoSomething();  // 2 space indent
  ...
}
```

3. Conditionals
Prefer no spaces inside parentheses. The `if` and `else` keywords belong on separate lines.
```c
if (condition) {  // no spaces inside parentheses
  ...  // 2 space indent.
} else if (...) {  // The else goes on the same line as the closing brace.
  ...
} else {
  ...
}

if(condition) {   // Bad - space missing after IF.
if (condition){   // Bad - space missing before {.
if(condition){    // Doubly bad.
```

Curly braces are always required, even for single-line statements.

4. Loops and Switch Statements
Switch statements may use braces for blocks. Annotate non-trivial fall-through between cases. Braces are required for single-statement loops. Empty loop bodies should use `{}`, not a single semicolon.

`case` blocks in `switch` statements can have curly braces or not, depending on your preference. If you do include curly braces they should be placed as shown below.

If not conditional on an enumerated value, switch statements should always have a `default` case (in the case of an enumerated value, the compiler will warn you if any values are not handled). If the default case should never execute, simply `assert`:

```c
switch (var) {
  case 0: {  // 2 space indent
    ...      // 4 space indent
    break;
  }
  case 1: {
    ...
    break;
  }
  default: {
    assert(false);
  }
}
```

```c
for (int i = 0; i < someNumber; ++i) {
  printf("for loop\n");
}
```

5. Pointer and Reference Expressions
No spaces around period or arrow. Pointer operators do not have trailing spaces.

The following are examples of correctly-formatted pointer and reference expressions:
```c
x = *p;
p = &x;
x = r.y;
x = r->y;
```
Note that:

    - There are no spaces around the period or arrow when accessing a member.
    - Pointer operators have no space after the `*` or `&`.

When declaring a pointer variable or argument, you may place the asterisk adjacent to either the type or to the variable name:

```c
// These are fine, space preceding.
char *c;
const string &str;

// These are fine, space following.
char* c;    // but remember to do "char* c, *d, *e, ...;"!
const string& str;

char * c;  // Bad - spaces on both sides of *
const string & str;  // Bad - spaces on both sides of &
```

You should do this consistently within a single file, so, when modifying an existing file, use the style in that file.

6. Boolean Expressions
When you have a boolean expression that is too long, be consistent in how you break up the lines.

In this example, the logical AND operator is always at the end of the lines:
```c
if (thisOneThing > thisOtherThing &&
    aThirdThing == aFourthThing &&
    yetAnother && lastOne) {
  ...
}
```

7. Return Values
Do not needlessly surround the return expression with parentheses.
```c
return result;                  // No parentheses in the simple case.
// Parentheses OK to make a complex expression more readable.
return (some_long_condition &&
        another_condition);
```

## WinAPI
1. Use `HANDLE_MSG` macro instead of huge `switch..case` block

2. Use `ZeroMemory` instead of `memset` with `0` as fill pattern
   ```c
   // bad
   memset(buffer, 0, sizeof(buffer));
   
   // good
   ZeroMemory(buffer, sizeof(buffer));
   ```

3. Use helper macro from `WindowsX.h` instead of `SendMessage`
   ```c
   // bad
   SendMessage(hCtrl,LB_RESETCONTENT,0,0L);
   
   // good
   ListBox_ResetContent(hCtrl);
   ```

4. Use definitions 
   ```c
   // bad
   ShowWindow(hDlg, 1);
   
   // good
   ShowWindow(hDlg, SW_SHOW);

