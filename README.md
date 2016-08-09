# My Interview Questions:

1. [栈内存与堆内存。](https://github.com/Coder-ZJQ/Interview-Questions#栈内存与堆内存。)
2. [@property 、 @synthesize 与 @dynamic 的作用。](https://github.com/Coder-ZJQ/Interview-Questions#@property 、 @synthesize 与 @dynamic 的作用。)
3. [@property 有哪些关键字？](https://github.com/Coder-ZJQ/Interview-Questions#@property 有哪些关键字？)
4. [\#define 与 const 的区别。](https://github.com/Coder-ZJQ/Interview-Questions#\#define 与 const 的区别。)
5. [int 、NSInteger 、NSNumber之间的区别。](https://github.com/Coder-ZJQ/Interview-Questions#int 、NSInteger 、NSNumber之间的区别。)
6. [\#import "" 、\#import <> 、\#include 、@class 的区别。](https://github.com/Coder-ZJQ/Interview-Questions#\#import "" 、\#import <> 、\#include 、@class 的区别。)
7. [protocol 、category 、extension 的使用场景与区别。](https://github.com/Coder-ZJQ/Interview-Questions#protocol 、category 、extension 的使用场景与区别。)
8. [Objective-C 中有哪几种数据存储方式。](https://github.com/Coder-ZJQ/Interview-Questions#Objective-C 中有哪几种数据存储方式。)
9. [id 与 instancetype 的区别。](https://github.com/Coder-ZJQ/Interview-Questions#id 与 instancetype 的区别。)
10. [NSNotification 、delegate 、KVO 的应用场景与区别。](https://github.com/Coder-ZJQ/Interview-Questions#NSNotification 、delegate 、KVO 的应用场景与区别。)
11. [block 的使用注意点。](https://github.com/Coder-ZJQ/Interview-Questions#block 的使用注意点。)
12. [应用的上架流程。](https://github.com/Coder-ZJQ/Interview-Questions#应用的上架流程。)
13. [什么是企业发布。](https://github.com/Coder-ZJQ/Interview-Questions#什么是企业发布。)
14. [客户端下的缓存机制。](https://github.com/Coder-ZJQ/Interview-Questions#客户端下的缓存机制。)
15. [静态库与动态库的区别。](https://github.com/Coder-ZJQ/Interview-Questions#静态库与动态库的区别。)
16. [如何给应用添加字体。](https://github.com/Coder-ZJQ/Interview-Questions#如何给应用添加字体。)
17. [单例的实现。](https://github.com/Coder-ZJQ/Interview-Questions#单例的实现。)
18. [nil 、Nil 、NULL 、NSNull 的区别。](https://github.com/Coder-ZJQ/Interview-Questions#nil 、Nil 、NULL 、NSNull 的区别。)
19. [Foundation对象与Core Foundation对象有什么区别。](https://github.com/Coder-ZJQ/Interview-Questions#Foundation对象与Core Foundation对象有什么区别。)
20. [以下代码的打印结果是什么？为什么？](https://github.com/Coder-ZJQ/Interview-Questions#以下代码的打印结果是什么？为什么？)     
``` objc
@interface JQApple : JQFruit
@end

@implementation JQApple
- (instancetype)init{
    self = [super init];
    if (self) {
        NSLog(@"%@", NSStringFromClass([self class]));
        NSLog(@"%@", NSStringFromClass([super class]));
    }
    return self;
}
@end
```


# My Answers:
#### 1. 栈内存与堆内存。
- **栈内存(stack)**：由系统自动分配，一般存放函数的参数值、局部变量等。由编译器自动创建与释放，创建与释放的方式类似于数据结构中的栈，先创建的后释放，后创建的先释放。
- **堆内存(heap)**：一般由程序员自主创建，最终也由程序员释放，若是创建了没释放，则会产生`memory leak`。一般用于存放全局变量或静态变量等。
#### 2. @property 、 @synthesize 与 @dynamic 的作用。
- **@property**: 用于声明成员变量的 getter/setter 方法，控制成员变量的访问权限，控制多线程时成员变量的访问环境。
- **@synthesize**: 与 @property 配套使用。@synthesize 会自动生成一个`_`开头的成员变量（若是不另外指定的话），并实现 @property 声明的 getter&setter 方法。
- **@dynamic**: 不会自动生成成员变量，程序员需自己添加成员变量并实现 getter/setter 方法。    
具体细节详见以下代码：
``` objc
#import <Foundation/Foundation.h>

@interface JQPerson : NSObject
{
    //  3.1 由于 @dynamic 并不会自动生成成员变量，因此需自主添加成员变量用于 getter/setter 方法，否则会报 “Use undeclared identifier” 错误。
    NSInteger _weight;
}

//  1. @property 的简单使用
@property (nonatomic, copy) NSString *name;
@property (nonatomic, assign) NSInteger age;
@property (nonatomic, assign) NSInteger height;
@property (nonatomic, assign) NSInteger weight;

@end

@implementation JQPerson

//  2. @synthesize 的使用
//  2.1 默认生成"_"开头的成员变量， 即：
@synthesize name = _name;
//  2.2 生成与 @property 相同的不带下划线的成员变量：
@synthesize age;
//  2.3 指定生成的成员变量名：
@synthesize height = H;

//  3. @dynamic 的使用
@dynamic weight;
//  3.2 实现 getter/setter 方法（这里只是简单的实现）：
- (void)setWeight:(NSInteger)weight {
    _weight = weight;
}
- (NSInteger)weight {
    return _weight;
}

@end
```
***（Tips：在都没有使用 @synthesize 以及 @dynamic 时，默认为 `@synthesize propertyName = _propertyName;`。但若是同时实现了 getter&setter 方法，则隐含表示为 `@dynamic propertyName;` 因此编译器并不会自动生成成员变量，此时若是使用成员变量则会出现 “Use undeclared identifier” 错误。解决方法可以在类的声明中自主添加私有的成员变量，或者使用 @synthesize，告知编译器自动生成成员变量。）***
#### 3. @property 有哪些关键字？
`@property`的关键字主要分为以下几类：   

- **原子性**（`nonatomic/atomic`）：
    - **atomic**: atomic 是线程安全的，但是执行效率不高。使用 atomic 关键字，编译器会自动为属性的 getter&setter 方法中加入一些互斥加锁代码，以保证在多个线程的情况下，不会出现一条线程调用 setter 方法但未执行完之前，另一条线程也调用 setter 方法，保证数据的有效性。也正是因为加锁的原因，会消耗一定的性能，影响效率。
    - **nonatomic**: nonatomic 不是线程安全的，如果对象无需考虑多线程的情况，可以使用 nonatomic 关键字，编译器便不会生成多余的加锁代码，可以提高执行效率。开发中一般常用 nonatomic 关键字。
- **读写权限**（`readonly/readwrite`）：
    - **readonly**: 只读关键字，只可调用对象的 getter 方法。
    - **readwrite**: 可读可写关键字，对象的 getter&setter 方法均可调用。
- **内存管理语义**（`assign/strong/weak/unsafe_unretained/copy/retain`）：
    - **assign**: 常用于基本数据类型（CGFloat、NSInteger等）。用于直接赋值，不必考虑内存管理，因为基本数据类型一般是分配在栈上的，而栈的内存会由系统自动处理。
    - **strong**: 强引用，引用存在对象即存在。
    - **weak**: 弱引用，只要对象没有强引用指向便会被释放，且 dealloc 会将指针置为 nil。
    - **unsafe_unretained**: 与 assign 及 weak 相似，但 unsafe_unretained 一般用于对象，且被释放后指针并不会被置为 nil，比较少用。
    - **copy**: 与 retain 相似，成员变量的 setter 方法会先 release 旧值， 再 copy 新值，retainCount 为 1。一般用于 NSString、block 等。内容拷贝。
    - **retain**: 在非ARC环境中，成员变量的 setter 方法会先 release 旧值，再 retain 新值，retainCount + 1。指针拷贝。
- **`getter&setter`方法名**：   
自定义属性的 getter&setter 方法名，一般常用于 `BOOL` 类型属性的 getter 方法，如：`@property (getter=isOpen)BOOL *open;`。
- **可否为空`Nullability Annotation`**（`nullable/nonnull/null_resettable/null_unspecified`）：为了与 swift 混编，适配 swift 中的 optional 可选类型而新增的关键字，比较少用到。
#### 4. \#define 与 const 的区别。
- **\#define**: 
- **const**: 
#### 5. int 、NSInteger 、NSNumber之间的区别。

#### 6. \#import "" 、\#import <> 、\#include 、@class 的区别。

#### 7. protocol 、category 、extension 的使用场景与区别。
#### 8. Objective-C 中有哪几种数据存储方式。
#### 9. id 与 instancetype 的区别。
#### 10. NSNotification 、delegate 、KVO 的应用场景与区别。
#### 11. block 的使用注意点。
#### 12. 应用的上架流程。
#### 13. 什么是企业发布。
#### 14. 客户端下的缓存机制。
#### 15. 静态库与动态库的区别。
#### 16. 如何给应用添加字体。
#### 17. 单例的实现。
#### 18. nil 、Nil 、NULL 、NSNull 的区别。
#### 19. Foundation对象与Core Foundation对象有什么区别。
#### 20. 以下代码的打印结果是什么？为什么？   
``` objc
@interface JQApple : JQFruit
@end

@implementation JQApple
- (instancetype)init{
    self = [super init];
    if (self) {
        NSLog(@"%@", NSStringFromClass([self class]));
        NSLog(@"%@", NSStringFromClass([super class]));
    }
    return self;
}
@end
```
均都打印 “JQApple”。   
因为 `self` 是类的隐藏参数，指向当前调用方法的对象。而 `super` 并不是如我们所想是指向当前对象父类的指针。其实 `super` 是一个编译器标识符，在运行时中与 `self` 相同，指向同一个消息接受者，只是 `self` 会优先在当前类的 `methodLists` 中查找方法，而 `super` 则是优先从父类中查找。验证如下：   
在终端运行：
> $ clang -rewrite-objc main.m

可以看到运行时代码如下：

``` objc
NSLog((NSString *)&__NSConstantStringImpl__var_folders_ld_03322m393b5cyvhz2zhv2c100000gn_T_main_97554f_mi_0, NSStringFromClass(((Class (*)(id, SEL))(void *)objc_msgSend)((id)self, sel_registerName("class"))));
NSLog((NSString *)&__NSConstantStringImpl__var_folders_ld_03322m393b5cyvhz2zhv2c100000gn_T_main_97554f_mi_1, NSStringFromClass(((Class (*)(__rw_objc_super *, SEL))(void *)objc_msgSendSuper)((__rw_objc_super){(id)self, (id)class_getSuperclass(objc_getClass("JQApple"))}, sel_registerName("class"))));
```

删除相关无关类型及方法后代码如下：
``` objc
objc_msgSend((id)self, sel_registerName("class"));
objc_msgSendSuper((__rw_objc_super){(id)self, (id)class_getSuperclass(objc_getClass("JQApple"))}, sel_registerName("class"));
```
查看函数定义：
``` objc
/** 
 * Sends a message with a simple return value to an instance of a class.
 * 
 * @param self A pointer to the instance of the class that is to receive the message.
 * @param op The selector of the method that handles the message.
 * @param ... 
 *   A variable argument list containing the arguments to the method.
 * 
 * @return The return value of the method.
 * 
 * @note When it encounters a method call, the compiler generates a call to one of the
 *  functions \c objc_msgSend, \c objc_msgSend_stret, \c objc_msgSendSuper, or \c objc_msgSendSuper_stret.
 *  Messages sent to an object’s superclass (using the \c super keyword) are sent using \c objc_msgSendSuper; 
 *  other messages are sent using \c objc_msgSend. Methods that have data structures as return values
 *  are sent using \c objc_msgSendSuper_stret and \c objc_msgSend_stret.
 */
OBJC_EXPORT id objc_msgSend(id self, SEL op, ...)
    __OSX_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);
/** 
 * Sends a message with a simple return value to the superclass of an instance of a class.
 * 
 * @param super A pointer to an \c objc_super data structure. Pass values identifying the
 *  context the message was sent to, including the instance of the class that is to receive the
 *  message and the superclass at which to start searching for the method implementation.
 * @param op A pointer of type SEL. Pass the selector of the method that will handle the message.
 * @param ...
 *   A variable argument list containing the arguments to the method.
 * 
 * @return The return value of the method identified by \e op.
 * 
 * @see objc_msgSend
 */
OBJC_EXPORT id objc_msgSendSuper(struct objc_super *super, SEL op, ...)
    __OSX_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);
```
可知 `objc_msgSend` 函数中的 `self` 参数是指指向接收消息的类的实例的指针，即消息接受者，而 `op` 参数则是指处理该消息的 `selector` ；`objc_msgSendSuper` 函数中的参数 `super` 则是一个 `objc_super` 结构体，`objc_super` 结构体定义如下：
``` objc
/// Specifies the superclass of an instance. 
struct objc_super {
    /// Specifies an instance of a class.
    __unsafe_unretained id receiver;

    /// Specifies the particular superclass of the instance to message. 
    __unsafe_unretained Class super_class;

    /* super_class is the first class to search */
};
```
其中 `receiver` 是指类的实例， `super_class` 则是指该实例的父类。可以看到在编译后的 `C++` 代码中有个 `__rw_objc_super` 结构体：
``` objc
struct __rw_objc_super { 
    struct objc_object *object; 
    struct objc_object *superClass; 
    __rw_objc_super(struct objc_object *o, struct objc_object *s) : object(o), superClass(s) {} 
};
``` 
其实即为 `objc_super` 结构体。通过 `(__rw_objc_super){(id)self, (id)class_getSuperclass(objc_getClass("JQApple"))}` 该段代码可知：我们把 `self` 以及 `JQApple` 的父类通过结构体的构造方法构造了一个 `__rw_objc_super` 结构体，也就是 `objc_super` 。因此 `objc_super` 结构体中的 `receiver` 既是 `self` 。所以 `[self class]` 和 `[super class]` 指向的是同一个消息接受者，只是 `self` 会优先从当前类的实现中寻找方法处理消息，而 `super` 则是会优先从 `objc_super` 结构体中的 `super_class` 也就是父类的实现中查找。 `JQFruit` 及 `JQApple` 中均未实现 `- (Class)class;` 方法，因此会逐级向上查找最终调用基类 `NSObject`的`- (Class)class;` 方法，通过官方开源的 `NSObject` 的 `- (Class)class;` 方法代码：
``` objc
- (Class)class{
    return object_getClass(self);
}
```
可知，消息接受者是 `self` ，而 `[self class]` 和 `[super class]` 指向的是同一个消息接受者，因此该段代码均打印 `JQApple`。

---
# to be continued...
