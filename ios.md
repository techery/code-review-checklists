# Code review checklist for iOS

## 1. Basics on naming classes, variables, accessors, general methods and abbreviations. 

**1.1 Putting Thought into Naming**

Cocoa strongly encourages expressive, clear, non-ambiguious names. Take a look at the classes listed in the table to the left. You have some idea of what they do just from their name.
Also notice how some are combinations of other class names. NSMenuItemCell suggests a cell which is used by a menu item.

**1.2 Class Names**

Whether they're a standard part of Cocoa or your own creation, class names are always capitalized.

Objective-C doesn't have namespaces, so prefix your class names with initials. This avoids "namespace collision," which is a situation where two pieces of code have the same name but do different things. Classes created by Cocoa Dev Central would probably be prefixed with "CDC".

If you subclass a standard Cocoa class, it's good idea to combine your prefix with the superclass name, such as CDCTableView.

Most of the standard Cocoa classes begin with NS for historical reasons, as Cocoa is based on the NeXTSTEP development framework.

**1.2 Variable Names**

Variable names start with lower-case letters, but are internally capitalized wherever a new word appears:


NSString * streetAddress  = @"1 Infinite Loop";
NSString * cityName       = @"Cupertino";
NSString * countyName     = @"Santa Clara";
As we discussed, Cocoa strongly favors clear, distinct variable names over terse ones. Ambiguity frequently results in bugs, so be explicit.

**Correct**

```
NSString       *hostName;
NSNumber       *ipAddress;
NSArray        *accounts;
```

**Incorrect**

```
NSString       *HST_NM;      // all caps and too terse
NSNumber       *theip;       // a word or abbreviation?
NSMutableArray *nsma;        // completely ambiguous
```

The variable naming syntax rules from C and many other languages apply. Variables can't start with a number, no spaces, no special characters other than underscores.

Apple discourages using an underscore as a prefix for a private instance variable.

```
NSString *name    // correct!
NSString *_name   // _incorrect_
```

In fact, the idea of a private prefix is largely unnecessary, as virtually all instance variables in Cocoa are protected anyway.

**1.3 Method Names: Accessors**

In constrast to many other languages, Objective-C discourages use of the "get" prefix on simple accessors. Instance variables and methods can have the same name, so use this to your advantage:

**Correct!**

```
- (NSString *) name;
- (NSColor *) color;

name  = [object name];
color = [object color];
```

**Incorrect**

````
- (NSString *) getName;
- (NSColor  *) getColor;

name  = [object getName];
color = [object getColor];
```

The "get" prefix is, however, used in situations when you're returning a value indirectly via a memory address:

When to Use "Get" Prefix

```
// copy objects from the NSArray to the buffer

id *buffer = (id *) malloc(sizeof(id) * [array count]);
[array getObjects: buffer];
( Don't worry if you don't know what malloc does. )
```

The "set" prefix is always used on setters, though:

```
[object setName:  name];
[object setColor: color];
```

**1.4 Adjectives**

Not all accessors return values like name, date, height, etc. Some represent a particularly quality of an object. These are often represented by BOOLs.

For example, "selectable". In Objective-C, the getter for this key is called -isSelectable, but the setter is -setSelectable:

```
BOOL selectable = [textView isSelectable];
BOOL editable   = [textView isEditable];

[textView setSelectable: YES];    // no "is"
[textView setEditable:   YES];    // no "is"

// if textview is editable.

if ([textView isEditable])
      [textView setEditable: NO];
```

Keep in mind that naming your accessors according to all of these rules isn't purely an issue of clarity and aesthetics. Cocoa relies heavily on key-value coding for much of its magic, and KVC relies on properly-named accessors.

**1.5 Preprocessor Contants / Macros**

Just like in ANSI C, preprocessor definitions should be in all caps

```
#define MAX_ENTRIES 20
#ifdef  ENABLE_BINDINGS_SUPPORT
```

**1.6 Method Names: Returning Objects**

In addition to simple accessors, classes or objects can also return objects based on various conditions or input. The format is simple:

```
[object/class thing+condition];
[object/class thing+input:input];
[object/class thing+identifer:input];
```

Examples

```
realPath    = [path     stringByExpandingTildeInPath];
fullString  = [string   stringByAppendingString:@"Extra Text"];
object      = [array    objectAtIndex:3];

// class methods
newString   = [NSString stringWithFormat:@"%f",1.5];
newArray    = [NSArray  arrayWithObject:newString];
```

If I wrote some of my own, this is what they might look like.

```
recipients  = [email    recipientsSortedByLastName];
newEmail    = [CDCEmail emailWithSubjectLine:@"Extra Text"];
emails      = [mailbox  messagesReceivedAfterDate:yesterdayDate];
```

Note that all of these messages first indicate what kind of thing will be returned, followed by what the circumstances will be for returning it. Also note that the last word right before the colon describes the type of the supplied argument.

Sometimes, you want a variation on a value. In that case, the format is generally:

```
[object adjective+thing];
[object adjective+thing+condition];
[object adjective+thing+input:input];
```

Examples

```
capitalized = [name    capitalizedString];
rate        = [number  floatValue];
newString   = [string  decomposedStringWithCanonicalMapping];
subarray    = [array   subarrayWithRange:segment];
```
