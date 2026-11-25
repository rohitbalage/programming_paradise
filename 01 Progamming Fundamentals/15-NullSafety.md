# Null and Null Safety

![Null and Null Safety](null-safety.png)

## 1. What is Null?

Null represents the absence of a value or object.

Imagine an empty box. The box exists, but
there is nothing inside it.

```kotlin
val name: String? = null
```

Null is not the same as zero, false or an empty string.

| Value | Meaning |
|---|---|
| null | No value |
| 0 | A numeric value |
| "" | An empty string |
| false | A Boolean value |

## 2. What is a NullPointerException?

A NullPointerException (NPE) occurs when we try
to access something through a null reference.

### Java

```java
String name = null;

System.out.println(name.length()); // NullPointerException
```

Imagine trying to drive a car that does not exist.

The program cannot access the properties or
methods of a missing object.

## 3. What is Null Safety?

Null safety is a mechanism that helps prevent
errors caused by accessing null values.

Kotlin has built-in null safety.

### Non-Nullable Type

```kotlin
var name: String = "Alex"

name = null // Compilation error
```

### Nullable Type

```kotlin
var name: String? = "Alex"

name = null // Allowed
```

The question mark (?) after a type tells Kotlin
that the variable is allowed to contain null.

## 4. Null Safety Operators in Kotlin

### 4.1 Safe Call Operator (?.)

Accesses a property or method only if
the receiver is not null.

```kotlin
val name: String? = null

println(name?.length) // null
```

Another example:

```kotlin
val name: String? = "Alex"

println(name?.length) // 4
```

Think of it as:

"If something exists, access it.
Otherwise, return null."

### 4.2 Elvis Operator (?:)

Provides a default value when the left-hand
expression is null.

```kotlin
val name: String? = null

val displayName = name ?: "Guest"

println(displayName) // Guest
```

It can also return early from a function.

```kotlin
fun printName(name: String?) {
    val validName = name ?: return

    println(validName)
}
```

### 4.3 Non-Null Assertion Operator (!!)

Forces Kotlin to treat a nullable value
as non-null.

```kotlin
val name: String? = "Alex"

println(name!!.length) // 4
```

But if the value is null:

```kotlin
val name: String? = null

println(name!!.length) // NullPointerException
```

Avoid using !! unless you are certain
that the value cannot be null.

### 4.4 Safe Cast Operator (as?)

Attempts to cast an object to another type.

If the cast fails, it returns null.

```kotlin
val value: Any = 100

val text = value as? String

println(text) // null
```

## 5. Null Checks and Smart Casting

Kotlin can automatically treat a nullable
variable as non-null after a valid null check.

```kotlin
fun printLength(name: String?) {
    if (name != null) {
        println(name.length)
    }
}
```

This is called Smart Casting.

Smart casting works when the compiler can
guarantee that the checked value has not changed.

## 6. Safe Call with let

The let function can execute a block of code
only when used with a non-null safe call.

```kotlin
val name: String? = "Alex"

name?.let {
    println(it.length)
}
```

Output:

```text
4
```

If name is null, the let block is skipped.

## 7. Chaining Safe Calls

Imagine a user who may or may not have an address.

```kotlin
data class Address(val city: String?)

data class User(val address: Address?)

val user: User? = null

val city = user?.address?.city ?: "Unknown"

println(city) // Unknown
```

If any part of the chain is null,
the remaining safe calls return null.

The Elvis operator then provides a default value.

## 8. Null Safety in Java

Java traditionally uses explicit null checks.

```java
String name = null;

if (name != null) {
    System.out.println(name.length());
} else {
    System.out.println("Name is missing");
}
```

Java also provides Optional to represent
a value that may or may not exist.

```java
Optional<String> name = Optional.empty();

System.out.println(name.orElse("Guest"));
```

## 9. Null Pointers in C++

C++ uses nullptr to represent a null pointer.

```cpp
int* ptr = nullptr;

if (ptr != nullptr) {
    std::cout << *ptr;
}
```

Dereferencing a null pointer causes
undefined behavior.

Unlike Kotlin, C++ does not automatically
enforce null safety for raw pointers.

## 10. Real-World Android Example

Imagine receiving vehicle information from an API.

```kotlin
data class Vehicle(
    val name: String?,
    val batteryPercentage: Int?
)

val vehicle = Vehicle("Jeep", null)

val name = vehicle.name ?: "Unknown Vehicle"

val battery = vehicle.batteryPercentage
    ?.let { "$it%" }
    ?: "Unavailable"

println(name)    // Jeep
println(battery) // Unavailable
```

Null safety allows the application to handle
missing API data without crashing.

## 11. Quick Comparison

| Operator | Name | Purpose |
|---|---|---|
| `?` | Nullable type | Allows null |
| `?.` | Safe call | Access safely |
| `?:` | Elvis | Provide a fallback |
| `!!` | Non-null assertion | Fail if null |
| `as?` | Safe cast | Return null if casting fails |
| `!= null` | Null check | Check whether a value exists |

## 12. Best Practices

- Prefer non-nullable types whenever possible.
- Use ?. to safely access nullable objects.
- Use ?: to provide meaningful fallback values.
- Avoid unnecessary use of !!.
- Do not replace missing data with misleading values.
- Validate nullable data received from APIs.

## 13. Key Takeaways

- Null means the absence of a value.
- Null is different from zero or an empty string.
- Kotlin separates nullable and non-nullable types.
- Safe calls prevent many null-related crashes.
- Null safety does not guarantee that every
  NullPointerException is impossible.