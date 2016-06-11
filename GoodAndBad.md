
# The Good the Bad

This document contains information about good and bad features in our programming languages.
It can be used to:
- Determine which language is right for your project
- Learn from past mistakes so we can develop a brighter future



## Primitive Types

All system languages should support fixed-width types like `int32_t`, `uint8_t`, etc and also
platform-dependent types like `size_t`.  These types are necessary to support platform
independent code and should be included in the language itself, or at least in a standard
library available to virtually all platforms.

### Type Conversion

Not sure there is a best way to do this.  I should list the possible ways you can deal
with this and then compare the pros/cons.

## Macros



## Enums

Rust's approach to enums is interesting. It works well with their `match` construct.
It can be implemented in other languages with some extra code since they are really just
tagged unions.  So then an enum that doesn't contain a payload just has a tag which acts
like enums in other languages.

Here's one of the common usages of Rust enums.

```
// Rust Definition
enum Result<T,E> {
  Ok(T),
  Error(E)
}
// Rust Usage
fn someFunction() -> Result(u32, u32)
{
  Result.Ok(0)
}
match someFunction() {
  Error(e) => /* handle the error */,
  Ok(value) => /* handle success */,
}

// D Definition
enum ResultTag {ok, error}
struct Result(T,E) {
  ResultTag tag;
  union Payload {
    T value;
    E error;
  }
  Payload payload;
  //alias this payload; // magic sauce
  
  static Result Ok(T value) {
    Result result;
    result.tag = ResultTag.ok;
    result.payload.value = value;
    return result;
  }
  static Result Error(E error) {
    Result result;
    result.tag = ResultTag.error;
    result.payload.error = error;
    return result;
  }
}
// D Usage
auto someFunction()
{
  return Result.Ok(0);
}
{
  auto result = someFunction();
  final switch(result.tag) {
  case ResultTag.ok:
    writefln("Success!");
    break;
  case ResultTag.error:
    writefln("Error!");
    break;
  }
}
```


## Arrays

Some info on arrays.  Alot of this information is shared with strings.

### Slices

Slices are a very simple concept that become very useful when implemented by the language.
Special syntax support for them offers convenience and enourages usage.

## A Standard String Type

C++ has taught us that if the language and the standard library do not provide a sufficient string type
then everyone will create their own.  Then for every new string type you've imported, you have to write
n squared conversions to and from every string type.

A sufficient standard string type should have a stored length without having to traverse it.
Using a struct with a pointer and a length seems to be quite popular and the most efficient representation in some cases.
However there are some situatios where a pointer to the string, and a pointer to the end of the string, i.e.
```
  | H | e | l | l | o | ? |
    ^                   ^
   ptr                limit
```
Most parsing functions do better with this type of string representation because they 

The better representation depends on what you do with the string.
If you need to know the length of the string, you should use a length, if you plan on
creating alot of slices, then the limit version might be better.


### Standard String Encoding

Standard strings should all be encoded using UTF-8.

### Null-Terminated

If you want to interoperate with other languages, especially C, there is value to making your strings null-terminated, and the cost of
adding an extra null character to the end of every string may be worth it.  Also, if all strings are guaranteed to be null-terminated, some
functions that don't need to know the length of the string (like parsing functions) could just input the pointer.

## Garbage Collection

This is a **BIG** topic.
In an ideal world, the language will support both garbage collected programs an non-garbage collected programs.
The standard library should also provide functionality for both types.
When a project does not use a garbage collector, it would also be nice if the compiler automatically injected
the code to clean up memory for you, and also verified memory safety at compile time (like rust).
Rust's approach may seem like the best of both worlds, however, the efficiency and safety requires sacrificing
simplicity resutling in longer development so as to satisfy the strict requirements necessary to
guarantee memory safety without a garbage collector.

## Function Overloading

## Parameters

### Named parameters
### Optional parameters
### Parameter default values

## Templates

## Compile-Time Function Evaluation (CTFE)

## Structs

## Classes

## foreach loop

Some type of foreach loop seems to be a very nice feature.

## D's alias this

Really cool feature :)

## D's Ranges

...

## D's Contract Programming

## D's template system

## D's mixins

## Uniform Function Call Symtax (UFCS)

```
function doSomething(MyType m) { ... }
doSomething(m); // normal call
m.doSomething(); // UFCS call
```


Nice feature, I think.  Are there con's to this?

## D's Storage Classes

D's storage classes like `const` and `immutable` don't seem to work very well.  They seem to only help in a small number
of cases, and there seem to be many cases that they just can't help with. I think this is a failed
feature that needs a rework.

## Function Guards

Seems like a really good idea to me.  In some cases function guards can be checked at compile time.
```
funcion divide(float a, float b) guard(b != 0) {
    return a / b;
}

print("10/3 = %s", divide(10, 3));
```

## Safe and Unsafe mode

Nice feature.

## Final Switch

Switch statements that guarantee all values have a branch.
D has `final switch` which works on enums.
Rust has the whole `match` construct which supports enum and MUCH MUCH more.

## Limiting Variable Modification

Rust has this cool feature that allows you to shadow a variable and rebind it as immutable.
When a developer looks at the code later, this will allow them to assume that the variable does
not change after it is shadowed by an immutable binding.  This means there's less for the developer
to keep track of when trying to understand code.



## Linking files

Good ideas/Bad ideas?

## Using whitespace for nesting code blocks

Python...etc