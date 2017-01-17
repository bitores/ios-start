
# ios-start

<p text-align="center"><img src="a.png" alt=""></p>
<p text-align="center"><img src="b.png" alt=""></p>



> 安装插件

```
sudo curl -fsSL https://raw.githubusercontent.com/supermarin/Alcatraz/deploy/Scripts/install.sh | sh  ---install plugin
rm -rf ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin ---uninstall plugin
rm -rf ~/Library/Application\ Support/Alcatraz ---remove all cache data

```

> 建议

```
1. 如果只是单纯的private变量，最好声明在implementation里. 
2. 如果是类的public属性，就用property写在.h文件里 
3. 如果自己内部需要setter和getter来实现一些东西，就在.m文件的类目里用property来声明 
4. .h中的interface的大括号{}之间的实例变量，.m中可以直接使用； 
5. .h中的property变量，.m中需要使用self.propertyVariable的方式使用propertyVariable变量
6. .h中声明的属性和成员变量均可以在子类中访问到.而.m则不可
7. 开发中(习惯),一般在成员变量前面加个_. 

eg.
ViewController是SubController的父类.
在ViewController.h中声明了成员变量imageView1和属性imageView2(@property) 
在ViewController.m中声明了成员变量imageView3和属性imageView4(@property). 

在ViewController.m中
1.通过self.xxx的方法可以出现imageView2和imageView4(属性) 
2.通过    _XXX的形式只能出现imageView2,imageView4(属性) 
3.通过    XXX的形式只能出现imageView1.imageView3(成员变量) 

在子类SubController.m中
1.通过self.XXX的形式只能出现imageView2(属性) 
2.通过 _XXX的形式什么都不会出现. 
3.通过XXX的形式可以出现imageView1(成员变量) 

总之,就是
在.h中声明的属性或者成员变量在其子类中均可以访问到,只不过形式不一样.
在.m中声明的属性或者成员变量只能在本类中访问到.
而属性其实就是成员变量的简写,内部自动包含了getter和setter方法.
但是
属性 在本类中通过 self.XXX 或 _XXX 访问，但子类中只能通过 self.XXX 访问，可见 _XXX局部性
成员变量通过 XXX 访问，OC的成员变量默认是protected的
```


> 常见错误

```
1、只有类的声明，没有类的实现
2、漏了@end
3、@interface和@implementation嵌套
4、两个类的声明嵌套
5、成员变量没有写在括号里面
6、方法的声明写在了大括号里面
```

> 语法细节

```
1、成员变量不能在｛｝中进行初始化，不能被直接拿出去访问
2、方法不能当做函数一样调用
4、成员变量／方法不能用static等关键字修饰，别跟C语言混在一起（暂时忽略）
5、类的实现可用写在main函数的后面，主要在声明后面就行了
```

> OC中方法与 函数

```
1、OC方法只能声明在@interface和@end之间，只能实现在@implementation和@end之间。也就是说OC方法不能独立于类村
2、C函数不属于类，跟类没有联系，C函数只归定义函数的文件所有
3、C函数不能访问OC对象的成员
4、低级错误：方法有声明，但是实现的时候写成了函数
```

> 可选参数

