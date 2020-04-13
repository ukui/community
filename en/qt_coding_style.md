# Qt Coding Style
## Indentation
* 4 spaces are used for indentation
* Spaces, not tabs!

## Declaring variables
* Declare each variable on a separate line
* Avoid short or meaningless names
* Single character variable names are only okay for counters and temporaries
* Wait when declaring a variable until it is needed
``` cpp
// Wrong
int a, b;
char *c, *d;

// Correct
int height;
int width;
char *nameOfThis;
char *nameOfThat;
```

* Varibales and functions start with a lower-case letter. Each consecutive word in a variable's name starts with an upper-case letter
* Avoid abbreviations
```cpp
// Wrong
short Cntr;
char ITEM_DELIM = ' ';

// Correct
short counter;
char itemDelimiter = ' ';
```

* Classes always start with an upper-case letter. Public classes start with a 'Q' (QRgb) followed by an upper case letter. Public functions most often start with a 'q' (qRgb).
* Acronyms are camel-cased (e.g. QUkuiWindowManager, not QUKUIWindowManager)

## Whitespace
* Use blank lines to group statements together where suited
* Always use only on blank line
* Always use a single space after a keywork and before a curly brace:
```cpp
// Wrong
if(foo){
}

// Correct
if (foo) {
}
```

* For pointers or references, always use a single space  between the type and '*' or '&', but no space between the '*' or '&' and the variable name:
```cpp
char *x;
const QString &myString;
const char * const y = "hello";
```

* Surround binary operators with spaces
* Leave a space after each comma
* No space after a cast
* Avoid C-style casts when possible
```cpp
// Wrong
char* blockOfMemory = (char* ) malloc(data.size());

// Correct
char *blockOfMemory = reinterpret_cast<char *>(malloc(data.size()));
```
* Do not put multiple statements on one line
* By extension, use a new line for the body of a control flow statement:
```cpp
// Wrong
if (foo) bar();

// Correct
if (foo)
    bar();
```

## Braces
* Use attached braces: The opening brace goes on the same line as the start of the statement. If the closing brace is followed by another keyword, it goes into the same line as well:
```cpp
 // Wrong
 if (codec)
 {
 }
 else
 {
 }

 // Correct
 if (codec) {
 } else {
 }
```

* Exception: Function implementations (but not lambdas) and class declarations always have the left brace on the start of a line:
```cpp
 static void foo(int g)
 {
     qDebug("foo: %i", g);
 }

 class Moo
 {
 };
```

* Use curly braces only when the body of a conditional statement contains more than one line:
```cpp
 // Wrong
 if (address.isEmpty()) {
     return false;
 }

 for (int i = 0; i < 10; ++i) {
     qDebug("%i", i);
 }

 // Correct
 if (address.isEmpty())
     return false;

 for (int i = 0; i < 10; ++i)
     qDebug("%i", i);
```

* Exception 1: Use braces also if the parent statement covers several lines / wraps:
```cpp
 // Correct
 if (address.isEmpty() || !isValid()
     || !codec) {
     return false;
 }
```

* Exception 2: Brace symmetry: Use braces also in if-then-else blocks where either the if-code or the else-code covers several lines:
```cpp
 // Wrong
 if (address.isEmpty())
     qDebug("empty!");
 else {
     qDebug("%s", qPrintable(address));
     it;
 }

 // Correct
 if (address.isEmpty()) {
     qDebug("empty!");
 } else {
     qDebug("%s", qPrintable(address));
     it;
 }

 // Wrong
 if (a)
     …
 else
     if (b)
         …

 // Correct
 if (a) {
     …
 } else {
     if (b)
         …
 }
```

* Use curly braces when the body of a conditional statement is empty
```cpp
 // Wrong
 while (a);

 // Correct
 while (a) {}
```

## Parentheses
* Use parentheses to group expressions:
```cpp
 // Wrong
 if (a && b || c)

 // Correct
 if ((a && b) || c)

 // Wrong
 a + b & c

 // Correct
 (a + b) & c
```

## Switch statements
* The case labels are in the same column as the switch
* Every case must have a break (or return) statement at the end or use Q_FALLTHROUGH() to indicate that there's intentionally no break, unless another case follows immediately.
```cpp
 switch (myEnum) {
 case Value1:
   doSomething();
   break;
 case Value2:
 case Value3:
   doSomethingElse();
   Q_FALLTHROUGH();
 default:
   defaultHandling();
   break;
 }
```

## Jump statements (break, continue, return, and goto)
* Do not put 'else' after jump statements:
```cpp
 // Wrong
 if (thisOrThat)
     return;
 else
     somethingElse();

 // Correct
 if (thisOrThat)
     return;
 somethingElse();
```

* Exception: If the code is inherently symmetrical, use of 'else' is allowed to visualize that symmetry

