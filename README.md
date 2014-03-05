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
  * [Operators](#operators)
  * [Properties](#properties)
  * [Protocol References](#protocol-references)
  * [Code Separation](#code-separation)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Methods](#methods)
* [Variables](#variables)
  * [Synthesize](#synthesize)
  * [Types](#types)
* [Naming](#naming)
  * [Common Naming](#common-naming)
  * [UIKit Components](#uikit-components)
  * [Category File Names](#category-file-names)
* [Comments](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Fast Enumeration](#fast-enumeration)
* [Blocks](#blocks)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Imports](#imports)
* [Enumerated Types](#enumerated-types)
* [Protocols & Delegates](#protocols-and-delegates)
* [Private Properties](#private-properties)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Code Commits](#code-commits)

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

Also use dot notation for setters and getters, even if it's not explicitly declared as a property. Mixing in one line is fine

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* also make sure you have one empty line before body.

**For example:**
```objc
if (user.happy) {

    // Do something
    
} else {

    // Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.
* space out blocks of code within methods in a readable way

### Operators
Use one space separation between operators for clarity

**For example:**
```objc
UIEdgeInsetsMake(4, 1, 5, 6);
self.view.autoresizingMask = UIViewAutoresizingFlexibleHeight | UIViewAutoresizingFlexibleWidth;

for (int i = 0; i < 2; i++) {

}
```

**Not:**
```objc
UIEdgeInsetsMake(4,1,5,6);
self.view.autoresizingMask=UIViewAutoresizingFlexibleHeight|UIViewAutoresizingFlexibleWidth;

for (int i=0; i<2; i++) {

}
```

### Properties

Spacing and order of property declarations (first attribute atomic/nonatomic, followed by strong/weak/copy)

**For example:**
```objc
@property (nonatomic) IBOutlet WebViewLabel *introLabel;
```

**Not:**
```objc
@property(nonatomic,strong) IBOutlet WebViewLabel *introLabel;
```

### Protocol References

Spacing of protocol references:

```objc
@property (nonatomic, weak) id<UITableViewDelegate> delegate;
@property (nonatomic, weak) id<UITableViewDelegate, Protocol2> delegate;
```

### Code Separation

Use `#pragma mark` to make navigating the code easier in XCode (an empty value or a '-' creates a horizontal rule). 
Use shorthand.

**For example:**
```objc
#pragma mark - Legal Footer
```

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors of adding a second line and expecting it to be part of the if-statement. Another defects may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement (this might become more dangerous issue). In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**For example:**
```objc
if (!error) {
	
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```
or
```objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, ? , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables.

* use braces around the evaluation part.
* might use `?:` form as well

**For example:**
```objc
result = (a > b) ? x : y;
```
or
```objc
result = (a) ?: b;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error handling

* Methods which return error parameter should also have `BOOL` return type.
* When methods return an error parameter by reference, switch on the returned value, not the error variable.

**For example:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
	
    // Handle Error
}
```

**Not:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
	
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Methods

In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.

* Use keywords before all arguments
* Make the word before the argument describe the argument.
* Don’t use "and" to link keywords that are attributes of the receiver. Although "and" may sound good in some cases, it causes problems as you create methods with more and more keywords.
* Although if the method describes two separate actions, use "and" to link them:

**For Example**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (int)runModalForDirectory:(NSString *)path andFile:(NSString *)name andTypes:(NSArray *)fileTypes;

- (BOOL)openFile:(NSString *)file withApplication:(NSString *)application andDeactivate:(BOOL)deactivate;
```

For methods that represent actions an object takes, start the name with a verb. 

**For Example**:
```objc
- (void)showAlert;
```

Use "did" or "will" for methods that are invoked to notify the delegate that something has happened or is about to happen.

**For Example**:
```objc
- (void)webViewDidScroll:(UIWebView *)sender;
- (void)accountBarWillShow:(AccountBarView *)accountBarView;
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**For example:**
```objc
@interface BFMarket: NSObject

@property (nonatomic) NSString *marketName;

@end
```

**Not:**
```objc
@interface BFMarket : NSObject {
    NSString *marketName;
}
```

### Synthesize

Use `@synthesize` only if required, for example:

* You are implementing all accessor methods
* The property is declared in a protocol
* The class or one of its parent is tagged with NS_REQUIRES_PROPERTY_DEFINITIONS.
* Always have one variable `@synthesize` per line.

This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**For example:**
```objc
@synthesize keyboardResponderView = _keyboardResponderView;
```

**Not**
```objc
@synthesize restClient, accountBar;
```

### Types

* When using primitive types, use the foundation types, e.g. NSInteger.
* Use BOOL for boolean values, and YES/NO;

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**For example:**
```objc
UIButton *settingsButton;
```

**Not**
```objc
UIButton *setBut;
```

A three letter prefix (e.g. `BUI`) should always be used for class names and constants, however may be omitted for Core Data entity names. Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**For example:**
```objc
static const NSTimeInterval BUIViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not:**
```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties and local variables should be camel-case with the leading word being lowercase. 

Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**For example:**
```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**
```objc
id varnm;
```

It is good to be both clear and brief as possible, but clarity shouldn’t suffer because of brevity.
In general, don’t abbreviate names of things. Spell them out, even if they’re long.

**For example:**
```objc
- (void)insertObject:(id)object atIndex:(NSInteger)index;
- (void)removeObjectAtIndex:(NSInteger)index;
- (void)removeObject:(id)object;
```

**Not:**
```objc
- (void)insert:(id)object at:(NSInteger)index;
- (void)remove:(id)object;
```

### UIKit Components

For UIKit items, suffix the variable name with the type of control:

```objc
@property (nonatomic) IBOutlet UILabel *passwordLabel;
@property (nonatomic) IBOutlet CheckboxButton *confirmationButton;
```

The exception might be with local variables where the declaration is very close to the references, e.g:

```objc
UIImageView *blueBackground = [[UIImageView alloc] initWithFrame:CGRectMake(0, 25, 320, 40)];
[cell.contentView addSubview:blueBackground];
```

### Category File Names

Category file naming should be as follows:

```objc
#import "UIViewController_PlayerProtectionNotificationsManager.h"
#import "UIViewController+ErrorHandlingAdditions.h"
#import "UIViewController+PlayerProtectionNotificationsManager.h"
```

### Common Naming

Common method argument names used everywhere in Apple frameworks:

```objc
...action:(SEL)aSelector
...alignment:(int)mode
...atIndex:(int)index
...content:(NSRect)contentRect
...doubleValue:(double)value
...floatValue:(float)value
...font:(NSFont *)font
...frame:(NSRect)frame
...frame:(NSRect)aFrame
...intValue:(int)value
...keyEquivalent:(NSString *)charCode
...length:(int)numBytes
...centerPoint:(NSPoint)point
...stringValue:(NSString *)value
...tag:(NSInteger)tag
...target:(id)target
...title:(NSString *)title
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

Place comments in the header file for things intended to be used by other classes.
 
In some cases, however, it may be clearer to inline your comments.

**For example:**
```objc
typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
    UITableViewCellStyleDefault, // Simple cell with text label and optional image view (behavior of UITableViewCell in iPhoneOS 2.x)
    UITableViewCellStyleValue1, // Left aligned label on left and right aligned label on right with blue text (Used in Settings)
    UITableViewCellStyleValue2, // Right aligned label on left with blue text and left aligned label on right (Used in Phone/Contacts)
    UITableViewCellStyleSubtitle // Left aligned label on top and left aligned label on bottom with gray text (Used in iPod).
};
```

## Imports

Some simple rules regarding imports:

* Only `#import` the super class in header files, or any classes that define any protocols the class adopts 
* `#import` all classes you send messages to in the implementation
* Forward declarations for everything else

Always use forward class declarations in header files whenever possible:

```objc
@class CheckboxButton;
```

## init and dealloc

`dealloc` methods should be placed at the **very bottom** of the implementation. `init` should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements.

Mostly use `instancetype` for the `init` methods.

`init` methods should be structured like this:

```objc
- (instancetype)init {
	
    self = [super init]; // or call the designated initalizer
    if (self) {
		
        // Custom initialization
    }

    return self;
}
```

## Fast Enumeration

Iterate through collections in the simplest way possible, for readability, unless more advanced features required (e.g. enumerateWithBlock).

## Blocks

Callback blocks should have the caller as the first parameter.

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**For example:**
```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**
```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**For example:**
```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**
```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

Define constants that need to be accessed from other classes in the header, otherwise put them in the implementation file.

**For example:**
```objc
static NSString * const BUIViewControllerCompanyName = @"Betfair Company";

static const CGFloat BUIImageThumbnailHeight = 50.0;
```

**Not:**
```objc
#define CompanyName @"Betfair Company"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**
```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

Also note the prefixes on the enum values are common to the enum name. Always put the default value at the top if the value is zero. Only define int values if the value means something, e.g. a bitfield.


## Protocols & Delegates

Use optional and required in protocol definitions with the following spacing if there is a mix:

**Example:**
```objc
@protocol TestProtocol <NSObject>
@optional
- (void)optionalProtocolMethod;
@required
- (void)requiredProtocolMethod;
@end
```

Delegate methods should identify the object sending the message by starting with the name.

**For example:**
```objc
- (BOOL)tableView:(UITableView *)tableView shouldSelectRow:(NSUInteger)row;
- (BOOL)application:(UIApplication *)sender openFile:(NSString *)filename;
```

The exception is if we only have one argument, the sender:

**For example:**
```objc
- (BOOL)notificationDidSend:(NSNotification *)notification;
```

Delegates must be referenced as `weak` in order to prevent retain circles.

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `BFPrivate` or `private`) should never be used unless extending another class.

**For example:**
```objc
@interface BFMarket ()

@property (nonatomic) BFMarketID *marketID;
@property (nonatomic) NSUInteger numberOfWinners;

@end
```

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**For example:**
```objc
if (!someObject) {
	
}
```

**Not:**
```objc
if (someObject == nil) {
	
}
```

**For a `BOOL`, here are two examples:**
```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**
```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:
```objc
@property (nonatomic, getter = isEditable) BOOL editable;
```

Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.

```objc
+ (instancetype)sharedInstance {
	
   static id sharedInstance = nil;
   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
	   
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Code Commits

When committing to source control, the message should start with the ID of the User Story worked on (e.g. "US39872 - Blah"). If multiple message lines are needed, provide a summary in the first line, then leave a line, then provide bullet points.

**For example:**
```objc
US39872 - Adds translations and checks safe language codes passed to APIs

- Adds pt-BR and es-AR languages
- Modifies RESTClient to pass safeUserLanguage to APIs now that we have hyphens in our language codes
```

When deciding what to commit - consider if a user can take a snapshot and have a usable build.

# Other Objective-C Style Guides

If ours doesn't fit your tastes, have a look at some other style guides:

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
