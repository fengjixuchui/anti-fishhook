# AntiFishhook

__AntiFishhook__ is an AntiHook library for [`fishhook`][fishhook] at runtime (make fishhook doesn't work) .  
include `fishhook` and `anti-fishhook`

[fishhook]: https://github.com/facebook/fishhook
[Swift Name Mangling]: https://www.mikeash.com/pyblog/friday-qa-2014-08-15-swift-name-mangling.html

[How it's work](https://github.com/TannerJin/IOSSecuritySuite/blob/master/IOSSecuritySuite/FishHookChecker.swift#L13)

### Note

 Run or test in your phone(arm64) instend of simulator   
 [`Swift Function name mangling`][Swift Name Mangling]

## Usage

### antiFishhook

```swift
import antiFishhook

FishHookChecker.denyFishHook("$s10Foundation5NSLogyySS_s7CVarArg_pdtF")  // Swift's Foudation.NSLog  
NSLog("Hello AntiFishHook")

FishHookChecker.denyFishHook("printf")                                  // printf
printf("Hello AntiFishHook")
```


### fishhook

```swift
typealias MyNSLog = @convention(thin) (_ format: String, _ args: CVarArg...) -> Void

func myNSLog(_ format: String, _ args: CVarArg...) {
    print("Hello fishHook")
}

let selfNSLog: MyNSLog  = myNSLog
let selfNSLogPointer = unsafeBitCast(selfNSLog, to: UnsafeMutableRawPointer.self)
var origNSLogPointer: UnsafeMutableRawPointer?

FishHook.replaceSymbol("$s10Foundation5NSLogyySS_s7CVarArg_pdtF", newMethod: selfNSLogPointer, oldMethod: &origNSLogPointer)

NSLog("Hello World")
// will print Hello fishHook

```

### Suggestion

Use by adding source file to your project instend of pod
