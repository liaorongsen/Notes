####   Android开发之版本统一

1. comileSdkVersion:

表示：编译版本，就是运行我们这个项目的需要的SDK

2. buildToolsVersion

表示：构建工具的版本，其中包括了打包工具aapt、dx等等。


build target并不存在于manifest文件中，而是存在于项目根目录中的project.properties文件中.
property.properties如果指定的target=android-20的话, 就会让IDE去本地(/home/wangxin/tool/android-sdk-linux/platforms/android-20/android.jar)load对应的android.jar, 并把这个android.jar加到工程项目中参与编译. 我们代码中对framework的所有API调用都要到这个android.jar中去找, 找不到就报编译错误.
用intellij idea建立的工程有个特殊点， 在我们自己PC上load哪个android.jar参与编译， 由Project Settings -> Modules -> Dependencies -> Module SDK决定. 修改property.properties没有作用， 这是由于property.properties只在服务器打包apk时，参与编译， 它决定了服务器去哪个本地路径 load对应的android.jar参与编译，生成对市场发布的apk. 作为一个良好的使用习惯，每个开发人员在自己PC上的Project Settings对API level的设置应该和property.properties中的设置保持一致.

3.minSdkVersion

minsdkversion这个值是给lint做静态代码检查用的, 代码编译成功进入打包阶段, lint load minsdkversion指定的framework(android.jar)检查它是否涵盖了在应用代码调用的所有framework API. 如果发现有某个API的调用没在这个android.jar中, 就会报lint错误. 禁止lint对某段代码报错, 就在代码前加上@targetAPI(value), 这样lint对这段代码检查时就用value对应的android.jar去查是否涵盖了这段代码对framework的所有api调用, 有的话就不会再报错了.
这就要求@targetAPI()和运行时的版本检查要配套, 不然打包成功了, 但是真机上的android.jar如果不包含某个版本新添加的API, 那么运行时就会报错说找不到某个方法, 直接crash.下面是典型的用法:
@TargetApi(20)
    public void text(){
        if(Build.VERSION.SDK_INT >= 20){

                 // 使用api11 新加 api的方法

            }
        else {
        // 低版本的折衷处理方法
        }
    }


4.TargetVersion

targetSdkVersion既不参与编译环节也不参与lint代码检查环节，targetSdkVersion 是传给framework用的, 跟编译是否报错没关系.
framework中的类会调用context.getApplicationInfo().targetSdkVersion去获取应用在AndroidManifest.xml中设置的值. 根据不同的值设置不同的表现形式， 比如DatePickerDialog在构造的时候，根据不同的targetSdkVersion设置不同的UI主题.targetSdkVersion这个属性是在程序运行时期起作用的.
下面这个帖子给了很精彩的说明， “A concept can be better delivered with examples, always”. 也充分说明了阅读android framework源码对于app开发的重要性.
http://stackoverflow.com/questions/4568267/android-min-sdk-version-vs-target-sdk-version


******************************************************************************************************

如果开发的应用用户较多，那么必须保证应用在多个版本不同的设备上能够正确的运行。这就要求对各个版本比较熟悉，知道在什么版本中加入了什么新的功能或特性。但是Android的版本太多了，是个令人头疼的问题。如果想了解Android的版本差异，建议读一下Android开发者文档上相关的章节。

为了让你的应用程序指定可以运行的版本，Android的manifest文件中提供了<uses-sdk>标签。该标签中有三个属性，分别是minSdkVersion，targetSdkVersion，maxSdkVersion。这三个属性比较容易让人迷惑，我也是仔细读了谷歌的官方文档，才弄清楚这三个属性的意义。此外，在项目构建时，还有个概念叫build target，在本文中也会进行分析。

什么是API level

其实标签<uses-sdk>中指定的并不是我们使用的sdk的版本，也不是Android系统的版本，而是我们使用的Android平台的版本，即API level。API level是一个整数，它指的是我们使用的框架（Framework）的版本，也就是我们使用的sdk中的各个平台下的android.jar。但是这个API level又和Android系统的版本有着对应关系，并且每个系统都会在内部记录它所使用的API level。举例来说，我使用的手机系统是Android 2.3.3， 那么它就会在内部记录使用的API level为10。这个内部的API level可以让系统判定能不能安装一款app，这个问题会在下文中提及。
android的系统版本和API level之间并不是一一对应的，比如Android 2.3， Android 2.3.1， Android 2.3.2对应API level 9， 而Android 2.3.3， Android 2.3.4对应API level 10。API level是Android向开发者提供的一套Framework（android.jar）的代号，可能发布了新的系统版本，但是这一套接口并没有变化，所以就不必提供新的Framework开发包，所以API level也不必改变。由此可知Android系统版本和API level是多对一的关系。由于API level就是发布的android.jar（一套接口）的代号，所以API level和sdk中platforms目录中的各个android.jar是一一对应的。说白了，Android系统版本是给Android用户看的，而API level是给应用程序开发者看的。



























