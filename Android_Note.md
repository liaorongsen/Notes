#1 Note 
Gradle����

1 ���
��ƪ�ĵ��ǻ���0.9�汾��Gradle�����1.0��ǰ�İ汾���ڲ����ݣ����ܻ�������ͬ
1.1 �µĹ���ϵͳ��Ŀ��
�¹���ϵͳ��Ŀ���ǣ�
ʹ�ô������Դ�����ø��Ӽ�
ʹ�ô���ͬһӦ�ó���Ĳ�ͬ�汾�������ף������Ƕ��apk�汾����ͬһ�汾�Ķ��ֶ���
ʹ�����ã���չ���Զ��幹����������
���õ�IDE����
1.2 Ϊʲôʹ��Gradle
Gradle��һ���߼�����ϵͳ�͹������ߣ�����ͨ������Զ��幹���߼�
����һЩ����ʹ������ѡ��Gradle��
ʹ���ض���������(DSL)�������Ϳ��ƹ����߼�
�����ű�����Groovy���ԣ�����ͨ��DSL���Ԫ��������ͨ���������DSLԪ�أ��������Զ���Ĺ����߼�
֧��Maven��(����)Ivy��������
�ǳ�������ʹ�����ʵ������Ҳ��ǿ���Լ���ʵ�ַ�ʽ
����ܹ��ṩ�Լ���DSL��API�������ű�ʹ��
�ṩ����Ĺ���API�Թ�IDE����
2 ����Ҫ��
Gradle 1.10����1.11����1.12���Լ�����汾0.11.1
SDK�Լ�Buid Tools 19.0.0��ĳЩ���ܿ�����Ҫ���µİ汾
3 ������Ŀ
Gradle��Ŀͨ����Ŀ��Ŀ¼�µ� build.gradle �ļ���������������
3.1 �򵥵Ĺ����ļ�
��򵥵�Java��Ŀ�����ļ� build.gradle
apply plugin: 'java'
����ű�Ӧ����Gradle��Java��������������ṩ�����Ͳ���JavaӦ�õ����й���
��򵥵�Android��Ŀ�Ĺ����ļ������������ݣ�
buildscript {
    repositories {
        mavenCentral()
    }
 
    dependencies {
        classpath 'com.android.tools.build:gradle:0.11.1'
    }
}
 
apply plugin: 'android'
 
