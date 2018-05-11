### 第一章
1.mian函数是入口整个工程的入口函数，一个工程仅有一个main函数
2.NSLog ===== System.out.println
3.每个字符串都是以"@"开头
4.占位符的使用 
```Objective-C
		int num = 10;
		float pi = 3.14;
		BOOL flag = true;
		char mychar = 'd';
		NSLog(@"整数的占位符为i：%i",num);
		NSLog(@"浮点类型的占位符为f：%.2f",pi);
		NSLog(@"布尔的占位符为d：%d",flag);
		NSLog(@"字符类型的占位符为c：%c",mychar);
		NSLog(@"请输入整型的数字:");
```
5.oc中的输入使用scanf，仅能输入int，float,char,BOOL类型
```Objective-C
		int mynum = o;
		scanf("%i",&mynum);
		NSLog(@"获得输入的数字：%i",mynum)
```
如果想输入字符串信息，需要如下代码：<br/>
```Objective-C
		char word[40];
		scanf("%s",word);
		NSString *info = [NSString stringWithCString:word encoding:NSUTF8StringEncoding];
		NSLog(@"%@",info)

```
6.import与include的区别<br/>
import会检查是否有重复的库，如果有，则忽略<br/>
include则不会<br/>
import""与<>的区别<br/>
""：用于引入内部的第三方类或类库<br/>
<>:用于引用系统类或类库<br/>
###第二章 指针及引用
1.什么是指针<br/>
在内存中，任何一个变量都可以看成由三部分组成，变量名，值，以及存放它的地址<br/>
如果有一个变量，它的值存储着另一个变量的地址值，则可以吧它看作指针<br/>
例如：声明指针变量
```Objective-C
		int *var;//声明指针变量
		int a = 10;//int变量
		var = &a //a变量的引用，即是a变量的地址，那么p的值就是a的地址
		NSLog(@"a变量的地址为;%p",&a);//打印a的地址：0x7fff5fbff83c
		NSLog(@"var指针变量的值为：%p"，var)//0x7fff5fbff83c
		NSLog(@"*var的值与a的值相同:%i",*var)//10
```
那么，指针的指针变量：<br/>
```Objective-C
		int **mypoint;
		mypoint = &var;
		NSLog(@"**mypoint的值与a的值相同，%i",**mypoint);
```
###第三章类的基础定义以及自定方法
定义一个类，是由两个部分文件组成：.h和.m<br/>
.h文件是头文件，定义变量和声明方法。例如<br/>
```Objective-C
		@interface Teacher:NSObject
		{
			//定义变量区
			int tid;
			NSString *tName;
		}
		/**
		* 方法的定义
		*/
		-(void)setTid:(int)tid;
		-(void)setTName:(NSString *)tName;
		-(int) getTid;
		-(NSString *)getTName;
		@end
```
@interface定义类的关键字<br/>
NSObject是一切类的基类<br/>
在.h文件中声明的变量基本上都是公有变量，也可以用@private声明为私有变量。但是在苹果的代码规范中很少使用@privare声明。由于OC的动态消息传递机制，OC中不存在真正意义上的私有方法，如果你不在.h文件中声明，只在.m文件中实现，或在.m文件的Class Extension里声明，那么基本上和私有方法差不多。公有的放到.h文件，私有的放到.m文件，import时只import.h文件。
Teacher.m
```Objective-C
#import "Teacher.h"
@implementation Teacher
- (void) setTid:(int)parTid
{
    self.tid = parTid;
}

- (NSString *) getTName
{
    return tName;
}

- (int) getTid
{
    return tid;
}

- (void) setTName:(NSString *)parTName
{
    self.tName = parTName;
}

- (void)test
{
    NSLog(@"私有方法");
}
@end
```
import 引用.h文件
@implementation实现.h文件中的所有方法
test方法，私有方法
Stydent.h
```Objective-C
@interface Student:NSObject
@property(nonatomic,assign) int stiID;
@property(nonatomic,strong) NSString *stuName;
+(Student *)instance;
-(id) initWithStuId:(int) stuId andStuName:(NSString *)stuNAme;
@end
```
@property 用于定义可自动生成的getter和setter变量<br/>
####对于@property属性的详解
使用@property定义property时可以在@property与类型之间用括号添加一些额外的指示符，常用的指示符由assign，atomic，copy，retain，strong，weak。<br/>
	-assign：该指示符对属性只是简单的复制，不更改引用计数。常用语NSInteger等OC的基础类型，以及Short，inyt，double，结构体等C数据类型，因为这些类型不存在被内存回收的问题。<br/>
	-atomic，nonatomic：指定setter和getter是否是原子操作，即是否线程安全，如果是atomic，那么存取方法都是线程安全的，即某一线程访问存活着取方法，其他线程不可以进入该存取方法。nonatomic则不具备线程安全的功能，需要指出的是，atomic是默认值，可以保证数据的完整性，但是相应的降低了性能，所以在单线程环境中建议使用nonatomic来提升性能。<br/>
	-copy：如果使用copy指示符，当点用setter方法对成员变量赋值时，会将被复制的对象复制一个副本，再将该副本给成员变量，相应的原先的被赋值对象的引用计数加1.当成员变量的类型是可变类型，或其子类是可变类型，被赋值的对象在赋值后有可能再被修改，如果不需要这种修改，则可以考虑copy指示符。<br/>
	-getter，setter。用于为getter方法，setter方法指定自定义方法名，如getter=myName，setter=setName：；setter方法后面有一个（：），这是因为我们需要在后面添加参数。
	——readonly，readwrite：readonly指示系统只合成getter方法，不合成setter方法。readwrite是默认值，指示系统需要合成setter和getter方法。<br/>
	-strong，weak。Strong指示符该属性对被赋值对象持有强引用，二weak指示符指定该属性对被赋值对象持有弱引用。强引用的意思是：只要该强引用指向被赋值的对象，那么该对象就不会自动回收，弱引用的意思是，即是该弱引用指向被赋值的对象，该对象也可能被回收吗，如果不希望对象被回收，可以使用strong指示符。如果需要保证程序性能，避免内存溢出，可以使用weak，内存一旦被回收，指针会被赋值为 inil。<br/>
	-unsafe_unretained：与weak不同，被unsafe_unretained指针所引用的对象被回收后，unsaff_unretained指针不会被赋值为nil，可能会导致程序出错。<br/>