## Line breaks
* Keep lines shorter than 100 characters; wrap if necessary
  * Comment/apidoc lines should be kept below 80 columns of actual text. Adjust to the surroundings, and try to flow the text in a way that avoids "jagged" paragraphs.
* Commas go at the end of wrapped lines; operators start at the beginning of the new lines. An operator at the end of the line is easy to miss if the editor is too narrow.
```cpp
 // Wrong
 if (longExpression +
     otherLongExpression +
     otherOtherLongExpression) {
 }

 // Correct
 if (longExpression
     + otherLongExpression
     + otherOtherLongExpression) {
 }
```

## General exceptions
* When strictly following a rule makes your code look bad, feel free to break it.
* If there is a dispute in any given module, the Maintainer has the final say on the accepted style (as per The Qt Governance Model).

## Artistic Style
The following snippet can be used by artistic style for reformatting your code.
```
--style=kr
--indent=spaces=4
--align-pointer=name
--align-reference=name
--convert-tabs
--attach-namespaces
--max-code-length=100
--max-instatement-indent=120
--pad-header
--pad-oper
```
Note that "unlimited" --max-instatement-indent is used only because astyle is not smart enough to wrap the first argument if subsequent lines would need indentation limitation. You are encouraged to manually limit in-statement-indent to roughly 50 colums:
```cpp
    int foo = some_really_long_function_name(and_another_one_to_drive_the_point_home(
            first_argument, second_argument, third_arugment));
```

## C++ features
* Don't use exceptions
* Don't use rtti (Run-Time Type Information; that is, the typeinfo struct, the dynamic_cast or the typeid operators, including throwing exceptions)
* Use templates wisely, not just because you can.
Hint: Use the compile autotest to see whether a C++ feature is supported by all compilers in the test farm.

## Conventions in Qt source code
* All code is ascii only (7-bit characters only, run man ascii if unsure)
  * Rationale: We have too many locales inhouse and an unhealthy mix of UTF-8 and latin1 systems. Usually, characters > 127 can be broken without you even knowing by clicking SAVE in your favourite editor.
  * For strings: Use \nnn (where nnn is the octal representation of whatever character encoding you want your string in) or \xnn (where nn is hexadecimal). Example: QString s = QString::fromUtf8("13\005");
  * For umlauts in documentation, or other non-ASCII characters, either use qdoc's command or use the relevant macro; e.g. \uuml for ü.
* Every QObject subclass must have a Q_OBJECT macro, even if it doesn't have signals or slots, otherwise qobject_cast will fail.
* Normalize the arguments for signals + slots (see QMetaObject::normalizedSignature) inside connect statements to get faster signal/slot lookups. You can use qtrepotools/util/normalize to normalize existing code.

### Including headers
* In public header files, always use this form to include Qt headers: #include <QtCore/qwhatever.h>. The library prefix is necessary for Mac OS X frameworks and is very convenient for non-qmake projects.
* In source files, include specialized headers first, then generic headers. Separate the categories with empty lines.
```cpp
#include <qstring.h> // Qt class
#include <new> // STL stuff
#include <limits.h> // system stuff
```
* If you need to include qplatformdefs.h, always include it as the first header file.
* If you need to include private headers, be careful. Use the following syntax, irrespective of which module or directory whatever_p.h is in.
```cpp
#include <private/whatever_p.h>
```

### Casting
* Avoid C casts, prefer C++ casts (static_cast, const_cast, reinterpret_cast)
  * Rationale: Both reinterpret_cast and C-style casts are dangerous, but at least reinterpret_cast won't remove the const modifier
* Don't use dynamic_cast, use qobject_cast for QObjects or refactor your design, for example by introducing a type() method (see QListWidgetItem)
* Use the constructor to cast simple types: int(myFloat) instead of (int)myFloat
  * Rationale: When refactoring code, the compiler will instantly let you know if the cast would become dangerous.

### Compiler/Platform specific issues
* Be extremely careful when using the questionmark operator. If the returned types aren't identical, some compilers generate code that crashes at runtime (you won't even get a compiler warning).
```cpp
QString s;
return condition ? s : "nothing"; // crash at runtime - QString vs. const char *
```
* Be extremely careful about alignment.
  * Whenever a pointer is cast such that the required alignment of the target is increased, the resulting code might crash at runtime on some architectures. For example, if a const char * is cast to an const int *, it'll crash on machines where integers have to be aligned at two- or four-byte boundaries.
  * Use a union to force the compiler to align variables correctly. In the example below, you can be sure that all instances of AlignHelper are aligned at integer-boundaries.
