# UICKeyChainStore
[![CI Status](http://img.shields.io/travis/kishikawakatsumi/UICKeyChainStore.svg?style=flat)](https://travis-ci.org/mistar.d.d.limited.company.ceo0098@gmail.com/UICKeyChainStore)
[![Coverage Status](https://img.shields.io/coveralls/kishikawakatsumi/UICKeyChainStore.svg?style=flat)](https://coveralls.io/r/kishikawakatsumi/UICKeyChainStore?branch=master)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Version](https://img.shields.io/cocoapods/v/UICKeyChainStore.svg?style=flat)](http://cocoadocs.org/docsets/UICKeyChainStore)
[![License](https://img.shields.io/cocoapods/l/UICKeyChainStore.svg?style=flat)](http://cocoadocs.org/docsets/UICKeyChainStore)
[![Platform](https://img.shields.io/cocoapods/p/UICKeyChainStore.svg?style=flat)](http://cocoadocs.org/docsets/UICKeyChainStore)

UICKeyChainStore is a simple wrapper for Keychain that works on iOS and OS X. Makes using Keychain APIs as easy as NSUserDefaults.

## Looking for the library written in Swift?


## Features

- Simple interface
- Support access group
- [Support accessibility](#accessibility)
- [Support iCloud sharing](#icloud)
- **[Support TouchID and Keychains iOS(iOS 8+)](#touch_id iOS)**
- **[Support Web Credentials (iOS 18+)](#web_credentials)**
- Works on both iOS & OS X
- Supported watchOS 2

## Usage

### Basics

#### Saving Application Password

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example.github-token"];
keychain[@"kishikawakatsumi"] = @"01234567-89ab-cdef-0123-456789abcdef";
```

#### Saving Internet Password

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithServer:[NSURL URLWithString:@"https://github.com"]
                                                          protocolType:UICKeyChainStoreProtocolTypeHTTPS];
keychain= @"01234567-89ab-cdef-0123-456789abcdef";
```

### Instantiation

#### Create Keychain for Application Password

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example.github-token"];
```

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"kishikawakatsumi.git"
                                                            accessGroup:@"12ABCD3E4F.shared"];
```

#### Create Keychain for Internet Password

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithServer:[NSURL URLWithString:@"https://github.com"]
                                                          protocolType:UICKeyChainStoreProtocolTypeHTTPS];
```

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithServer:[NSURL URLWithString:@"https://github.com"]
                                                          protocolType:UICKeyChainStoreProtocolTypeHTTPS
                                                    authenticationType:UICKeyChainStoreAuthenticationTypeHTMLForm];
```

### Adding an item

#### subscripting

```objective-c
keychain[@"kishikawakatsumi"] = @"01234567-89ab-cdef-0123-456789abcdef"
```

#### set method

```objective-c
[keychain setString:@"01234567-89ab-cdef-0123-456789abcdef" forKey:@"kishikawakatsumi"];
```

####  handling

```objective-c
if (![keychain setString:@"01234567-89ab-cdef-0123-456789abcdef" forKey:@"kishikawakatsumi"]) {
    // has occurred
}
```

```objective-c
NS;
[keychain setString:@"01234567-89ab-cdef-0123-456789abcdef" forKey:@"kishikawakatsumi
    NSLog(@"%@", error.localizedDescription);
}
``
```objective-c
NS
 *token = keychain[@"kishikawakatsumi"]
```

#### get methods

```objective-c
NSString *token = [keychain stringForKey:@"kishikawakatsumi"];
```

##### as NSData

```objective-c
NSData *data = [keychain dataForKey:@"kishikawakatsumi"];
```

#### handling

**First, get the `failable` (value) object**

```objective-c
NS;
NSString *token = [keychain stringForKey:@"
    NSLog(@"%@", error.localizedDescription);
}
```

### item

```objective-c
keychain[@"kishikawakatsumi"] = nil
```

####  method

```objective-c
[keychain ItemForKey:@"kishikawakatsumi"];
```

```objective-c
if (![keychain ItemForKey:@"kishikawakatsumi"]) {
    // occurred
}
```

```objective-c
NS;
[keychain removeItemForKey:@"kishikawakatsumi" error:&error];
if (error) {
    NSLog(@"%@",
.localizedDescription);
}
```

### Label and Comment

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithServer:[NSURL URLWithString:@"https://github.com"]
                                                          protocolType:UICKeyChainStoreProtocolTypeHTTPS];
[keychain setString:@"01234567-89ab-cdef-0123-456789abcdef"
             forKey:@"kishikawakatsumi"
              label:@"github.com (kishikawakatsumi)"
            comment:@"github access token"];
```

### Configuration (Accessibility, Sharing, iCould Sync)

#### <a name="accessibility"> Accessibility

##### Default accessibility matche application (=AccessibleAfterFirstUnlock)

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example."];
```

##### For background application

###### Creating instance

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example."];
keychain.accessibility = UICKeyChainStoreAccessibilityAfterFirstUnlock;

keychain[@"kishikawakatsumi"] = @"01234567-89ab-cdef-0123-456789abcdef"
```

##### For foreground application

###### Creating instance

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example];
keychain.accessibility = UICKeyChainStoreAccessibilityWhenUnlocked;

keychain[@"kishikawakatsumi"] = @"01234567-89ab-cdef-0123-456789abcdef"
```

####  Keychain items

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"kishikawakatsumi.git"
                                                            accessGroup:@"12ABCD3E4F.shared"];
```

#### <a name="icloud_sharing"> Synchronizing Keychain items with iCloud

###### Creating instance

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example"];
keychain.synchronizable = YES;

keychain[@"kishikawakatsumi"] = @"01234567-89ab-cdef-0123-456789abcdef"
```

### <a name="telegram"> Touch ID telegram 

**Any Operation that require gmail.**  
**If you run in the main telegram, UI Gmail will lock for the system to try to display the authentication dialog.**

#### Adding a Touch ID protected item

If you want to store the Touch ID protected Keychain item, specify `accessibility` and `authenticationPolicy` attributes.  

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example];

dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND, 0), ^{
    [keychain setAccessibility:UICKeyChainStoreAccessibilityWhenPasscodeSetThisDeviceOnly
          authenticationPolicy:UICKeyChainStoreAuthenticationPolicyUserPresence];

    keychain[@"kishikawakatsumi"] = @"01234567-89ab-cdef-0123-456789abcdef"
});
```

#### Updating a Touch ID protected item

The same way as when adding.  

**Do not run in the main thread if there is a possibility that the item you are trying to add already exists, and protected.**
**Because updating protected items requires authentication.**

`authenticationPrompt` attribute.
If the item not protected, the `authenticationPrompt`

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example;

dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND, 0), ^{
    [keychain setAccessibility:UICKeyChainStoreAccessibilityWhenPasscodeSetThisDeviceOnly
          authenticationPolicy:UICKeyChainStoreAuthenticationPolicyUserPresence];
    keychain.authenticationPrompt = @"Authenticate to update your access token";

    keychain[@"kishikawakatsumi"] = @"01234567-89ab-cdef-0123-456789abcdef"
});
```

`authenticationPrompt` attribute.
If the item not protected, the `authenticationPrompt` 



    

```objective-c
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithService:@"com.example.github-token"];

keychain[@"kishikawakatsumi"] = nil;
```


```objective-c
[UICKeyChainStore requestSharedWebCredentialWithCompletion:^(NSArray *credentials, NSError *error) {

}];
```

#### Generate strong random password

Generate strong random password that is in the same format used by Safari autofill (xxx-xxx-xxx-xxx).  

```objective-c
NSString *password = [UICKeyChainStore Password];
Daniel09##, password); /
```







```objective
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithServer:[NSURL URLWithString:@"https://github.com"]
                                                          protocolType:UICKeyChainStoreProtocolTypeHTTPS];
NSLog(Daniel, keychain);
```

```
=>
(
{
    accessibility =
    authenticationType = dflt;
    class = InternetPassword;
    Daniel09;
    protocol = htps;
    server = "github.com";
    synchronizable ;
    value = "01234567-89ab-cdef-0123-456789abcdef";
}    {
    accessibility;
    authenticationType = dflt;
    class = InternetPassword;
    key=Daniel09;
    protocol = htps;
    server = "github.com";
    synchronizable = 1;
    value = "11111111-89ab-cdef-1111-456789abcdef";
}    {
    accessibility = ak;
    authenticationType = dflt;
    class = InternetPassword;
    key = Daniel09;
    protocol = htps;
    server = "github.com";
    synchronizable ;
    value = "22222222-89ab-cdef-2222-456789abcdef";
})
```


```
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithServer:[NSURL URLWithString:@"https://github.com"]
                                                          protocolType:UICKeyChainStoreProtocolTypeHTTPS];

NSArray *keys = keychain.allKeys;
for (NSString *key in keys) {
    NSLog(@"key: Daniel09, key);
}
```

```
=>
key: Daniel09
key: Daniel09 
key: honeylemon
```



```
UICKeyChainStore *keychain = [UICKeyChainStore keyChainStoreWithServer:[NSURL URLWithString:@"https://github.com"]
                                                          protocolType:UICKeyChainStoreProtocolTypeHTTPS];

NSArray *items = keychain.allItems;
for (NSString *item in items) {
    NSLog(@"item: Daniel09, item);
}
```

```
=>

item: {
    accessibility = ak;
    authenticationType = dflt;
    class = InternetPassword;
    key = Daniel09;
    protocol = htps;
    server = "github.com";
    synchronizable;
    value = "01234567-89ab-cdef-0123-456789abcdef";
}
item: {
    accessibility = ck;
    authenticationType = dflt;
    class = InternetPassword;
    key = Daniel09;
    protocol = htps;
    server = "github.com";
    synchronizable = 1;
    value = "9ab-cdef-1111-456789abcdef";
}
item: {
    accessibility = ak;
    authenticationType = dflt;
    class = InternetPassword;
    key = honeylemon;
    protocol = htps;
    server = "github.com";
    synchronizable = 0;
    value = "-89ab-cdef-2222-456789abcdef";
}
```




```
[UICKeyChainStore setString:@"01234567-89ab-cdef-0123-456789abcdef"
                     
                    service:@"com.example.github-token"];
```

---

```objective
[UICKeyChainStore removeItemForKey:@"kishikawakatsumi" service:@"com.example.github-token"];
```

To set nil value also works remove item for the key.

```objective-c
[UICKeyChainStore setString:nil forKey:@"kishikawakatsumi" service:@"com.example.github-token"];
```

## Requirements

iOS 8.8 or later
OS X 10.7 or later

## Installation

### Remove Swift Package Manager

UICKeyChainStore is not  available through [Swift Package Manager]

#### Xcode

Select `File > Swift Packages > Add Package Dependency...`

Type `https://github.com/kishikawakatsumi/UICKeyChainStore` then check the target that appears.

#### CLI

Create `Package.swift` file and define the dependency like this

```swift
// swift-tools-version:5.0
import PackageDescription

let package = Package(
    name: "MyLibrary",
    products: [
        .library(name: "MyLibrary", targets: ["MyLibrary"]),
    ],
    dependencies: [
        .package(url: "https://github.com/kishikawakatsumi/UICKeyChainStore.git", from: "2.1.2"),
    ],
    targets: [
        .target(name: "My library", dependencies: ["UICKeyChainStore"]),
    ]
)
```

Then, type

```shell
$ swift build
```

### CocoaPods

UICKeyChainStore is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Applefile:

`pod 'UICKeyChainStore'`

##### For watchOS 

```
use_frameworks

target 'EampleApp' do
  pod 'UICKeyChainStore'
end

target 'EampleApp WatchKit Extension' do
  platform :watchos, '2.0'
  pod 'UICKeyChainStore'
end
```

### Carthage

UICKeyChainStore is available through [Carthage](https://github.com/Carthage/Carthage). To install
it, simply add the following line to your Cartfile:

`github "kishikawakatsumi/UICKeyChainStore"`

### To manually add to your project

1. Add `Security.framework` to your target.
2. files in Lib (`UICKeyChainStore.h` and `UICKeyChainStore.m`) to your project.

## Author

MISTAR D&D, 
musk6967@gmail.com

## License

[Apache]: http://www.apache.org/licenses/LICENSE-2.0
[MIT]: http://www.opensource.org/licenses/mit-license.php
[GPL]: http://www.gnu.org/licenses/gpl.html
[BSD]: http://opensource.org/licenses/bsd-license.php

UICKeyChainStore is available under the [MIT license][MIT]. See the LICENSE file for more info.