```
- (NSString *)makeMilk:(NSString *)milk fruit:(NSString *)fruit food:(NSString *)food, ...;
{
    NSMutableArray *arr = [[NSMutableArray alloc] init];
    va_list params;  //定义一个指向个数可变的参数列表指针；
    id argument;
    if (food) {
        //使参数列表指针arg_ptr指向函数参数列表中的第一个可选参数，说明：argN是位于第一个可选参数之前的固定参数，
        （或者说，最后一个固定参数；…之前的一个参数），函数参数列表中参数在内存中的顺序与函数声明时的顺序是一致的。
        如果有一va函数的声明是void va_test(char a, char b, char c, …)，则它的固定参数依次是a,b,c，最后一个固定参数argN为c，
        因此就是va_start(arg_ptr, c)。
        va_start(params, food);
        while ((argument = va_arg(params, id))) {//返回参数列表中指针arg_ptr所指的参数，
        返回类型为type，并使指针arg_ptr指向参数列表中下一个参数
            [arr addObject:argument];
        }
        va_end(params);//释放列表指针
    }
    return [NSString stringWithFormat:@%@_%@_%@,milk,fruit,[arr componentsJoinedByString:@_]];
}

使用：
Man *man = [[Man alloc] init];
[man makeMilk:@马奶 fruit:@苹果 food:@鱼儿,@肉儿,@鸡儿,@鸭儿,@鹅儿,nil];


关于 va_list va_start va_arg va_end 几个宏 

　　va_list params; //定义一个指向个数可变的参数列表指针;  
　　va_start(params,actionNum);//va_start 得到第一个可变参数地址,  
　　va_arg(params,id);//指向下一个参数地址  
　　va_end(params); //置空指针
```

> 只有实现没有声明

```
1、没有@interface，只有@implementation，也是能成功定义一个类的
@implementation Car : NSObject
{
    @public
    int wheels;
    int speed;
}
-(void) run
{
    NSLog(@"%i个轮子，%i时速的车子跑起来了", wheels, speed);
}
@end
 
2、@imlementation中不能声明和@interface一样的成员变量

```

> OC中的命名规则

```
类名：大驼峰（首字母大写）
方法名：小驼峰（首字母小写）
```

> 类目与延展

```
类目：可以扩展原有类的方法；不可以可以添加实例变量，属性。
延展可以看作是匿名类目，形式和类目相同，不用新创建文件
延展定义要在类本身@implementation 代码区域中进行实现
延展：可以添加类的私有方法、私有变量、属性
```