总结一下：<br/>
	-strong和原来的retain比较相似，strong的property将对应_strong的指针，它将持有所指向的对象。<br/>
	-weak不持有所指向的对象，而且当所指对象销毁时能将自己设置为nil，基本所有的outlet都应该用weak。<br/>
	-unsafe_unretained就是原来的assign，当需要支持IOS4时需要用到这个关键字。
	copy和原来基本一样，copy一个对象并且为其创建一个strong指针。<br/>
	-assign对于对象来说应该永远不用assign，实在需要的话该用unsafe_unretained代替。但是对于基本类型比如int float BOOL double这样的还是要用assign<br/>
基本数据类型，如：int，float，BOOL，double等，使用assign<br/>
对象一般使用Strong，在非acr环境下，使用retain。<br/>
所有UI界面的对象使用weak，例如outlet对象。<br/>
####OC中方法的定义
1.定义结构<br/>
StudentService.h<br/>
```Objective-C
@interface StudentService:NSObject
-(Bool)loginfromForStuName:(NSString *)stuName andStuPwd:(NSString *) stuPwd;
@end
```
2.自定义构造方法<br/>
```Objective-C
-（id） initWithStuId:(int) stuId andStuName:(NSString *) stuName{
	//调用弗雷德构造方法
	self = [super init];
	id(self!=nil){
		_stuID = stuId;
		_stuName = stuName;
	}
	return self;
}
```
3.静态方法<br/>
```Objective-C
//静态方法
+(Student *)instance{
	return [[Student alloc]init];
}
```
####使用类（调用方法）
使用类/调用构造方法，使用alloc实例化类，而尽量不要用new<br/>
```Objective-C
Student *stu = [[Student alloc]init];
Student *stu2 = [[Student alloc]initWithStuId:1 andStuName:@"KKKK"];
```
调用自定义方法<br/>
```Objective-C
StudentService *myService = [[StudentService alloc]init];
Bool isLogin = [myService loginfromForStuName:@"xxx" andStuPwd@"ddqdq"];
```
super相当于java中的super，self相当于java的this<br/>
调用静态方法
```Objective-C
Student *stu = [Student instance];
```
使用类的属性<br/>
```Objective-C
stu.stuID=1;
stu.stuName = @"zzzzz";
```
总结：调用类属性使用点语法（.）调用类方法使用[]，实例化类使用alloc<br/>
####常用字符串操作函数
常见的字符串创建<br/>
```Objective-C
NSString *str = @"sdsadasd"
```
指针字符类型的转换为字符串<br/>
```Objective-C
char *text = "蝴蝶结卡的空间啊薮";
str = [NSString stringWithUTF8String:text];
```
将URL请求响应转换为字符串
```Objective-C
NSURL *url = [NSURL URLWithString:@"http://www.weather.com.cn/adat/sk/101010100.html"];
NSString *strText = [NSString stringWithContentsOfURL:url encoding:NSUTF8StringEncoding error:nil];
```
将字符串转换为全大写字母/全部小写/首字母大写<br/>
```Objective-C
//转换成大写
NSLog(@"大写：%@"，[str uppercaseString]);
//转换成小写
NSLog(@"小写：%@",[str lowercaseString]);
//转换成首字母大写
NSLog(@"首字母大写：%@",[str capitalizedString]);
```
判断两个字符串是否相等<br/>
```Objective-C
BOOL result = [@"abc" isEqualToString:@"ABC"];
```
字符串链接<br/>
```Objective-C
NSString *str2 = [str stringByAppendingString:@"Shangdong"];
NSString *str3 = [NSString stringWithFormat:@"城市信息为：%@市%@省"，str，str2];

```
字符串截取	
```Objective-C
NSString *str = @"123456789";
NSLog(@"%@",[str substringFromIndex:3]);
NSLog(@"%@",[str substringToIndex;3]);
NSRange range = NSMakeRange(4,5);
NSLog(@"%@",[str substringWithRange:range]);
NSString *str2 = @"1,2,3-4-5-6,7,8,9";
NSArray *array = [str2 componentsSeparatedByString:@"-"];
for(NSString *var in arrat){
	NSLog(@"%@",var);
}
NSString *str3 = [array objectAtIndex:0];
NSLog(@"%@",str3);
```
字符串转换
```Objective-C
NSString *str = @"123";
int a = [str intvalue];
NSLog(@"%i",a);
float b = [str floatValue];
NSLog(@"%.2f",b);
NSString *str = @"划分空间啊函数";
NSLog(@"%zi".[str length]);
unichar temp = [@"dhjkashdjka" characterAtIndex:3];
NSLog(@"%c",temp);
//字符串转字符
const char *s = [@"dasdas" UTF8String];
NSLog(@"%s",s);
```
#### 数组
创建数组<br/>
```Objective-C
//创建数组两种方式
NSArray *array = @[@"one",@"two",@"three"];
NSArray *array2 = nil;
//nil只是一个结束标识，而不是其中一个元素
array2 = [NSArray arrarWithObjects;@"one",@"two",@"three",nil];
```
取元素（查）<br/>
```Objective-c
NSLog(@"%@",array[2]);
NSLog(@"%@",[array objectAtIndex:1]);
NSLog(@"%@",[array firstObject]);
NSLog(@"%@",[array lastObject]);
//从数组中提取一个子数组
NSArray *array5 = [array5subarratWithRange:NSMakeRange(2,2)];
//containsObject 可以用来判断数组是否包含一个指定元素
if ([array4 containsObject:@"one"]){
	NSLog(@"array4 包含one")；
}
//查出一个元素在数组中的索引位置
NSLog(@"three 在array4中的索引位置 %lu",[array4 indexofObject:@"three"]);

```
新增元素生成新的数组<br/>
```Objective-C
//arrayByAddingObject给一个数组对象嫁一个元素生成一个新的数组
NSArray *array1 = [array arrayByAddingObject:@"four"];
//arrayByAddingObjectsFromArray给一个数组对象加一个数组生成一个新的数组
NSArray *array2 = [array arrarByAddingObjectsFromArray:array1];
```
获取大小<br/>
```Objective-C
NSLog(@"%lu",[array count]);
```
循环<br/>
```Objective-C
for(NSString *var in array){
	NSLog(@"%@",var);
}
```
其他<br/>
```Objective-C
NSString *str1 = @"one:twe:dwd:dwf";
//用一个固定的分隔符把一个字符串隔开创建一个数组
NSArray *array4 = [str1 componentsSeparatedByString:@":"];
```
可变数组<br/>
创建<br/>
```Objective-C
//创建一个可变数组
NSMutableArray *mtArray2 = [NSMutableArray arraWithCapacity:50];
NSMutableArray *mtArray1 = [[NSMutableArray alloc]init];
```
添加元素（增）<br/>
```Objective-C
//往可变数组添加一个新的元素
[mtArray1 addObject:@"one"];
NSObject *newobj1 = [NSObject new];
[mtArray1 addObject:newobj1];

```
获取元素(查)<br/>
```Objective-C
NSLog(@"%@",array[2]);
NSLog(@"%@",[array objectAtIndex:1]);
```
修改元素（改）<br/>
```Objective-C
//替换掉指定索引位置的元素
[mtArray1 replaceObjectIndex:2 withObject:@"sasas"];
//在指定的索引位置插入一个新的元素
[mtArray1 insertObject:@"one4" atIndex:2];
```
删除元素（删除）<br/>
```Objective-C
//从一个可变数组中移除一个元素
[mtArray1 removeObject:@"one2"];
[mtArray1 removeObjectAtIndex:2];
```
####字典
不可变字典<br/>
创建<br/>
```Objective-C
//value-key的赋值，最后一个必须是nil
NSDictionary *dictionary = [NSDictionary dictionaryWithObjectsAndKeys:@"zzzzz"，@"name",@"12331313131",@"number",nil];
//初始化一个元素
NSDictionary *dict1 = [NSDictionary dictionaryWithObject:@"1" forkey:@"a"];
//初始化，利用数组
NSDictionary *dict2 = [NSDictionary dictionaryWithObject:[NSArray arrayWithObjects:@"1",@"2",@"3",nil] forKeys:[NSArray arrayWithObjects@"a",@"b",@"c",nil]];
```
数量<br/>
```Objective-C
int count = [dict2 count];
```
得到字典中所有的key/value值<br/>
```Objective-C
NSArray *allkeys = [dict2 allkeys];
NSArray *allvalues = [dict2 allvalues];
```
获取key对应的value<br/>
```Objective-C
NSString *value = [dict2 valueForKey:@"b"];
NSString *value2 = [dict2 objectForKey:@"b"];
```
获得keys对应的values(数组)<br/>
```Objective-C
NSArray *strarray = [dict2 objectsForKeys:[NSArray arrayWithObjects:@"a",@"d",@"c",nil] notFoundMarker:@"not found"];
```
遍历之for循环<br/>
变量名必须为key<br/>
```Objective-C
for(NSString *key in dict2){
	NSLog(@"%@ = %@",key,[dict2 objectsForKeys:key]);
}
```
遍历之枚举<br/>
```Objective-C
NSEnumrator *en = [dict2 keyEnumerator];
id keyvalue = nil;
while(keyvalue = [en nextObject]){
	NSLog(@"key:%@-value:%@",keyvalue,[dict2 valueForKey:keyvalue]);
}

```
可变字典<br/>
创建<br/>
```Objective-C
NSMutableDictionarray *mtdict = [[NSMutableDictionarray alloc]init];
```
添加元素（增）<br/>
```Objective-C
[mtdict setObject:@"1" forkey:@"a"];
```
删除元素（删）<br/>
```Objective-C
[mtdict removeObjectForKey:@"a"];
```
获取key对应value（查）<br/>
```objective-C
NSString *value = [mtdict valueForKey:@"b"];
NSString *value2 = [mtdict objectsForKeys:@"b"];
```
修改元素值<br/>
```Objective-C
// 原来有key=a的值，再次设置就想当于修改
[mtdict setObject:@"ffff" forkey:@"a"];
```
####OC协议
OC协议==java中的接口<br/>
协议常用ui开发中事件设计模式，即委托设计模式的实现<br/>
@protocol为协议定义关键字，通常协议只是定义方法（函数），所以，只有.h文件<br/>
与java区别，在OC中协议常用分为两块，@required（必须，要求）和@optional(可选)<br/>
@required关键字下定义的方法，实现协议的类，必须实现该方法，默认的方法都是@required<br/>
@optional可选关键字，可以选择性实现和不实现<br/>
例如：<br/>
```Objective-C
@protocol Emp<NSObject>
@required
-(void) work;
-（void） gongzi;
@optional
-(void) dakai;
@end
```
在OC中实现一个协议的方法：<br/>
```Objective-C
#import "Emp.h"

@interface SEEmp : NSObject<Emp>
-(void) goutong;
@end
```
说明：<br/>
1.利用<>引入协议名称，如果是实现多个协议用逗号分隔(,)。<br/>
2.需要实现协议中的方法，不用在.h文件中再次定义。（goutong是扩展方法）；<br/>
协议的实例化方式：<br/>
1.实例化协议的子类<br/>
```Objective-C
SEEmp *se = [[SEEmp alloc]init];
```
2.多态方式的实例化<br/>
```Objective-C
id<Emp> se = [[SEEmp alloc]init];
```
3.类型强制转换<br/>
```Objective-C
（SEEmp *）se
```
4.基于协议的多态
```Objective-C
-(void) tinghuibao:(id<Emp>)emp;
```

git测试修改

