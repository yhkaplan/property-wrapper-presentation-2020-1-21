slidenumbers: true
slide-transition: true

# [fit] Property Wrappers

---

# [fit] What are they?

---

- New in 5.1
- Java-like annotations
- Can accept (generic) parameters
- Used in SwiftUI

---

# [fit] Purpose

^ To reduce code
To provide a standard way to mutate properties like lazy keyword in stdlib

---


# [fit] Examples

---

# [fit] SwiftUI

---

```swift
struct ContentView: View {
  @State private var value = 0.0

  var body: some View {
    VStack {
      Text("Value is \(value)")
      Slider(value: $value)
    }
  }
}
```

^ @State, @ObservedObject, @EnvironmentObject, @GestureState, @Binding

---

# UIKit/Foundation

---

```swift
class ViewController: UIViewController {
  @Keychain(key: "secret_info") var secretInfo = ""
}
```

^ Keychain, UserDefaults, Codable

---

# [fit] Let's make one!

---

```swift
@propertyWrapper
struct TwelveOrLess {
  private var number = 0

  var wrappedValue: Int {
    get { return number }
    set { number = min(newValue, 12) }
  }
}

// Use
struct S {
  @TwelveOrLess var num = 13 // 12
}
```

^Example from The Swift Programming Language, must provide a wrappedValue

---

```swift
@propertyWrapper
struct Clamped {
  private var number = 0

  private let maxNum: Int
  private let minNum: Int

  var wrappedValue: Int {
    get { return number }
    set { number = max(min(newValue, maxNum), minNum) }
  }
}

// Use
struct S {
  @Clamped(maxNum: 10, minNum: 0) var num = 13 // 10
}
```

^More generic, they can accept properties themselves

---

# [fit] Projected values

---

```swift
@propertyWrapper
struct State<T> {
    //...
    var projectedValue: Binding<T>
}
```

^Allows you to access the additional metaproperties, in this case, the type that is a Binding.

---

```swift
struct ContentView: View {
  @State private var isDisabled = false

  var body: some View {
    OtherView($isDisabled) // Binding<Bool>
  }
}
```

^ Projected values can be any type, allowing you to present useful related types and data. Use a dollar sign to access. The dollar sign is a reserved symbol in Swift, so a conflicting name cannot be defined.
Show previous SwiftUI example

---

# Conclusion

^ Great for Codable, Keychain, and UserDefaults, and storage
But can't throw, or ensure things with advanced compiler errors, really using Swift's type system because they work at runtime
Like operator overloads, very concise and powerful, but not to be overused
Take a look at Burritos and see if any of those look useful

---

# More info

---

- The Swift Programming Language
- [Burritos](https://github.com/guillermomuntaner/Burritos)

