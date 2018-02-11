title: 使用keyChain存储UUID

date: 2016-03-20 9:41:02

categories: iOS

tags:

- keyChain
- UUID

------

一、在你的工程里找到Capabilities，把里面的keyChainsharing设置为ON，如图所示：

![](https://s1.ax1x.com/2018/02/11/9G6fI0.jpg)

二、这样就会在左侧的目录帮我们自动生成Entitlements文件，如图所示：

![](https://s1.ax1x.com/2018/02/11/9G6IRU.jpg)

三、我们点开entitlements文件，发现里面的item和我们的Bundle Identifier，以及刚刚keychain Groups是一致的：

![](https://s1.ax1x.com/2018/02/11/9G6xJK.jpg)

四、用keyChain存储UUID，可以使用下面的类:

```
#import <Foundation/Foundation.h>

@interface KeyChainStore : NSObject

+ (void)save:(NSString *)service data:(id)data;
+ (id)load:(NSString *)service;
+ (void)deleteKeyData:(NSString *)service;

@end

```

```
#import "KeyChainStore.h"

@implementation KeyChainStore


+ (NSMutableDictionary *)getKeychainQuery:(NSString *)service {
    return [NSMutableDictionary dictionaryWithObjectsAndKeys:
            (id)kSecClassGenericPassword,(id)kSecClass,
            service, (id)kSecAttrService,
            service, (id)kSecAttrAccount,
            (id)kSecAttrAccessibleAfterFirstUnlock,(id)kSecAttrAccessible,
            nil];
}

+ (void)save:(NSString *)service data:(id)data {
    //Get search dictionary
    NSMutableDictionary *keychainQuery = [self getKeychainQuery:service];
    //Delete old item before add new item
    SecItemDelete((CFDictionaryRef)keychainQuery);
    //Add new object to search dictionary(Attention:the data format)
    [keychainQuery setObject:[NSKeyedArchiver archivedDataWithRootObject:data] forKey:(id)kSecValueData];
    //Add item to keychain with the search dictionary
    SecItemAdd((CFDictionaryRef)keychainQuery, NULL);
}

+ (id)load:(NSString *)service {
    id ret = nil;
    NSMutableDictionary *keychainQuery = [self getKeychainQuery:service];
    //Configure the search setting
    //Since in our simple case we are expecting only a single attribute to be returned (the password) we can set the attribute kSecReturnData to kCFBooleanTrue
    [keychainQuery setObject:(id)kCFBooleanTrue forKey:(id)kSecReturnData];
    [keychainQuery setObject:(id)kSecMatchLimitOne forKey:(id)kSecMatchLimit];
    CFDataRef keyData = NULL;
    if (SecItemCopyMatching((CFDictionaryRef)keychainQuery, (CFTypeRef *)&keyData) == noErr) {
        @try {
            ret = [NSKeyedUnarchiver unarchiveObjectWithData:(__bridge NSData *)keyData];
        } @catch (NSException *e) {
            NSLog(@"Unarchive of %@ failed: %@", service, e);
        } @finally {
        }
    }
    if (keyData)
        CFRelease(keyData);
        return ret;
}

+ (void)deleteKeyData:(NSString *)service {
    NSMutableDictionary *keychainQuery = [self getKeychainQuery:service];
    SecItemDelete((CFDictionaryRef)keychainQuery);
}

@end
```

五、存储并获得UUID

```
/**
 *  得到设备的UUID
 *
 *  @return 返回UUID
 */
+ (NSString *)getUUID{

    NSString * strUUID = (NSString *)[KeyChainStore load:KEY_APPID];
    if ([strUUID isEqualToString:@""] || !strUUID)
    {
        CFUUIDRef uuidRef = CFUUIDCreate(kCFAllocatorDefault);

        strUUID = (NSString *)CFBridgingRelease(CFUUIDCreateString (kCFAllocatorDefault,uuidRef));

        [KeyChainStore save:KEY_APPID data:strUUID];

    }

    return strUUID;

}

```

注意：这里的KEY_APPID是一个宏，里面值可以自己定义，建议用Bundle Identifier