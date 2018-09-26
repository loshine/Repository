# Proguard

## 介绍

我们通常说的`proguard`包括四个功能：

* shrinker（压缩）：检测并移除没有用到的类，变量，方法和属性；
* optimizer（优化）：优化代码，非入口节点类会加上`private/static/final`, 没有用到的参数会被删除，一些方法可能会变成内联代码。
* obfuscator（混淆）：使用短又没有语义的名字重命名非入口类的类名，变量名，方法名。入口类的名字保持不变
* preverifier（预校验）：预校验代码是否符合 Java1.6 或者更高的规范(唯一一个与入口类不相关的步骤)

![proguard](https://i.imgur.com/hwuiIIB.png)

## 配置

### Keep配置

#### keep

`-keep [,modifier, ...] class_specification`

指定类和类的成员变量是入口节点，保护它们不被移除混淆。

例如，对一个可执行jar包来说，需要保护 main 的入口类；对一个类库来说需要保护它的所有 public 元素。例子：

```txt
-injars       myapplication.jar
-outjars      myapplication_out.jar
-libraryjars  <java.home>/lib/rt.jar
-printmapping myapplication.map

-keep public class mypackage.MyMain {
    public static void main(java.lang.String[]);
}
```

#### keepclassmembers

`-keepclassmembers [,modifier] class_specification`

保护的指定的成员变量不被移除、优化、混淆。例如，保护所有序列化的类的成员变量。

```txt
-keepnames class * implements java.io.Serializable
# 这指定了继承Serizalizable的类的如下成员不被移除混淆
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    !static !transient <fields>;
    !private <fields>;
    !private <methods>;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}
```

#### keepclasseswithmembers

`-keepclasseswithmembers [,modifier,...] class_specification`

拥有指定成员的类将被保护，根据类成员确定一些将要被保护的类。例如保护所有含有main方法的类。

```txt
# 这种情况下含有main方法的类和main方法都不会被混淆。
-injars      in.jar
-outjars     out.jar
-libraryjars <java.home>/lib/rt.jar
-printseeds

-keepclasseswithmembers public class * {
    public static void main(java.lang.String[]);
}
```

#### keepnames

`-keepnames class_specification`是`-keep,allowshrinking class_pecification`的简写

指定一些类名受到保护，前提是他们在 shrink 这一阶段没有被去掉。也就是说没有被入口节点直接或间接引用的类还是会被删除。仅在 obfuscate 阶段有效。

#### keepclassmembernames

`-keepclassmembernames class_specification`是`-keepclasseswithmembers,allowshrinking class_specification`的简写
与`-keepclassmembers`相似。保护指定的类成员，前提是这些成员在 shrink 阶段没有被删除。仅在 obfuscate 阶段有效。

#### keepclasseswithmembernames

`-keepclasseswithmembernames class_specification`是`-keepclasseswithmembers,allowshrinking class_specification`的简写
与`-keepclasseswithmembers`类似。保护指定的类，如果它们没有在 shrink 阶段被删除。仅在 obfuscate 阶段有效。

### 压缩配置

#### dontshrink

`-dontshrink`
声明不压缩输入文件。默认情况下，除了 -keep 相关配置指定的类，所有其它没有被引用到的类都会被删除。每次 optimizate 操作之后，也会执行一次压缩操作，因为每次 optimizate 操作可能删除一部分不再需要的类。

### 优化配置

#### dontoptimize

`-dontoptimize`

声明不优化输入文件。默认情况下，优化选项是开启的，并且所有的优化都是在字节码层进行的。

#### optimizations

`-optimizations optimization_filter`

更加细粒度地声明优化开启或者关闭。只在 optimize 这一阶段有效。这个选项的使用难度较高。

#### optimizationpasses

`-optimizationpasses n`
指定执行几次优化，默认情况下，只执行一次优化。执行多次优化可以提高优化的效果，但是，如果执行过一次优化之后没有效果，就会停止优化，剩下的设置次数不再执行。这个选项只在 optimize 阶段有效

#### assumenosideeffects

`-assumenosideeffects class_specification`

指定一些方法被删除也没有影响（尽管这些方法可能有返回值），在 optimize 阶段，如果确定这些方法的返回值没有使用，那么就会删除这些方法的调用。proguard 会自动的分析你的代码，但不会分析处理类库中的代码。
例子:

```txt
# 删除代码中Log相关的代码
-assumenosideeffects class android.util.Log {
    public static boolean isLoggable(java.lang.String, int);
    public static int v(...);
    public static int i(...);
    public static int w(...);
    public static int d(...);
    public static int e(...);
}
```

> 使用这条配置有点危险，如果删除了一些预料之外的代码，很容易就会导致代码崩溃。**所以，谨慎使用**

#### allowaccessmodification

`-allowaccessmodification`

这项配置是设置是否允许改变作用域的，使用这项配置之后可以提高优化的效果。但是，如果你的代码是一个库的话，最好不要配置这个选项，因为它可能会导致一些`private`变量被改成`public`。

#### mergeinterfacesaggressively

指定一些接口可能被合并，即使一些子类没有同时实现两个接口的方法。这种情况在 java 源码中是不允许存在的，但是在 java 字节码中是允许存在的。它的作用是通过合并接口减少类的数量，从而达到减少输出文件体积的效果。仅在 optimize 阶段有效。

> 这项配置对于一些虚拟机的65535方法数限制是有一定效果的。

### 混淆配置

#### dontobfuscate

`-dontobfuscate`

声明不混淆。默认情况下，混淆是开启的。除了 keep 配置中声明的类，其它的类或者类的成员混淆后会改成简短随机的名字。

#### printmapping

`-printmapping [filename]`

指定输出新旧元素名的对照表的文件。映射表会被输出到标准输出流或者是一个指定的文件。

> 这个文件在追踪异常的时候是有用的，在`{android_sdk_home}/tools/proguard/lib`目录下有一个`retrace.jar`文件。我们可以把混淆后的 Stack Trace 用这个工具处理一下，就会转变成容易阅读的类。所以，做 app 的同学每次发版本的时候都要把这个文件留下来，并标记清楚版本。这对线上版本的调试非常重要。

#### applymapping

`-applymapping filename`

指定重用一个已经写好了的map文件作为新旧元素名的映射。元素名已经存在在mapping文件中的元素，按照映射表重命名；没有存在到mapping文件的元素，重新赋一个新的名字。
mapping 文件可能引用到输入文件中的类和类库中的类。这里只允许设置一个 mapping 文件。仅在 obfuscate 阶段有效。

#### obfuscationdictionary

`-obfuscationdictionary filename`

指定一个文本文件用来生成混淆后的名字。默认情况下，混淆后的名字一般为a,b,c这种。
通过使用`-obfuscationdictionary`配置的字典文件，可以使用一些非英文字符做为类名、成员变量名、方法名。
最有用的做法一般是选择已经在类文件中存在的字符串做字典，这样可以稍微压缩包的体积。
一个字典的例子：

```txt
# 这里巧妙地使用java中的关键字作字典，混淆之后的代码更加不利于阅读
#
# This obfuscation dictionary contains reserved Java keywords. They can't
# be used in Java source files, but they can be used in compiled class files.
# Note that this hardly improves the obfuscation. Decent decompilers can
# automatically replace reserved keywords, and the effect can fairly simply be
# undone by obfuscating again with simpler names.
# Usage:
#     java -jar proguard.jar ..... -obfuscationdictionary keywords.txt
#
 
do
if
for
int
new
try
byte
case
char
else
goto
long
this
void
break
catch
class
const
final
float
short
super
throw
while
double
import
native
public
return
static
switch
throws
boolean
default
extends
finally
package
private
abstract
continue
strictfp
volatile
interface
protected
transient
implements
instanceof
synchronized
```

#### classobfuscationdictionary

`-classobfuscationdictionary filename`

指定一个混淆类名的字典，字典的格式与`-obfuscationdictionary`相同

#### packageobfuscationdictionary

`-packageobfuscationdictionary filename`
指定一个混淆包名的字典，字典格式与`-obfuscationdictionary`相同

#### useuniqueclassmembernames

`-useuniqueclassmembernames`

指定相同的混淆名对应相同的方法名，不同的混淆名对应不同的方法名。

#### dontusemixedcaseclassnames

`-dontusemixedcaseclassnames`

指定在混淆的时候不使用大小写混用的类名。

#### keeppackagenames

`-keeppackagenames [package_filter]`

声明不混淆指定的包名。 配置的过滤器是逗号隔开的一组包名。包名可以包含`?`,`*`通配符，并且可以在前面加`!`否定符。

#### flatternpackagehierarchy

`-flatternpackagehierarchy [packagename]`

所有重新命名的包都重新打包，并把所有的类移动到 packagename 包下面。如果没有指定 packagename 或者 packagename 为`""`，那么所有的类都会被移动到根目录下

#### repackageclasses

`-repackageclasses [package_name]`

所有重新命名过的类都重新打包，并把他们移动到指定的 packagename 目录下。如果没有指定 packagename，同样把他们放到根目录下面。
这项配置会覆盖`-flatternpackagehierarchy`的配置。它可以代码体积更小，并且更加难以理解。这个与废弃的配置`-defaultpackage`作用相同。

#### keepattributes

`-keepattributes [attribute_filter]`
指定受保护的属性，可以有一个或者多个`-keepattributes`配置项，每个配置项后面跟随的是 Java 虚拟机和 proguard 支持的 attribute，两个属性之间用逗号分隔。属性名中可以包含`*`,`**`,`?`等通配符，也可以加`!`将某个属性排除在外。
当混淆一个类库的时候，至少要保持`InnerClasses`,`Exceptions`,`Signature`属性。为了跟踪异常信息，需要保留`SourceFile`,`LineNumberTable`两个属性。如果代码中有用到注解，需要把`Annotion`的属性保留下来。
例子：

```txt
-keepattributes SourceFile, LineNumberTable
-keepattributes *Annotation*
-keepattributes EnclosingMethod

# 可以直接写在一行
-keepattributes Exceptions, InnerClasses, Signature, Deprecated,
                SourceFile, LineNumberTable, *Annotation*, EnclosingMethod
```

### 预校验配置

略

### 普通配置

#### verbose

`-verbose`

声明在处理过程中输出更多信息。添加这项配置之后，如果处理过程中出现异常，会输出整个 StackTrace 而不是一条简单的异常说明。

#### dontnote

`-dontnote [class_filter]`

声明不输出那些潜在的错误和缺失，比如说错别字或者重要的配置缺失。配置中的 class_filter 是一串正则表达式，混淆过程中不会输出被匹配到的类相关的内容。

#### dontwarn

`-dontwarn [class_filter]`

声明不输出那些未找到的引用和一些错误，但续混淆。配置中的 class_filter 是一串正则表达式，被匹配到的类名相关的警告都不会被输出出来。

#### ignorewarnings

`-ignorewarnings`

输出所有找不到引用和一些其它错误的警告，但是继续执行处理过程。不处理警告有些危险，所以在清楚配置的具体作用的时候再使用。

#### printconfiguration

`-printconfiguration [filename]`

输出整个处理过程中的所有配置参数，包括文件中的参数和命令行中的参数。可以不加文件名在标准输出流中输出，也可以指定文件名输出到文件中。它在调试的时候比较有用。

#### dump

`-dump [filename]`声明输出整个处理之后的jar文件的类结构，可以输出到标准输出流或者一个文件中。

## 过滤器

### File Filters

就像普通的匹配器一样，可以使用通配符来过滤文件名。

* `?` 代表文件名中的一个字符
* `*` 代表文件名中的一部分,不包括文件分隔符
* `**` 代表文件名中的一部分，包括文件分隔符
* `!` 放在文件名前面表示将某文件排除在外

## Keep配置的修饰符

### includedescriptorclasses

它是用来声明描述目标成员的元素也应当被保护。它在保护native方法时特别有效。因为它可以同时保证参数类型，返回类型不被混淆。保证最终的方法签名保持一致。

例子：

```txt
-keepclasseswithmembernames,includedescriptorclasses class * {
    native <methods>;
}
```

`-keepclasseswithmembernames`是保护符合条件的含有 native 方法的类。附加的`includedescriptorclasses`是保证参数和返回类型的类同样不被混淆。这样就可以做到这些类的方法签名与调试时完全一致。

### allowshrinking

修饰`-keep`, 声明一个元素可以被移除，即使它已经声明了被保护。意味着它有可能在压缩阶段被删除，但是它又是必须的入口，所以它有可能不参与优化和混淆阶段。

### allowoptimization

修饰`-keep`, 声明一个元素可以被优化，即使它已经声明被保护。这意味着该元素参与优化阶段，但是不参与压缩和混淆阶段。特殊用途的时候使用。

### allowobfuscation
 
与前几个类似，修饰`-keep`,只参与混淆阶段，但是不参与压缩和优化阶段。