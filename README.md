KCMutableDictionary
===================

`KCMutableDictionary` is a subclass of `NSMutableDictionary` that is transparently persisted in your app's keychain. The general idea is to make secure storage of small items in the keychain as easy as using a mutable dictionary.

Four restrictions are imposed:

1. `KCMutableDictionary` may not be initialized with data. Use, for example, `[KCMutableDictionary dictionary]` or `KCMutableDictionary.new`.

2. Keys and values are restricted to property list objects, that is, objects that can be serialized with `NSPropertyListSerialization`. This includes `NSData`, `NSString`, `NSArray`, `NSDictionary`, `NSDate`, and `NSNumber`, but not (for example) `NSSet`.

3. Mutable values may be stored in `KCMutableDictionary`, but they will be replaced with immutable copies.

4. Reads are fast, but writes are (relatively) slow.

Using KCMutableDictionary
-------------------------

Copy `KCMutableDictionary.h` and `KCMutableDictionary.m` to your project and `#import "KCMutableDictionary.h"` as required. When the dictionary is instantiated (`[KCMutableDictionary dictionary]` is as good a method as any), it automatically populates itself from the keychain. Stores are synchronous: the dictionary is immediately written to the keychain and then read back (so that the dictionary items are now immutable copies of the originals).

For security reasons, you may not want the dictionary to remain resident in memory once you're done with it. Do not remove items from the dictionary, since this will also remove them from the backing store in the keychain. Instead, release the dictionary by assigning `nil` to its reference.

The serialized dictionary is stored by default in the keychain with the key `<bundleID>.__KCMutableDictionary__`. Named dictionaries can be stored in the keychain as well; used the initializer `initWithName:` to use a dictionary named `<bundleID>.__KCMutableDictionary__.<name>`.

Acknowledgements
----------------

`KCMutableDictionary` was inspired by Mark Granoff's [granoff/Lockbox](https://github.com/granoff/Lockbox) and a suggestion by [Ernesto Rivera](https://github.com/rivera-ernesto). Matt Gallagher has a useful [post](http://cocoawithlove.com/2008/12/ordereddictionary-subclassing-cocoa.html) on the subject of subclassing `NSMutableDictionary` at [Cocoa with Love](http://cocoawithlove.com) that I didn't stumble on until too late to save me some time. Thanks also to my many smart friends.