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