android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"
}    
Note(��ע): ���µ�android�������
apply plugin: 'com.android.application'
�����Android�����ű��������������Ҫ���ݣ�
buildscript { ... } �����������������̵Ĵ��롣����������У�������ʹ��Maven�ֿ⣬�Լ�һ��Maven�ļ�(artifact)������·��������ļ����ǰ�����Android Gradle����Ŀ⣬�汾Ϊ0.11.1
Ȼ��android�����Ӧ�ã���֮ǰ��Java���һ��
���android { ... } ������anroid����������Ҫ�Ĳ�������Ҳ��Adnroid DSL����ڡ�Ĭ�ϵ�����£�ֻ�б���Ŀ��SDK�汾���͹������߰汾�Ǳ���ġ��ڽű��У���Ӧ����compileSdkVersion��buildtoolsVersion���ԡ�compileSdkVersion�;ɱ���ϵͳ��project.properties�ļ��е�target���Զ�Ӧ�����������compileSdkVersion������һ��intֵ(API Level)����һ����֮ǰ��target����ֵһ�����ַ���
�ص�: ��Ӧ��ֻӦ��android�����ͬʱӦ��java����ᵼ�¹�������
ע��: ��ͬ����Ҫһ��local.properties�ļ���ָ��SDK��·������build.gradle��ͬһ·���£����ļ���ʹ��sdk.dir����ָ�������ߣ����������ANDROID_HOME����������������һ�µģ������ѡ��һ����ϲ���ķ�ʽ��
3.2 ��Ŀ�ṹ
ǰ���android�����ű�ʹ����Ĭ�ϵ��ļ���Ŀ¼�ṹ��Gradle��ѭԼ���������õ�ԭ���ڿ��ܵ�������ṩ�˺����Ĭ�����ò�����
��������Ŀ����������source sets�������main source code��test code��λ�����µ�Ŀ¼�У�
src/main/ src/androidTest/
����ЩĿ¼�У�������Ŀ¼��ӦԴ�����
������Java����Android�����Դ��Ŀ¼����ԴĿ¼������ ��
java/ resources/
����Android������������е��ļ���Ŀ¼
AndroidManifest.xml res/ assets/ aidl/ rs/ jni/
Note: src/androidTest/AndroidManifest.xml ���Ǳ���ģ����Զ���������
3.2.1 ������Ŀ�ṹ
��Ĭ����Ŀ�ṹ�����ʵ�ʱ�򣬿���������ĿĿ¼������Gradle�ĵ�������ͨ������Ľű���������Java��Ŀ��sourceSets��
sourceSets {
    main {
        java {
            srcDir 'src/java'
        }
        resources {
            srcDir 'src/resources'
        }
    }
}
Note: srcDir �����ָ����Ŀ¼��Դ�ļ�Ŀ¼�б���(����Gradele�ĵ���û���ἰ������ʵ������������)��
Ϊ���滻Ĭ�ϵ�Դ�ļ�Ŀ¼�б�����ʹ��srcDirs��ָ��Ŀ¼���顣��Ҳ��һ�ֲ�ͬ��ʹ�÷�ʽ��
sourceSets {
    main.java.srcDirs = ['src/java']
    main.resources.srcDirs = ['src/resources']
}
�������Ϣ���ο�Gradle�ĵ��е�Java�������
Android���ʹ�����Ƶ��﷨����������ʹ�����Լ���sourceSets����Ӧ��Ŀ¼��(build.gradle�ļ��е�)android������ָ��
������һ��ʾ������ʹ�þ���Ŀ��Դ��ṹ�����ҽ�androidTest sourceSetӳ�䵽testsĿ¼
android {
    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            
            java.srcDirs = ['src']
            
            resources.srcDirs = ['src']
            
            aidl.srcDirs = ['src']
            renderscript.srcDirs = ['src']
            
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
        }
 
        androidTest.setRoot('tests')
    }
}
Note: ���ھɵĽṹ�����е�Դ�ļ� (java, aidl, renderscript, and java��Դ�ļ�)����һ��Ŀ¼�������Ҫӳ����ЩsourceSet�����srcĿ¼��
Note: setRoot() ����������sourceSet(������Ŀ¼)ָ���µ�Ŀ¼���������棬��src/androidTest/* ָ���� tests/*
������Android���еģ����������Java sourceSets�о�û������
��migrated�� ʾ��(λ�ڱ�ҳ��ײ�)��չʾ���ⲿ������
3.3 ��������
3.3.1 ͨ������
��build�ļ���Ӧ��һ��������Զ�����һϵ�й�������Java�����Android�����������������Լ�����£�
assemble
�����Ŀ���
check
ִ�����м��
build
ִ��assemble��check����task�����й���
clean
������Ŀ���
����assemble, check �� build �������κ�ʵ�ʵ����顣����ֻ��ê������(anchor tasks)������������������ʵ��ִ��ʵ�ʲ���������
�����Ͳ���Ҫ������Ŀ��ʲô���ͣ�ʹ�õ���ʲô�����������ִ��ͬ��������
���磬ʹ��findbugs������ᴴ���µ����񣬲���check�����������ʹ��check������ʱ�������ͻᱻ���á�
���ն�(�����У�gradle��ĿĿ¼��)�����������������Բ�ѯ���߼��������
gradle tasks ��������������Կ���ȫ�����������������ϵ��
gradle tasks --all
Note: Gradle�Զ�����һ��������������������ļ����ٴ�ִ�й�������ʱ������ļ�û�иı䣬Gradle��ָ����������ΪUP-TO-DATE����ζ��������Ҫִ�С������Ļ������������ȷ�ػ����������������ᵼ�·Ǳ���Ĺ�������
3.3.2 Java��Ŀ������
Java�����Ҫ������������������������ê�������������ϵ
assemble
jar 
������񴴽����
check
test 
����������в���
jar����ֱ�ӻ��ӵ�����������������classes���񽫱���JavaԴ��
testClasses�������ڱ�����Եģ��������������ٱ����ã���Ϊtest������������(��������classes����һ��)
ͨ����˵����ֻ��Ҫ����assemble����check���񣬶�����Ҫ������������
�������Gradle Java����ĵ�����Java�����ȫ����������ǵ�����
3.3.3 Android����
Android���ʹ��ͬ����Լ�������ֺ���������ļ��ݣ���������˶����ê������
assemble
���������֯��Ŀ�����
check
�����Ŀ�������м��
connectedCheck
���м����Ҫһ�������ӵ��豸����ģ�������������������ӵ��豸���첽���С�
deviceCheck
ͨ��APIs����Զ���豸�����м�顣��ͨ����CI�����������С�
build
����assemble��check
clean
������Ŀ���
�µ�ê�������Ǳ���ģ��Ա�֤�ڲ���Ҫ�豸���ӵ�����������г����顣
��Ҫע����ǣ�build���񲢲�����deviceCheck����connectedCheck
һ��Android��Ŀ���������������debug APK �� release APK������ÿһ�������Լ���ê������������������ɶ����Ĺ�����
assemble
assembleDebug
assembleRelease
���Ƕ�����������������ɹ���һ��apk����Ҫ�Ķ�����衣assemble���������������������Ե���assemble����������APK��
Tip: Gradle֧������������ʹ��camel��ʽ����������д��
���磺
gradle aR��gradle assembleRelease��һ���ģ���Ϊû�б����������ͬ������д
ê������checkҲ���Լ���������
check
lint
connectedCheck
connectedAndroidTest
connectedUiAutomatorTest (not implemented yet)
deviceCheck
�����ڵ��������ʵ�ֲ�����չ��ʱ������������
���գ������Ϊ���й�������(debug, release, test)����install/uninstall�����������ļ����԰�װ�Ļ�(����ǩ��)��
3.4 �����Ĺ������̶���
Android����ṩ�˴���DSL��ֱ�Ӵӹ���ϵͳ�ж��ƴ�������顣
3.4.1 Manifest����
ͨ��DSL��������������manifest���ԣ�
minSdkVersion
targetSdkVersion
versionCode
versionName
applicationId (ʵ�ʵ�packageName �C ǰ�� ApplicationId versus PackageName �鿴����)
Package Name for the test application
Instrumentation test runner
���磺
android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"
 
    defaultConfig {
        versionCode 12
        versionName "2.0"
        minSdkVersion 16
        targetSdkVersion 16
    }
}
������λ��androidԪ���е�defaultConfigԪ���С�
֮ǰ�汾��Android Pluginʹ��packageName������manifest�ļ���packageName ���ԡ���0.11.1�汾��ʼ����Ӧ����build.gradle�ļ�ʹ��applicationId������manifest�ļ���packageName ���ԡ�
����Ϊ������AndroidӦ�õ�packageName(��ΪAndroidӦ�õ�ID)��java����֮������塣
�ڹ����ļ��ж����ǿ��֮�������������Ƕ�̬�ġ�
���磬���Դ�һ���ļ��ж�ȡ�汾���ƣ�����ʹ���Զ�����߼���
def computeVersionName() {
    ...
}
 
android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"
 
    defaultConfig {
        versionCode 12
        versionName computeVersionName()
        minSdkVersion 16
        targetSdkVersion 16
    }
}
Note: ��������Ҫ��ָ����Χ���Ѿ����ڵ�getter��������ͻ�����磬��defaultConfig { ...}�е���getVersionName()���Զ�ʹ��defaultConfig.getVersionName()�����������Զ��������getVersionName()��
�������û��ͨ��DSL���ã���ôĬ�ϵ�����ֵ�ᱻʹ�á�������Ĭ�ϵ�����ֵ�б�
Property Name	Default value in DSL object	Default value
versionCode	-1	value from manifest if present
versionName	null	value from manifest if present
minSdkVersion	-1	value from manifest if present
targetSdkVersion	-1	value from manifest if present
applicationId	null	value from manifest if present
testApplicationId	null	applicationId + ��.test��
testInstrumentationRunner	null	android.test.InstrumentationTestRunner
signingConfig	null	null
proguardFile	N/A (set only)	N/A (set only)
proguardFiles	N/A (set only)	N/A (set only)
������ڹ����ű���ʹ���Զ�����߼���ȡ��Щ���ԣ���ô�ڶ��е����Ծͺ���Ҫ�����磬���������д��
if (android.defaultConfig.testInstrumentationRunner == null) {
    // assign a better default...
}
������ֵ��null����ô�ڹ��������лᱻ�����е�Ĭ��ֵ���������DSLԪ�ز���������Ĭ��ֵ(�����е�ֵ)���������ѯ�������ֵ������Ϊ�˷�ֹ����Ӧ�õ�manifest�ļ���������ı�Ҫ��
3.4.2 ��������(Build Types)
Ĭ������£�Android������Զ�������Ŀͬʱ����debug��release�汾��Ӧ�ó���
�������汾�Ĳ�֮ͬ����Ҫ�����ܷ���һ����ȫ�豸�ϵ��Գ��򣬺�APK���ǩ����
debug�汾ʹ��һ���Զ���������Կ/֤�飬��ʹ����֪��name/password��ǩ��(��ֹ���������г���������ʾ)��release�汾�ڹ���������û��ǩ������Ҫ�Ժ�ǩ����
��Щ����ͨ��һ����������(BuildTpye)���������á�Ĭ������£�debug��release�������������Ͷ��ᱻ������
Android��������Զ���������ʵ����Ҳ�����������������͡���Щ����buildTypes��DSL���������ã�
android {
    buildTypes {
        debug {
            applicationIdSuffix ".debug"
        }
 
        jnidebug.initWith(buildTypes.debug)
        jnidebug {
            packageNameSuffix ".jnidebug"
            jniDebuggable true
        }
    }
}
����Ĵ���Ƭ����������¹��ܣ�
����Ĭ�ϵ�debug�������ͣ�
���������ó�<app appliationId>.debug���Ա���ͬһ�豸��ͬʱ��װdebug��release�汾
������һ���µ���Ϊjnidebug�Ĺ������ͣ���debug�������͵�һ������
����jnidebug�������ͣ��������JNI������������һ����ͬ�İ�����׺
����һ���µĹ������;�����buildTypes������ʹ��һ���µ�Ԫ��һ���򵥣�����ͨ������initWith()����ʹ�ñհ�������
�����ǿ����õ������Ժ����ǵ�Ĭ��ֵ��
Property name	Default values for debug	Default values for release / other
debuggable	true	false
jniDebuggable	false	false
renderscriptDebuggable	false	false
renderscriptOptimLevel	3	3
applicationIdSuffix	null	null
versionNameSuffix	null	null
signingConfig	android.signingConfigs.debug	null
zipAlignEnabled	false	true
minifyEnabled	false	false
proguardFile	N/A (set only)	N/A (set only)
proguardFiles	N/A (set only)	N/A (set only)
����������Щ���ԣ�Build Types������ͨ��Դ�����Դ��Ӱ�칹�����̡�
ÿһ���������Ͷ��ᴴ��һ��ƥ���sourceSet��Ĭ�ϵ�·��Ϊ��
src/<buildtypename>/
����ζ���µĹ������͵����ֲ�����main����androidTest(���ǲ��ǿ��Ҫ���)�����������͵����Ʊ�����Ψһ�ġ�
������sourceSetһ�����������͵�sourceSet�������±�����
android {
    sourceSets.jnidebug.setRoot('foo/jnidebug')
}
���⣬ÿһ��Build Type���ᴴ��һ��assemble<BuildTypeName>����
��ǰ�棬assembleDebug��assembleRelease�Ѿ��ᵽ���ˣ���������ǵ���Դ����debug��release�������ͱ�Ԥ������ʱ��������ص�����ͱ��Զ������ˣ�����assembleDebug��assembleRelease
�����build.gradleƬ��ͬ���ᴴ��assembleJnidebug����assemble��������assembleDebug��assembleRelease����һ������assembleJnidebug��
Tip: �������������������gradle aJ������assembleJnidebug����
�����õ��ĳ�����
ֻ��debugģʽ����Ҫ��Ȩ�ޣ���releaseģʽ����Ҫ
�Զ���debugʵ��
debugģʽʹ�ò�ͬ����Դ(���磬��Դȡֵ��ǩ��֤���)
BuildType��Դ�����Դͨ�����·�ʽʹ�ã�
manifest�ļ��ϲ���app��manifest�ļ���
Դ����Ϊ��һ��Դ��Ŀ¼
��Դ���ӵ�main����Դ�У�ȡ���Ѿ����ڵ�ֵ
3.4.3 ǩ������
��һ��Ӧ�ó���ǩ����Ҫ���£�
һ��keystore
һ��keystore����
һ��key�ı���
һ��key����
�洢����
λ�ã���������������ʹ洢����һ�����һ��ǩ������(SigningConfig)
Ĭ������£�debugǩ������ʹ��һ��debug keystore����֪���������֪�ı����Լ��������롣
debug keystoreλ��$HOME/.android/debug.keystore�����û�еĻ����Զ�����һ����
debug�������ͻ��Զ�ʹ��debug SigningConfig��
���Դ�������ǩ�����û����Զ���Ĭ���ڽ����á�ͨ��signingConfigs DSL����������
android {
    signingConfigs {
        debug {
            storeFile file("debug.keystore")
        }
 
        myConfig {
            storeFile file("other.keystore")
            storePassword "android"
            keyAlias "androiddebugkey"
            keyPassword "android"
        }
    }
 
    buildTypes {
        foo {
            debuggable true
            jniDebuggable true
            signingConfig signingConfigs.myConfig
        }
    }
}
�����Ƭ���޸�debug keystore��λ�õ���Ŀ��Ŀ¼�¡����Ӱ���κ�ʹ�����Ĺ������ͣ�����������У���Ӱ�����debug�������͡�
����Ҳ������һ���µ�ǩ�����ú�һ��ʹ�������ǩ�����õ��еĹ������͡�
Note: ֻ��Ĭ��·����debug keystore�����ڵ�ʱ��ᱻ�Զ��������ı�debug keystore��·���򲻻����µ�·�����Զ�����debug keystore������һ�����ֲ�ͬ��ǩ�����ã�����ʹ��Ĭ�ϵ�debug keystore·�������Զ�����debug keystore��Ҳ����˵���Ƿ��Զ�����debug keystore������keystore��λ�þ��������������õ����ơ�
Note: keystore��·��ͨ������Ŀ��Ŀ¼�����·��������Ҳ����ʹ�þ���·�������ܲ��Ƽ�����(debug keystore���⣬��Ϊ�����Զ�������)��
Note: �����Ҫ����Щ�ļ���ӵ��汾����ϵͳ�У�����ܲ��������д���ļ��С������Stack Overflow�����ṩ�˴Ӵӿ���̨���߻���������ȡ�ķ�����
http://stackoverflow.com/questions/18328730/how-to-create-a-release-signed-apk-file-using-gradle
�����Ժ�����ָ�ϣ��ṩ�����ϸ��
3.4.4 ����ProGuard
ProGuard��Gradle plugin for ProGuard 4.10��ʼ֧�ֵ�(since Gradle plugin 0.4)������������͵�minifyEnabled���Ա�����Ϊtrue����ôProgruard������Զ�����ӽ�������Ӧ������Ҳ�Զ���������
Note: ��Gradle����汾0.14.0��ʼBuildType.runProguard����ΪminifyEnabled���ԡ�������ο�Release notes
android {
    buildTypes {
        release {
            minifyEnabled true
            proguardFile getDefaultProguardFile('proguard-android.txt')
        }
    }
 
    productFlavors {
        flavor1 {
        }
        flavor2 {
            proguardFile 'some-other-rules.txt'
        }
    }
}
Variant��ʹ�����������Ĺ����ļ���������������Ӧ��Build Type��flavor�еġ�
SDK��������Ĭ�ϵĹ����ļ���
proguard-android.txt
proguard-android-optimize.txt
����λ��sdk·���£�ʹ��getDefaultProguardFile()���Ի�ȡ�ļ�������·�������ǳ����Ƿ�Ҫ�����Ż�֮�⣬����������ͬ�ġ�
3.4.5 ѹ����Դ�ļ�
����ʱ�����Զ��Ƴ�û�б�ʹ�õ���Դ�ļ���������ϸ��Ϣ��鿴�ĵ���Դ�ļ�ѹ��
4 ������ϵ��Android����Ŀ�Ͷ���Ŀ����
Gradle��Ŀ�������������������Щ����������ⲿ�����ư�����������Gradle��Ŀ��
4.1 ���������ư�
4.1.1 ���ذ�
Ϊ������һ���ⲿ��jar����������Ҫ��compile���������һ������
dependencies {
    compile files('libs/foo.jar')
}
 
android {
    ...
}
Note: dependenciesDSLԪ���Ǳ�׼Gradle API��һ���֣���������androidԪ�ء�
compile��������������mainӦ�õġ��κ���ӵ�����·���еĶ������ᱻ��������յ�apk�ļ��С�
����������һЩ���������ʱ�����õ������ã�
compile: ��module
androidTestCompile: ����module
debugCompile: debug�������͵ı���
releaseCompile: release�������͵ı���
��Ϊ����һ��apk��Ȼ��һ����صĹ������ͣ�����apkͨ�������������������ã�compile��<buildtype>Compile
����һ����������ʱ���Զ�����һ�����������ֵı�������<buildtype>Compile
������debug�汾����Ҫʹ��һ���Զ���⣨�����¼crash��Ϣ������release�汾����Ҫ��������������ͬһ����Ĳ�ͬ�汾��ʱ�򣬻�ǳ����á�
Ҳ����ͨ�����һ��Ŀ¼������Ŀ¼�µ�����jar�ļ��� compile fileTree(dir: 'libs', include: ['*.jar'])
4.1.2 Զ���ļ�
Gradle֧�ִ�Maven����Ivy�ֿ��ȡ�����ļ���
���ȣ�����Ѳֿ���ӵ��б��У���Σ����밴��Maven����Ivy���ļ������淶������������
repositories {
    mavenCentral()
}
 
 
dependencies {
    compile 'com.google.guava:guava:11.0.2'
}
 
android {
    ...
}
Note: mavenCentral()��ָ���ֿ�URL�ı�ݷ�ʽ��Gradle֧��Զ�̺ͱ��زֿ⡣
Note: Gradle��ѭ������ϵ�Ĵ����ԡ����һ���������ļ�Ҳ���������ļ�����Щ���������ļ�Ҳ�ᱻ��ȡ������
�������������������Ϣ����鿴Gradle�û�ָ�Ϻ�DSL�ĵ�
4.2 ����Ŀ����
Gradle��Ŀ����ͨ������Ŀ������������gradle��Ŀ��
һ������Ŀ����ͨ������������Ŀ��Ϊ��Ŀ¼����ָ������Ŀ��Ŀ¼�¡�
���磬��Ŀ�ṹ���£�
MyProject/
 + app/
 + libraries/
    + lib1/
    + lib2/
����������ṹ�ж���3����Ŀ��Gradle��ͨ�����������������ǣ�
:app
:libraries:lib1
:libraries:lib2
ÿ����Ŀ�����Լ���build.gradle�ļ������������������������⣬�ڸ�Ŀ¼�»���һ��settings.gradle�ļ������������е�����Ŀ��
Ŀ¼�ṹ���£�
MyProject/
 | settings.gradle
 + app/
    | build.gradle
 + libraries/
    + lib1/
       | build.gradle
    + lib2/
       | build.gradle
settings.gradle�ļ�������ʮ�ּ򵥣�
include ':app', ':libraries:lib1', ':libraries:lib2'
ָ���ĸ��ļ�����һ��ʵ�ʵ�Gradle��Ŀ��
:app��Ŀ����������������Ŀ����ô������ϵ�������£�
dependencies {
    compile project(':libraries:lib1')
}
������ڶ���Ŀ���õ���Ϣ�����
4.3 ����Ŀ
������Ķ���Ŀ�����У�:libraries:lib1��:libraries:lib2������Java��Ŀ��:appAndroid��Ŀ����ʹ�����������jar�ļ���
Ȼ�����������Ҫ����ʹ����Android API����Android��Դ�ļ��Ĵ���(�ڿ���Ŀ��ʹ����Android API��Android��Դ�ļ�)����Щ����Ŀ�Ͳ����ǳ����Java��Ŀ��������Android����Ŀ��
4.3.1 ����һ������Ŀ
һ������Ŀ�ͳ����Android��Ŀ�����ƣ�ֻ�к��ٵ�����
��Ϊ��������Ŀ�͹���Ӧ�ó���һ��������ʹ�ò�ͬ�Ĳ������������Ŀ�Ĳ���͹���Ӧ�ó���Ĳ�����ڲ�����󲿷ֵĴ��룬�������Ƕ�����com.android.tools.build.gradlejar���ṩ��
buildscript {
    repositories {
        mavenCentral()
    }
 
    dependencies {
        classpath 'com.android.tools.build:gradle:0.5.6'
    }
}
 
apply plugin: 'android-library'
 
android {
    compileSdkVersion 15
}
����һ��ʹ��API 15����Ŀ���Ŀ��SourceSets��������ϵ�Ĵ����Ӧ�ó�����Ŀ��һ�������Ҷ��Ʒ�ʽҲһ����
4.3.2 ��ͨ��Ŀ�Ϳ���Ŀ������
һ������Ŀ����Ҫ�����һ��.aar����������Android�Ĺ鵵�ļ���������������õ�Դ�루����jar�ļ����߱���.so�ļ����Լ���Դ�ļ���manifest, res, assets����
һ������ĿҲ��������һ������apk�����ԣ���������Ӧ�ó���
����ʹ��ͬ����ê������assembleDebug, assembleRelease�����������������й�������Ŀ����ͨ��Ŀû������
���ಿ�֣�����Ŀ��Ӧ�ó�����Ŀһ�������й������ͺ�product flavors���������ɶ���汾��aar��
Ҫע����ǣ�����Build Type���ò������ڿ���Ŀ��Ȼ��������Զ���sourceSet���ı�������������ݣ��������Ǳ���ͨ��Ŀʹ�û��Ǳ����ԡ�
4.3.3 ����һ������Ŀ
����һ������Ŀ������������Ŀ�ķ�ʽһ����
dependencies {
    compile project(':libraries:lib1')
    compile project(':libraries:lib2')
}
Note: ������ɶ������Ŀ����ô˳���Ǻ���Ҫ�ġ���;ɹ���ϵͳ�е�project.properties�ļ��е�����˳��һ����Ҫ��
4.3.4 ����Ŀ����
Ĭ�ϵ�����£�����Ŀֻ�ᷢ��release���ְ汾(release variant)������汾�ᱻ���������˿���Ŀ����Ŀʹ�ã����������Լ���������ʲô�汾������Gradle���µ����ƣ�������Ŭ������������ơ�
�������¿����ĸ��汾�ᱻ������
android {
    defaultPublishConfig "debug"
}
Ҫע����ǣ���������������Ʊ�����������variant���ƣ�release��debug����������ֻ����û��flavor��ʱ���ʹ�á������Ҫ����flavor��ʱ��ı�Ĭ�ϵķ����汾�����������д��
android {
    defaultPublishConfig "flavor1Debug"
}
��������Ŀ�����а汾Ҳ�ǿ��ܵġ����Ǽƻ�����ͨ����Ŀ������Ŀ�Ĺ���������������������������Gradle�����ƣ����ڻ�������ô��������Ҳ��Ŭ���޸�������⣩��
�������а汾�Ĺ���Ĭ��û�п������������£�
android {
    publishNonDefault true
}
������ʶ���������variant�汾��ζ�ŷ������aar�ļ�����������һ��aar�ļ��а����˶��variant�汾��ÿһ��aar�ļ�����һ��������variant��
����һ��variant�汾��ζ�Ź�������һ�����õ�aar�ļ�����ΪGradle��Ŀ������ļ�������ļ����Է�����maven�ֿ⣬������������Ŀ�����ÿ���Ŀʱ��Ϊ����Ŀ�ꡣ
Gradle��Ĭ���ļ��ĸ�����������ʹ����Ĭ���ļ���
compile project(':libraries:lib2')
Ϊ�����������ķ����汾�������ָ������ʹ����һ����
dependencies {
    flavor1Compile project(path: ':lib1', configuration: 'flavor1Release')
    flavor2Compile project(path: ':lib1', configuration: 'flavor2Release')
}
��Ҫ: Ҫע���ѷ�����������������variant�汾�������˹������ͣ�������õ�ʱ��Ҳ�����������ġ�
��Ҫ: ����������Ĭ�ϰ汾������Maven������������Щ����İ汾��Ϊ��չ������������������������ζ�Ų������������ݵط�����maven�ֿ⡣��Ӧ�÷���һ��������vatiant���ֿ⣬���߿�����������������֧�ֿ���Ŀ������
5 ����
����һ������Ӧ���Ѿ�������Ӧ����Ŀ�ڡ�����Ҫ�ٴ��������Ĳ�����Ŀ��
5.1 ��Ԫ����
�����Եĵ�Ԫ���Թ���֧���Ѿ����뵽1.1�У������뿴���ҳ�档�����������ֽ������ǡ�instrumentation tests��
5.2 ����������
����ǰ���ᵽ�ģ�������main sourceSet �ľ��� androidTest sourceSet��Ĭ����src/androidTest/·���¡�
�����sourceSet �ṹ����һ��ʹ��Android���Կ�ܣ����ҿ��Բ����豸�ϵĲ���apk������Ӧ�ó�����������԰�����Ԫ���ԣ����ɲ��ԣ��ͺ���UI�Զ������ԡ�
����Ӧ�õ�<instrumentation>�ڵ����Զ����ɵģ�������Ҳ���Դ���һ��src/androidTest/AndroidManifest.xml ���������manifest�ļ���������������
������һЩ����Ӧ�ÿ������õ�ֵ��
testPackageName
testInstrumentationRunner
testHandleProfiling
testFunctionalTest
����ǰ���������ģ���Щ��defaultConfig���������ã�
android {
    defaultConfig {
        testPackageName "com.test.foo"
        testInstrumentationRunner "android.test.InstrumentationTestRunner"
        testHandleProfiling true
        testFunctionalTest true
    }
}
�ڲ���Ӧ�ó����manifest�ļ��У�instrumentation�ڵ��targetPackage����ֵ���Զ�ʹ�ò���Ӧ�õ�package�������ã���ʹ���������ͨ��defaultConfig����Build Type�����Զ���ġ���Ҳ��manifest�ļ���Ҫ�Զ����ɵ�һ��ԭ��
���⣬�������sourceSetҲ����ӵ���Լ��������� Ĭ������£�Ӧ�ó���������������Զ���ӵĲ���Ӧ�õ�classpath�У�����Ҳ����ͨ����������չ��
dependencies {
    androidTestCompile 'com.google.guava:guava:11.0.2'
}
����Ӧ��ͨ��assembleTest������������assembleTest��������main�е�assemble ������Ҫ�ֶ��������У������Զ����С�
Ŀǰֻ��һ��Build Type�����ԡ�Ĭ���������debug Build Type��������Ҳ����ͨ�������Զ������ã�
android {
    ...
    testBuildType "staging"
}
5.3 ���в���
����ǰ���ᵽ�ģ����ͨ��ê������connectedCheck����������Ҫһ���豸�����ӡ�
�������������androidTest������˽�������androidTest�����task����ִ���������ݣ�
ȷ��Ӧ�úͲ���Ӧ�ö���������������assembleDebug��assembleTest��
��װ������Ӧ��
������Щ����
ж��������Ӧ��.
����ж���һ�������豸����ô���в��Զ���ͬʱ���������������豸�ϡ��������һ������ʧ�ܣ���������һ���豸�����������ʧ�ܡ�
���в��Խ����������ΪXML�ĵ���·��Ϊ��
build/androidTest-results
����ͳ����JUnit���ƣ����н��������build/test-results��
ͬ������Ҳ�����Զ������ã�
android {
    ...
 
    testOptions {
        resultsDir = "$project.buildDir/foo/results"
    }
}
android.testOptions.resultsDir��Project.file(String)��á�
5.4 ����Android��
����Android����Ŀ�ķ�����Ӧ����Ŀ�Ĳ��Է�������һ����
Ψһ�Ĳ�ͬ���������⣨�������������������Զ���Ϊ�����ⱻ��ӵ�����Ӧ���С�������ǲ���APK����ֻ�������Ĵ��룬�������˿���Ŀ�Լ��Ϳ������������
���manifest����ϵ�����Ӧ�õ�manifest�У���������Ŀ���������ʱһ������
androidTest���ֻ�ǰ�װ������ж�أ�����APK����Ϊû������APKҪ��װ����
�����Ĳ��ֶ������Ƶġ�
5.5 ���Ա���
�����е�Ԫ���Ե�ʱ��Gradle�����һ��HTML��ʽ�ı����Է���鿴����� 
Android pluginҲ�ǻ��ڴˣ�������չ��HTML�����ļ����������������豸�ı��涼�ϲ���һ���ļ����档
5.5.1 ������Ŀ
��Ŀ�����Զ����ɲ������У����Ա���Ĭ��λ�ã�
build/reports/androidTests
��ǳ�������JUnit�ı�������λ��build/reports/tests�������ı���ͨ��λ��build/reports/<plugin>/
���·��Ҳ����ͨ�����·�ʽ�Զ��壺
android {
    ...
 
    testOptions {
        reportDir = "$project.buildDir/foo/report"
    }
}
���潫��ϲ������ڲ�ͬ�豸�ϵĲ��Խ����
5.5.2 ����Ŀ���Ա���
��һ�������˶��Ӧ�û��߶������Ŀ����Ŀ�У���ͬʱ�������в��Ե�ʱ������һ����һ�����ļ���¼���еĲ��Կ����Ƿǳ����õġ�
Ϊ��ʵ�����Ŀ�ģ���Ҫʹ��ͬһ�������ļ��е���һ�����������ͨ�����·�ʽ��ӣ�
buildscript {
    repositories {
        mavenCentral()
    }
 
    dependencies {
        classpath 'com.android.tools.build:gradle:0.5.6'
    }
}
 
apply plugin: 'android-reporting'
�������ӵ���Ŀ�ĸ�Ŀ¼�£�������settings.gradle�ļ�ͬ��Ŀ¼��build.gradle�ļ��С�
Ȼ�����������е�������Ŀ��Ŀ¼�£�������������Ϳ����������в��Բ��ϲ����б��棺
gradle deviceCheck mergeAndroidReports --continue
ע�⣺--continue ѡ��������в��ԣ���ʹ����Ŀ�е��κ�һ������ʧ�ܶ�����ֹͣ�����û�����ѡ���һ��ʧ�ܲ��Խ�����ֹȫ�����Ե����У�����ܵ���һЩ��Ŀû��ִ�й����ǵĲ��ԡ�
5.6 Lint֧��
��0.7.0�汾��ʼ�������Ϊ��Ŀ��һ���ض���variant�汾����lint��Ҳ����Ϊ����variant�汾������lint������������һ������������һ��variant�汾�д��������⡣
�����ͨ������lintѡ������lint��ͨ���������ֻ��Ҫ��������һ���֣������г������п�ʹ�õ�ѡ�
android {
    lintOptions {
        // set to true to turn off analysis progress reporting by lint
        quiet true
        // if true, stop the gradle build if errors are found
        abortOnError false
        // if true, only report errors
        ignoreWarnings true
        // if true, emit full/absolute paths to files with errors (true by default)
        //absolutePaths true
        // if true, check all issues, including those that are off by default
        checkAllWarnings true
        // if true, treat all warnings as errors
        warningsAsErrors true
        // turn off checking the given issue id's
        disable 'TypographyFractions','TypographyQuotes'
        // turn on the given issue id's
        enable 'RtlHardcoded','RtlCompat', 'RtlEnabled'
        // check *only* the given issue id's
        check 'NewApi', 'InlinedApi'
        // if true, don't include source code lines in the error output
        noLines true
        // if true, show all locations for an error, do not truncate lists, etc.
        showAll true
        // Fallback lint configuration (default severities, etc.)
        lintConfig file("default-lint.xml")
        // if true, generate a text report of issues (false by default)
        textReport true
        // location to write the output; can be a file or 'stdout'
        textOutput 'stdout'
        // if true, generate an XML report for use by for example Jenkins
        xmlReport false
        // file to write report to (if not specified, defaults to lint-results.xml)
        xmlOutput file("lint-report.xml")
        // if true, generate an HTML report (with issue explanations, sourcecode, etc)
        htmlReport true
        // optional path to report (default will be lint-results.html in the builddir)
        htmlOutput file("lint-report.html")
 
   // set to true to have all release builds run lint on issues with severity=fatal
   // and abort the build (controlled by abortOnError above) if fatal issues are found
   checkReleaseBuilds true
        // Set the severity of the given issues to fatal (which means they will be
        // checked during release builds (even if the lint target is not included)
        fatal 'NewApi', 'InlineApi'
        // Set the severity of the given issues to error
        error 'Wakelock', 'TextViewEdits'
        // Set the severity of the given issues to warning
        warning 'ResourceAsColor'
        // Set the severity of the given issues to ignore (same as disabling the check)
        ignore 'TypographyQuotes'
    }
}
6 ������ͬ�汾(Build Variants)
�¹���ϵͳ��һ��Ŀ�����Ϊһ��Ӧ�ù�����ͬ�İ汾��
��������Ҫ�ĳ�����
ͬһ��Ӧ�õĲ�ͬ�汾�����磬��Ѱ���շѰ�
ͬһ��Ӧ�ã�Ϊ����Google Play Store�Ϸ�������������豸���������ͬ��apk��
��������������ۺ�
Ҳ����˵����ͬһ����Ŀ��������Щ��ͬ��apk��������ʹ��һ���⹤�̺�2�����ϵ���Ӧ�ù��̡�
6.1 ��Ʒ����(Product flavors)
һ��product flavor��������Ŀ���������һ���Զ���Ӧ�ð汾��һ��������Ŀ�����в�ͬ��flavor�������ɲ�ͬ��Ӧ�á�
����¸���(flavor)�����������ͬӦ�ð汾������С�����Ρ���������Ƿ�ͬһ��Ӧ�ã����Ļش��ǿ϶��Ļ������Ǳ�ʹ�ÿ���Ŀ���õ�������
flavorʹ��productFlavors ���DSL������������
android {
    ....
 
    productFlavors {
        flavor1 {
            ...
        }
 
        flavor2 {
            ...
        }
    }
}
���ﴴ��������flavor���ֱ��� flavor1 �� flavor2��
ע�⣺ flavor�����ֲ��ܺ����еĹ�������(Build Type)���ֳ�ͻ�����ߺ�androidTest���sourceSet�����ֳ�ͻ��
6.2 �������ͣ���Ʒ���ƣ����ְ汾(Build Type + Product Flavor = Build Variant)
ǰ���Ѿ��ᵽ��ÿһ���������Ͷ�������һ��apk�����˵Ļ����뿴3.4.2
Product Flavors Ҳ����ͬ�������飬ʵ���ϣ���Ŀ����������п��ܵģ�Build Types��Product Flavors����ϣ������Product Flavors�Ļ���
ÿ��Build Types��Product Flavors����Ͼ���һ��Build Variant��
���磬Ĭ�ϵ�debug �� release������Build Types�������洴��������flavor������4��Build Variants��
Flavor1 - debug
Flavor1 - release
Flavor2 - debug
Flavor2 - release
û��flavor����ĿҲ��Build Variants��ʹ��Ĭ�ϵ�û�����ֵ�flavor���ã�ʹ��Build Variants�б������� Build Typesһ����
6.3 ProductFlavor����
ÿ��flavor�����������ıհ��ṹ�����ã�
android {
    ...
 
    defaultConfig {
        minSdkVersion 8
        versionCode 10
    }
 
    productFlavors {
        flavor1 {
            packageName "com.example.flavor1"
            versionCode 20
        }
 
        flavor2 {
            packageName "com.example.flavor2"
            minSdkVersion 14
        }
    }
}
ע�⵽android.productFlavors.*��android.defaultConfig��������������ͬ������ζ�����ǹ�����ͬ�����ԡ�
defaultConfigΪ���е�flavor�ṩĬ�ϵ����ã�ÿ��flavor�����Ը����������ֵ������������У����յ��������£�
flavor1
packageName: com.example.flavor1
minSdkVersion: 8
versionCode: 20
flavor2
packageName: com.example.flavor2
minSdkVersion: 14
versionCode: 10
ͨ����Build Type���ûḲ���������á�����Build Type����packageNameSuffix����ӵ�Product Flavor���� packageName�ϡ�
Ҳ��һЩ����£�һ�����������ͬʱ��Build Type �� Product Flavor���������ã���ʱ����Ҫ���������������ˡ�
���磬signingConfig��������һ�������
��������android.buildTypes.release.signingConfig������release�汾ʹ��ͬһ��SigningConfig��Ҳ���Ե�������android.productFlavors.*.signingConfig�ø�releaseʹ�ø��Ե�SigningConfig��
6.4 Դ���Ϻ�������ϵ
�͹����������ƣ���ƷflavorҲ����ͨ�������Լ���sourceSetsӰ�����յĴ������Դ
������������У��������ĸ�sourceSet��
android.sourceSets.flavor1
λ�� src/flavor1/
android.sourceSets.flavor2
λ�� src/flavor2/
android.sourceSets.androidTestFlavor1
λ�� src/androidTestFlavor1/
android.sourceSets.androidTestFlavor2
λ�� src/androidTestFlavor2/
��ЩsourceSet ������������apk����android.sourceSets.main�Լ��������͵�sourceSetһ��
�����ǹ���apkʱ������sourceSet�Ĵ���ԭ��
�����ļ������Դ��(src/*/java)���ᱻ�ϲ���������һ�������
���Manifest�ļ���ϲ���һ��������ʹ��flavor�͹�������һ���������в�ͬ�������permission
������Դ��ʹ����ѭ���ȼ����ǣ�Product Flavor��Դ����main sourceSet��Դ��Build Type��Դ����Product Flavor��Դ
ÿ��Build Variant�����Դ�����ɸ��Ե�R�ļ��������������ɵ�Դ�룩������Build Variant���Ṳ���κζ�����
��󣬺�Build Typeһ����Product Flavor���������Լ������������磬flavor������һ�����汾��һ��֧���汾����ô�ͻ��������sdk���������汾��������
dependencies {
    flavor1Compile "..."
}
������ر������£�src/flavor1/AndroidManifest.xmlҲ����Ҫ���һ������Ȩ��
ÿ��variantҲ����������sourceset��
android.sourceSets.flavor1Debug
λ�� src/flavor1Debug/
android.sourceSets.flavor1Release
λ�� src/flavor1Release/
android.sourceSets.flavor2Debug
λ�� src/flavor2Debug/
android.sourceSets.flavor2Release
λ�� src/flavor2Release/
��Щsourceset��build type��sourceset�и��ߵ����ȼ�������variant����Ķ��ơ�
6.5 ����������
ǰ���ᵽ��ÿ��Build Type���Լ���assemble<name>���񡣵���Build Variant��Build Type �� Product Flavor��ϡ�
��ʹ��Product Flavor��ʱ�򣬸����assemble-type����ᱻ�����������ֱ��ǣ�
assemble<Variant Name>
����ֱ�ӹ���һ��Variant�汾������assembleFlavor1Debug��
assemble<Build Type Name>
������ָ��Build Type������APK������assembleDebug���ṹ��Flavor1Debug��Flavor2Debug����Variant�汾��
assemble<Product Flavor Name>
������ָ��flavor������APK������assembleFlavor1���ṹ��Flavor1Debug��Flavor1Release����Variant�汾��
assemble����ṹ�����п��ܵ�variant�汾��
6.6 ����
���Զ�flavor����Ŀ�ͼ���Ŀʮ�����ơ�
androidTest sourceset��������������flavor��ͨ�ò��ԣ�ͬʱ��ÿ��flavorҲ�����и��ԵĲ��ԡ�
���������ᵽ�ģ����Ը�flavor��sourceSet�ᱻ������
android.sourceSets.androidTestFlavor1
λ�� src/androidTestFlavor1/
android.sourceSets.androidTestFlavor2
λ�� src/androidTestFlavor2/
ͬ��������Ҳ�����������Լ���������
dependencies {
    androidTestFlavor1Compile "..."
}
����ͨ��ê������deviceCheck�����в��ԣ�����androidTest���񣨵�ʹ��flavorʱ������Ϊê�����񣩡�
ÿ��flavor���Լ����������в��ԣ�androidTest<VariantName>�����磺
androidTestFlavor1Debug
androidTestFlavor2Debug
���Ƶģ�ÿ��variant���й�������apk�Ͱ�װ/ж������
assembleFlavor1Test
installFlavor1Debug
installFlavor1Test
uninstallFlavor1Debug
��
������ɵ�HTML����֧�ְ���flavor�ϲ���
���Խ���ͱ���λ�����£�������ÿ��flavor�汾�ģ�Ȼ���Ǻϲ��ġ�
build/androidTest-results/flavors/
build/androidTest-results/all/
build/reports/androidTests/flavors
build/reports/androidTests/all/
�ı���һ��·����ֻ��Ӱ���Ŀ¼����Ȼ��Ϊÿ��flavor�ͺϲ���Ľ��������Ŀ¼��
6.7 ��flaverά�ȵİ汾(Multi-flavor variants)
ĳЩ����£�Ӧ�ÿ�����Ҫ���ڶ����׼����������汾��
���磬Google Play��multi-apk֧��4����ͬ�Ĺ�������Ϊÿһ����������������apkҪ��ʹ�ö��Product Flavorά�ȡ�
������һ����Ϸ��Ŀ����demo�͸��Ѱ汾����Ҫʹ��multi-apk�е�ABI������������Ҫ���3��ABI�������汾��������Ҫ����6��apk��û�м�����Build Type�����İ汾����
Ȼ�������Ѱ汾�Ĵ��������������ABI����һ������˴����򵥵�6��flavor����һ���÷�����
�෴�ģ���flavor��Ϊ����ά�ȣ����Զ��������п��ܵ����variant��
�������ͨ��Flavor Dimensions��ʵ�֡�flavor�������䵽һ���ض���ά��
android {
    ...
 
    flavorDimensions "abi", "version"
 
    productFlavors {
        freeapp {
            flavorDimension "version"
            ...
        }
 
        x86 {
            flavorDimension "abi"
            ...
        }
    }
}
��android.flavorDimensions�����ж�����ܵ�ά�ȣ�����ÿ��flavor��ָ��һ��ά�ȡ�
�����Ѿ�����ά�ȵ�flavor([freeapp, paidapp] �� [x86, arm, mips])����Build Type[debug, release]���ᴴ������variant��
x86-freeapp-debug
x86-freeapp-release
arm-freeapp-debug
arm-freeapp-release
mips-freeapp-debug
mips-freeapp-release
x86-paidapp-debug
x86-paidapp-release
arm-paidapp-debug
arm-paidapp-release
mips-paidapp-debug
mips-paidapp-release
android.flavorDimensions�ж���ά�ȵ�˳��ǳ���Ҫ��
ÿ��variant�����ɶ��Product Flavor���������
android.defaultConfig
One from the abi dimension
One from the version dimension
ά�ȵ�˳������ĸ�flavor�����ûḲ����һ���������Դ��˵����Ҫ�������ȼ�flavor�е���Դ���滻�����ȼ��ġ�flavorά�ȶ���ʱ�����ȼ���ǰ����������������У�
abi > version > defaultConfig
��άflavor��ĿҲ�ж����sourceset����variant���ƣ�����û��build type��
android.sourceSets.x86Freeapp Location src/x86Freeapp/
android.sourceSets.armPaidapp Location src/armPaidapp/
etc��
��Щsourceset������flavor-combination�ļ�����ж��ơ����ǱȻ�����flavor sourceset���ȼ��ߣ����Ǳ�build type sourceset���ȼ��͡�
7 �߼���������
7.1 ����ѡ��
7.1.1 Java����ѡ��
android {
    compileOptions {
        sourceCompatibility = "1.6"
        targetCompatibility = "1.6"
    }
}
Ĭ��ֵ��1.6��Ӱ�����б���javaԴ�������
7.1.2 aaptѡ��
android {
    aaptOptions {
        noCompress 'foo', 'bar'
        ignoreAssetsPattern "!.svn:!.git:!.ds_store:!*.scc:.*:<dir>_*:!CVS:!thumbs.db:!picasa.ini:!*~"
    }
}
Ӱ������ʹ��aapt������
7.1.3 dexѡ��
android {
    dexOptions {
        incremental false
        preDexLibraries = false
        jumboMode = false
        javaMaxHeapSize "2048M"
    }
}
Ӱ������ʹ��dex������
7.2 �޸Ĺ�������
������Java��Ŀ��һ�����޵�����ͬ���������������
classes������һ������JavaԴ������񡣺�������build.gradle�ļ��Ľű�����classes���á�����project.tasks.classes����д��
��Android��Ŀ�У�������е㸴�ӣ���Ϊ���ڴ���ͬ�����������ǵ������ǻ���Build Type �� Product Flavor���ɵġ�
Ϊ�˽��������⣬android���������ԣ�
applicationVariants (ֻ������ app plugin)
libraryVariants (ֻ������ library plugin)
testVariants (���߶�����)
�����߷ֱ𷵻�һ��ApplicationVariant, LibraryVariant, �� TestVariant�����DomainObjectCollection��
Ҫע��ʹ����Щcollection�е���һ�����ᴥ���������е���������ζ��ʹ��collection֮��Ӧ���޸����á�
DomainObjectCollection����ֱ�ӷ������ж��󣬻���ͨ��������ɸѡ�������㣩��
android.applicationVariants.each { variant ->
    ....
}
��������variant����������Щ���ԣ�
Property Name	Property Type	Description
name	String	variant�����֣�������Ψһ��
description	String	variant������
dirName	String	Variant�����ļ�������������Ψһ�ġ�����Ҳ���ж�����ļ��У����硰debug/flavor1��
baseName	String	variant����Ļ������֣�����Ψһ
outputFile	File	Variant�����������һ���ɶ�д������
processManifest	ProcessManifest	����Manifest������
aidlCompile	AidlCompile	����AIDL�ļ��ĵ�����
renderscriptCompile	RenderscriptCompile	����Renderscript�ļ�������
mergeResources	MergeResources	�ϲ���Դ�ļ�������
mergeAssets	MergeAssets	�ϲ�asset������
processResources	ProcessAndroidResources	����������Դ�ļ�������
generateBuildConfig	GenerateBuildConfig	����BuildConfig�������
javaCompile	JavaCompile	����JavaԴ���������
processJavaResources	Copy	����Java��Դ�ļ�������
assemble	DefaultTask	variant�ı�־����assemble
ApplicationVariantӵ�����¶������ԣ�
Property Name	Property Type	Description
buildType	BuildType	variant�Ĺ�������
productFlavors	List	Variant��ProductFlavor��һ�㲻Ϊ�յ������ֵ
mergedFlavor	ProductFlavor	android.defaultConfig��variant.productFlavors�ĺϲ�
signingConfig	SigningConfig	variantʹ�õ�SigningConfig����
isSigningReady	boolean	�����true��������variant�Ѿ��߱���ǩ�������������Ϣ
testVariant	BuildVariant	����������variant��TestVariant
dex	Dex	����������dex�����񣬿⹤�̸����Կ���Ϊ��
packageApplication	PackageApplication	���������apk�����񣬿⹤�̸����Կ���Ϊ��
zipAlign	ZipAlign	��apk���ж���(zipalign)�����񣬿⹤�̻���apk�޷�ǩ��ʱ�������Կ���Ϊ��
install	DefaultTask	��װapk�����񣬿���Ϊ��
uninstall	DefaultTask	ж������
LibraryVariantӵ�����¶������ԣ�
Property Name	Property Type	Description
buildType	BuildType	variant�Ĺ�������
mergedFlavor	ProductFlavor	The defaultConfig values
testVariant	BuildVariant	���ڲ������variant��Variant
packageLibrary	Zip	����ɿ⹤��AAR�ļ������񣬷ǿ⹤�̸�����Ϊ��
TestVariantӵ�����¶������ԣ�
Property Name	Property Type	Description
buildType	BuildType	variant�Ĺ�������
productFlavors	List	Variant��ProductFlavor��һ�㲻Ϊ�յ������ֵ
mergedFlavor	ProductFlavor	android.defaultConfig��variant.productFlavors�ĺϲ�
signingConfig	SigningConfig	variantʹ�õ�SigningConfig����
isSigningReady	boolean	�����true��������variant�Ѿ��߱���ǩ�������������Ϣ
testedVariant	BaseVariant	����ǰTestVariant���Ե�BaseVariant
dex	Dex	����������dex�����񣬿⹤�̸����Կ���Ϊ��
packageApplication	PackageApplication	���������apk�����񣬿⹤�̸����Կ���Ϊ��
zipAlign	ZipAlign	��apk���ж���(zipalign)�����񣬿⹤�̻���apk�޷�ǩ��ʱ�������Կ���Ϊ��
install	DefaultTask	��װapk�����񣬿���Ϊ��
uninstall	DefaultTask	ж������
connectedAndroidTest	DefaultTask	�������ӵ��豸������android���Ե�����
providerAndroidTest	DefaultTask	ʹ����չAPI����android���Ե�����
API for Android specific task types.
android���������API��
ProcessManifest
File manifestOutputFile
AidlCompile
File sourceOutputDir
RenderscriptCompile
File sourceOutputDir
File resOutputDir
MergeResources
File outputDir
MergeAssets
File outputDir
ProcessAndroidResources
File manifestFile
File resDir
File assetsDir
File sourceOutputDir
File textSymbolOutputDir
File packageOutputFile
File proguardOutputFile
GenerateBuildConfig
File sourceOutputDir
Dex
File outputFolder
PackageApplication
File resourceFile
File dexFile
File javaResourceDir
File jniDir
File outputFile
ֱ����Variant������ʹ�á�outputFile�����Ըı����յ�����ļ���
ZipAlign
File inputFile
File outputFile
ֱ����Variant������ʹ�á�outputFile�����Ըı����յ�����ļ���
����Gradle�Ĺ�����ʽ��Android plugin�����÷�ʽ�� ÿ��task���͵�API�����޵ġ� 
���ȣ�Gradleʹ������ֻ���������������·����һЩ����ʹ�õ�ѡ���ʶ����ˣ�����ֻ����һЩ������������
��Σ��������������붼�ܸ��ӣ�һ�㶼�����sourceSet��Build Type��Product Flavor�е�ֵ��Ϊ�˱��ֹ����ļ��ļ򵥣��ɶ������ǵ�Ŀ�����ÿ�����ͨ����΢�Ķ�DSL�������޸Ĺ������̣����������뵽�����ļ�������ѡ����ȥ��
������Ҫע�⣬����ZipAlign����������ͣ������������Ͷ�Ҫ������˽�����������������С�����ζ�Ų����ֶ�������Щ�����ʵ����
��ЩAPIҲ���ܸı䡣������˵����ǰ��API��Χ�����޸���������루���ܵĻ������������Ӷ���Ĵ�����̣���Ҫ�Ļ�������ӭ�������ر�����Щû��Ԥ���������⡣
����Gradle����(DefaultTask, JavaCompile, Copy, Zip)����ο�Gradle�ĵ���
7.3 BuildType��Product Flavor���Բο���
�����ڴ��� 
����Gradle����(DefaultTask, JavaCompile, Copy, Zip)����ο�Gradle�ĵ���
7.4 ʹ�ã�JDK��1.7�汾��sourceCompatibility��
ʹ��Android KitKat��buildTools v19���Ϳ���ʹ��diamond operator��multi-catch����switch��ʹ���ַ�����try with resource�ȵȣ�jdk7�е������ԣ���Ҫʹ����Щ����Ҫ�޸���Ĺ����ļ����£�
android {
    compileSdkVersion 19
    buildToolsVersion "19.0.0"
 
    defaultConfig {
        minSdkVersion 7
        targetSdkVersion 19
    }
 
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }
}
ע�⣺����Խ�minSdkVersion��ֵ����Ϊ19֮ǰ�İ汾��ֻ����ֻ��ʹ�ó���try with resources֮����������ԡ��������Ҫʹ��try with resources���ԣ������Ҫ��minSdkVersionҲ����Ϊ19��
��ͬ��Ҳ��Ҫȷ��Gradleʹ��1.7���߸��߰汾��JDK����Android Gradle pluginҲ��Ҫ0.6.1���߸��ߵİ汾��

Auth at  2017.6.13