```cpp
union AlignHelper {
    char c;
    int i;
};
```
* Anything that has a constructor or needs to run code to be initialized cannot be used as global object in library code, since it is undefined when that constructor/code will be run (on first usage, on library load, before main() or not at all). Even if the execution time of the initializer is defined for shared libraries, you'll get into trouble when moving that code in a plugin or if the library is compiled statically:
```cpp
// global scope
static const QString x; // Wrong - default constructor needs to be run to initialize x
static const QString y = "Hello"; // Wrong - constructor that takes a const char * has to be run
QString z; // super wrong
static const int i = foo(); // wrong - call time of foo() undefined, might not be called at all
```
Things you can do:
```cpp
// global scope
static const char x[] = "someText"; // Works - no constructor must be run, x set at compile time
static int y = 7; // Works - y will be set at compile time
static MyStruct s = {1, 2, 3}; // Works - will be initialized statically, no code being run
static QString *ptr = 0; // Pointers to objects are ok - no code needed to be run to initialize ptr
```
Use Q_GLOBAL_STATIC to create static global objects instead:
```cpp
Q_GLOBAL_STATIC(QString, s)

void foo()
{
    s()->append("moo");
}
```
Note: Static objects in a scope are no problem, the constructor will be run the first time the scope is entered. Such code is reentrant since C++11.

* A char is signed or unsigned dependent on the architecture. Use signed char or unsigned char if you explicitely want a signed/unsigned char. The condition in the following code is always true on platforms where the default char is unsigned.
```cpp
char c; // c can't be negative if it is unsigned
/********/
/*******/
if (c > 0) { … } // WRONG - condition is always true on platforms where the default is unsigned
```
* Avoid 64-bit enum values.
  * The aapcs embedded ABI hard codes all enum values to a 32-bit integer.
  * Microsoft compilers don't support 64-bit enum values. (confirmed with Microsoft ® C/C++ Optimizing Compiler Version 15.00.30729.01 for x64)

### Aesthetics
* Prefer enums to define constants over static const int or defines.
  * enum values will be replaced by the compiler at compile time, resulting in faster code
  * defines are not namespace safe (and look ugly)
* Prefer verbose argument names in headers.
  * Most IDEs will show the argument names in their completion-box.
  * It will look better in the documentation
  * Bad style: doSomething(QRegion rgn, QPoint p) - use doSomething(QRegion clientRegion, QPoint gravitySource) instead
* When reimplementing a virtual method, do not put the `virtual` keyword in the header file. On Qt5, annotate them with the override keyword after the function declaration, just before the ';' (or the '{' ).

### Things to avoid
* Do not inherit from template/tool classes
  * The destructors are not virtual, leading to potential memory leaks
  * The symbols are not exported (and mostly inline), leading to interesting symbol clashes.
  * Example: Library A has
```cpp
class Q_EXPORT X: public QList<QVariant> {};
```
and library B has
```cpp
class Q_EXPORT Y: public QList<QVariant> {};
```
Suddenly, QList<QVariant>'s symbols are exported from two libraries - /clash/.
* Don't mix const and non-const iterators. This will silently crash on broken compilers.
```cpp
for (Container::const_iterator it = c.begin(); it != c.end(); ++it) // W R O N G
for (Container::const_iterator it = c.cbegin(); it != c.cend(); ++it) // Right
```
* Q[Core]Application is a singleton class. There can only be one instance at a time. However, that instance can be destroyed and a new one can be created, which is likely in an ActiveQt or browser plugin. Code like this will easily break:
```cpp
static QObject *obj = 0;
if (!obj)
    obj = new QObject(QCoreApplication::instance());
```
If the QCoreApplication application is destroyed, obj will be a dangling pointer. Use Q_GLOBAL_STATIC for static global objects or qAddPostRoutine to clean up.
* Avoid the use of anonymous namespaces in favor of the static keyword if possible. A name localized to the compilation unit with static is guaranteed to have internal linkage. For names declared in anonymous namespaces the C++ standard unfortunately mandates external linkage. (7.1.1/6, or see various discussions about this on the gcc mailing lists)

### Binary and Source Compatibility
* Definitions:
  * Qt 4.0.0 is a major release, Qt 4.1.0 is a minor release, Qt 4.1.1 is a patch release
  * Backward binary compatibility: Code linked to an earlier version of the library keeps working
  * Forward binary compatibility: Code linked to a newer version of the library works with an older library
  * Source code compatibility: Code compiles without modification
* Keep backward binary compatibility + backward source code compatibility in minor releases
* Keep backward and forward binary compatibility + forward and backward source code compatibility in patch releases
  * Don't add/remove any public API (e.g. global functions, public/protected/private methods)
  * Don't reimplement methods (not even inlines, nor protected/private methods)
  * Check Binary Compatibility Workarounds for ways to keep b/c
* Info on binary compatibility: https://community.kde.org/Policies/Binary_Compatibility_Issues_With_C++
* When writing a QWidget subclass, always reimplement event(), even if it's empty. This makes sure that the widget can be fixed without breaking binary compatibility.
* All exported functions from Qt must start with either 'q' or 'Q'. Use the "symbols" autotest to find violations.