> [#import 作用]()

```
2、拷贝
1、oc.h文件中一般没有 c/c++中的去重定义 #ifdefined OC_H
2、头文件中尽量少引入其他头文件（使用@class），但也有必须要引用头文件的情况

1、该类若继承了某个超类，则必须引入定义那个超类的头文件。
2、该类若遵循某协议，也要引入该协议对应的头文件
```


> extern（代表：可能在别的源文件里定义）

```
extern工作原理:先在当前文件查找有没有全局变量，没有找到，才会去其他文件查找。

1、在 .h 和 .m 同时声明一个变量时，虽然写的时候不报错，但是编译时会报错
eg.
.h
NSString *const myTest;

.m
NSString *const myTest = @".m";
error: duplicate symbol for architecture x86_64

解决：
在 .h 中加上 extern
.h
extern NSString *const myTest;


2、

@interface ExternModel : NSObject
  	//  这个是不合法的：因为OC的类会将这个全局变量当做成员属性来处理，
  	//  而成员属性是需要加{}的，所以不合法
    NSString *lhString;//声明全局变量的时候默认带有extern，这里必须显式声明
@end

改为

@interface ExternModel : NSObject
	// 这里由于带有extern所以会被认为是全局变量
    extern NSString *lhString;
@end

-----
.h extern 声明了全局变量,则 .m 必须实现

@implementation ExternModel
    NSString *lhString=@"hello";
@end


其它文件中访问类的全局变量，可以不用导入 .h 文件，通过 extern 直接访问


@implementation ViewController

- (void)viewDidLoad {
      [super viewDidLoad];
       extern NSString *lhString;
       NSLog(@"%@",lhString);
}

3、相比于 #define 定义的宏变量 来说，extern 是定义全局变量的最佳实践
而且，extern 变量可以进行指针比较操作

[myURL isEqualToString:kInitURL];
```

> 静态局部变量

``` 
（非静态）全局变量的作用域是整个源程序，静态全局变量的作用域是定义它的那个源文件，
静态局部变量的作用域是定义它的那个方法。
```
> oc类中的成员变量与属性变量区别


#####1、成员变量（无需与外界接触的变量）
```
@interface FirstClass : NSObject
{
    //类的成员变量，默认访问权限为protect
    int m;
    double n;
    char c;
    float f;
}

成员变量在{}中定义，一旦声明后可以在本类的实现文件.m文件中直接使用，相当于这个类里面的全局变量
```

#####2、属性变量（用于与其他对象交互的变量）
```
@property(参数)类型 变量名；
@property(nonatomic,strong);//非基本类型用strong
@property(nonatomic,assign);//基本类型assign

在声明部分使用@property定义属性，
```

> @property ()里面的属性

```
访问控制： readwrite      readonly
内存管理：assign 基本       retain 对象  copy 实现NScopying
线程安全：atomic     nonatomic 非原子性
```

####[全局变量可否定义在头文件(.h)中](http://www.jianshu.com/p/d4f294f681b7)

```
全局变量作用域:从定义变量的那一行直到文件结束

一般不管是常量还是外部全局变量定义在源文件中的，那为什么不把它们定义在头文件中呢？
这是我在学习oc时比较纠结的问题，当我把一个常量定义在头文件中时，这时候报的"重复定义的变量"的错
误给了我答案:
因为外部全局变量肯定必须是整个工程唯一的，import的作用是把头文件中的内容进行拷贝，若有多个文件
import了一个定义了外部全局变量的头文件，那在整个工程中就会出现多个同名同类型的外部全局变量，原
来如此啊，豁然开朗。

但是把静态变量定义在头文件中，多个文件import这个头文件不会报“重复定义变量”的问题，这是因为静态
变量是内部全局，只在一个文件内有效。但是也不能因为没有错误就把认为可以把静态变量定义在头文件中
，这种做法是不推荐的，既然定义了一个静态变量那肯定是要在方法内使用它的，只有在源文件中才会有方
法的实现，所以静态变量也要定义在源文件中
```

> 引用外部变量

```
引用时 加上 extern 声明，声明为此为 外部变量 拓展了作用域
外部变量：1、在其它文件定义的 2、在同一文件，不同区块定义

eg.
	extern 外部变量 新名称

eg.

	static int counter = 0； //定义静态变量counter

	@implementation Person

	extern int counter;     //声明需要使用的静态变量

静态变量定义在方法或函数体外

	该方式适用于当静态变量会被本文件内的多个函数使用的情况，最好能把静态变量定义
	统一放在源文件的起始处(@implementation的外面),这样有利于代码维护和可读性，
	比如：

	Vars.m 文件
	//定义2个静态变量
	static int count;
	static int a;
	@implementation ClassName

	@end

```

> 数组

```
1、可变数组是不可变数组的子类
2、数组中元素类型可不一致，类js

```

> 字符串

```
可变字符串是不可变字符串的子类
```


> 关键字

```
 关键字：NS 、self 、super 、alloc 、init、 release、 @property 、@property(strong/remain/copy/weak/assign、setter/getter、nonatomic/atomic、readonly/readwrite)
```


> 函数

```
1、OC 中 所有成员方法都是 虚函数
2、函数的参数中不可以设置默认值
```

> 不能被继承的类 类似 final 类

```
NSString 、 NSArray 、NSDictionary 这三个类不能被继承
```


> 协议

```
1、oc无多继承，但可通过其可实现多继承
```


> 类目（分类、类别）

```
使用：扩展别人的类（可替代子类）

1、不能用来添加成员变量、只能添加方法
2、可替代子类
3、同一类放在多个文件中，便于协作
4、可以被继承
```


> 类延展属性的访问权限

```
作用：定义私有属性或方法

1、oc中属性没有 public private protected关键字
2、其访问权限是根据其写的位置确定
3、写在类的延展中是 private
4、普通类在 .h 文件中声明的属性或方法为 public 
```

> 特殊数据类型

```
一些特殊的数据类型 id(任何数据类型)、nil(空的对象)、Nil(空的类)、SEL() ，IMP(函数指针)
```

> id

```
oc中的id是一种数据类型，可以存放任何数据的对象，万能指针 id==NSObject*
```

> SEL 类型

```
SEL是一个类型，用SEL声明的一个变量，里面装的是消息
SEL s = @selector(methodName)
```

> block

```
block：块语法，本质上是匿名函数。与函数指针很相似：int（*）（int x，int y）
block定义： int (^myblock)(int) = ^(int num){return num * 7;};
解释：myblock为block变量，block类型：int(^)(int),block的值：^int (int num){return num * 7;}
形式：^返回值类型（参数列表）｛函数体｝，其中返回值类型可以省略
```

> property

```

1、atomic 加上线程安全锁（多线程），nonatomic 不会加线程安全锁（非多线程）

2、assign: 对基础数据类型 （NSInteger，CGFloat）和C数据类型（int, float, double, char, 等等）

2.1、retain： 对其他NSObject和其子类

2.3、copy： 对NSString

3、readonly，readwrite ：用于访问权限控制

4、setter=setter方法名字，getter=getter方法名字， 自定义方法名的

5、strong与retain功能相似；weak与assign相似，只是当对象消失后weak会自动把指针变为nil

注：参数作用是-自动生成私有属性或其它属性的setter和getter方法的声明和实现

```

> 资源列表

+ [OC的一些基本声明方法整理](http://www.jianshu.com/p/51662cd3a6cc)

+ [IOS 精讲-类目、延展、协议、属性、内存管理、Block](https://www.chuanke.com/v4702151-153167-581927.html)

+ [oc block使用](http://www.th7.cn/Program/IOS/201502/397959.shtml)

+ [IOS感应器](http://www.jianshu.com/p/233be81b8ead)

+ [IOS中权限](http://blog.csdn.net/Arnly/article/details/52904638)

+ [快速确定 OC中@property 中的参数](http://bbs.itheima.com/thread-330152-1-1.html)

+ [ios约束布局](http://blog.csdn.net/pucker/article/details/41843511)

+ [ios AutoLayout 详解](http://www.cocoachina.com/ios/20151020/13825.html)

+ [为iPhone 6设计自适应布局](http://www.cocoachina.com/ios/20141020/9978.html)

+ [iOS 开发----Storyboard/xib 连线问题](https://my.oschina.net/u/2458687/blog/505839)

+ [IOS开发基础常用控件简介 ](http://www.open-open.com/solution/view/1422433408017/)

+ [IOS 控件之基础整理 ](http://blog.csdn.net/edgargwjchangemyself/article/details/45678353)

+ [ IOS视图之基础整理 ](http://blog.csdn.net/edgargwjchangemyself/article/details/45678545)

+ [IOS之基本UI控件 ](http://www.2cto.com/kf/201502/376744.html)

+ [iOS基本UI控件总结 ](http://www.tuicool.com/articles/qmMjQnJ)

+ [苹果AppStore被拒理由大全](https://github.com/jcccn/Why-Reject)

+ [IOS 开源列表](http://www.cnblogs.com/oc-bowen/p/5590825.html)

+ [iOS 当前各版本所占比例](https://david-smith.org/iosversionstats/)

+ [IOS 百度传课](http://www.chuanke.com/course/_ios_____2.html?page=2)

+ [IOS多线程](http://www.chuanke.com/v4242218-178896-935590.html)）

+ [iOS 打测试包流程 在fir上发布](https://www.oschina.net/code/snippet_2248391_51855)

+ [Pod使用，Pod安装慢解决、Podfile 被安装后，使用 xcworkspace 打开项目](http://blog.csdn.net/eqera/article/details/39312125)

+ [iOS开发中的crash文件及重新符号化的问题](http://www.jianshu.com/p/390bc4998b3e)