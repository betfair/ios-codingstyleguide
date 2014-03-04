# Betfair Objective-C Style Guide

This style guide outlines the coding conventions of the iOS team at Betfair. We welcome your feedback in [issues](https://github.com/betfair/ios-codingstyleguide/issues) and [pull requests](https://github.com/betfair/ios-codingstyleguide/pulls).

## Introduction

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)

## Dot-Notation Syntax

Dot-notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* also make sure you have one empty line before body.

**For example:**
```objc
if (user.isHappy) {

//Do something
} else {

//Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.