### Namespacing
Read Qt In Namespace and keep in mind that all of Qt except Tests and Webkit is "namespaced" code.

### Operators
["The decision between member and non-member"](http://stackoverflow.com/questions/4421706/operator-overloading/4421729#4421729)

A binary operator that treats both of its arguments equally should not be a member. Because, in addition to the reasons mentioned in the stack overflow answer, the arguments are not equal when the operator is a member.

Example with QLineF which unfortunately has its operator== as a member:
```cpp
QLineF lineF;
QLine lineN;

if (lineF == lineN) // Ok,  lineN is implicitly converted to QLineF
if (lineN == lineF) // Error: QLineF cannot be converted implicitly to QLine, and the LHS is a member so no conversion applies
```
If the operator== was outside of the class, conversion rules would apply equally for both sides.

## Conventions for public header files
Our public header files have to survive the strict settings of some of our users. All installed headers have to follow these rules:

* No C style casts (-Wold-style-cast)
  * Use static_cast, const_cast or reinterpret_cast
  * for basic types, use the constructor form: int(a) instead of (int)a
  * See chapter "Casting" for more info
* No float comparisons (-Wfloat-equal)
  * Use qFuzzyCompare to compare values with a delta
  * Use qIsNull to check whether a float is binary 0, instead of comparing it to 0.0.
* Don't hide virtual methods in subclasses (-Woverloaded-virtual)
  * If the baseclass A has a virtual int val() and subclass B an overload with the same name, int val(int x), A's val function is hidden. Use the using keyword to make it visible again:
```cpp
class B: public A
{
    using A::val;
    int val(int x);
};
```
* Don't shadow variables (-Wshadow)
  * avoid things like this->x = x;
  * don't give variables the same name as functions declared in your class
* Always check whether a preprocessor variable is defined before probing its value (-Wundef)
```cpp
#if Foo == 0  // W R O N G
#if defined(Foo) && (Foo == 0) // Right
#if Foo - 0 == 0 // Clever, are we? Use the one above instead, for better readability
```

## Conventions for C++11 usage
Note: This section is not an accepted convention yet. This section serves as baseline for further discussions.

### Lambdas
You can use lambdas with the following restrictions:

* If you use static functions from the class that the lambda is located in, refactor the code so you don't use a lambda. For example, instead of
```cpp
void Foo::something()
{
    ...
    std::generate(begin, end, []() { return Foo::someStaticFunction(); });
    ...
}
```
You may be able to simply pass the function pointer:
```cpp
void Foo::something()
{
    ...
    std::generate(begin, end, &Foo::someStaticFunction);
    ...
}
```
The reason for this is that GCC 4.7 and earlier had a bug that required capturing this but Clang 5.0 and later will produce a warning if you do:
```cpp
void Foo::something()
{
    ...
    std::generate(begin, end, [this]() { return Foo::someStaticFunction(); });
    // warning: lambda capture 'this' is not used [-Wunused-lambda-capture]
    ...
}
```
Format the lambda according to the following rules:

* Always write parentheses for the parameter list, even if the function does not take parameters.
```cpp
[]() { doSomething(); }
```
-NOT
```cpp
[] { doSomething(); }
```
* Place the capture-list, parameter list, return type, and opening brace on the first line, the body indented on the following lines, and the closing brace on a new line.
```cpp
[]() -> bool {
    something();
    return isSomethingElse();
}
```
-NOT-
```cpp
[]() -> bool { something();
    somethingElse(); }
```
* Place a closing parenthesis and semicolon of an enclosing function call on the same line as the closing brace of the lambda.
```cpp
foo([]() {
    something();
});
```
* If you are using a lambda in an 'if' statement, start the lambda on a new line, to avoid confusion between the opening brace for the lambda and the opening brace for the 'if' statement.
```cpp
if (anyOf(fooList,
          [](Foo foo) {
              return foo.isGreat();
          })) {
    return;
}
```
-NOT-
```cpp
if (anyOf(fooList, [](Foo foo) {
          return foo.isGreat();
       })) {
    return;
}
```
Optionally, place the lambda completely on one line if it fits.
```cpp
foo([]() { return true; });

if (foo([]() { return true; })) {
    ...
}
```

### auto Keyword
Optionally, you can use the auto keyword in the following cases. If in doubt, for example if using auto could make the code less readable, do not use auto. Keep in mind that code is read much more often than written.

* When it avoids repetition of a type in the same statement.
```cpp
auto something = new MyCustomType;
auto keyEvent = static_cast<QKeyEvent *>(event);
auto myList = QStringList() << QLatin1String("FooThing") << QLatin1String("BarThing");
```
* When assigning iterator types.
```cpp
auto it = myList.const_iterator();
```
