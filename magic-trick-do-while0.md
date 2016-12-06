# Magic Trick: do {...} while(0)

When doing macro expansion, we want to use it just like a function call.
For example,

**Right Example**

```cpp
//define a helper macro here
#define foo(x,y)		\
	do{			    	\
		func1(x,y);		\
		func2(x,y);		\
	}while(0)

//use it

if (condition)
	foo(x,y); //just like a function call
else
	bar(x,y);
```

This section will be expanded as:

```cpp
if (condition)
	do{	func1(x,y); func2(x,y);}while(0);
else
	bar(x,y);
```
which logically equals to

```cpp
if (condition){
	func1(x,y);
	func2(x,y);
}
else
	bar(x,y);
```

However, if we don't use "do ... while(0)", what we get as following:
**Wrong Example 1**
```cpp
//define a helper macro here
#define foo(x,y)		\
	{			    	\
		func1(x,y);		\
		func2(x,y);		\
	}

//use it

if (condition)
	foo(x,y); //just like a function call
else
	bar(x,y);
```

Expand-->

```cpp
if (condition)
	{func1(x,y);func2(x,y);}; \\one extra semicolon which makes no sense
else
	bar(x,y);
```





or if you omit brackets

**Wrong Example 2**

```cpp
//define a helper macro here
#define foo(x,y)		\
		func1(x,y);		\
		func2(x,y);

//use it

if (condition)
	foo(x,y); //just like a function call
else
	bar(x,y);
```

-->

```cpp
if (condition)
	func1(x,y);
	func2(x,y);
else
	bar(x,y);
```

which generates wrong code.
