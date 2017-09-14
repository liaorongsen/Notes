  
## Window与Activity;(参照老罗的博客)


 WindowManagerService服务大致按照以下方式来控制哪些窗口需要显示的以及要显示在哪里：

       1. 每一个Activity窗口的大小都等于屏幕的大小，因此，只要对每一个Activity窗口设置一个不同的Z轴位置，然后就可以使得位于最上面的，即当前被激活的Activity窗口，才是可见的。

       2. 每一个子窗口的Z轴位置都比它的父窗口大，但是大小要比父窗口小，这时候Activity窗口及其所弹出的子窗口都可以同时显示出来。

       3. 对于非全屏Activity窗口来说，它会在屏幕的上方留出一块区域，用来显示状态栏。这块留出来的区域称对于屏幕来说，称为装饰区（decoration），而对于Activity窗口来说，称为内容边衬区（Content Inset）。

       4. 输入法窗口只有在需要的时候才会出现，它同样是出现在屏幕的装饰区或者说Activity窗口的内容边衬区的。

       5. 对于壁纸窗口，它出现需要壁纸的Activity窗口的下方，这时候要求Activity窗口是半透明的，这样就可以将它后面的壁纸窗口一同显示出来。

       6. 两个Activity窗口在切换过程，实际上就是前一个窗口显示退出动画而后一个窗口显示开始动画的过程，而在动画的显示过程，窗口的大小会有一个变化的过程，这样就导致前后两个Activity窗口的大小不再都等于屏幕的大小，因而它们就有可能同时都处于可见的状态。事实上，Activity窗口的切换过程是相当复杂的，因为即将要显示的Activity窗口可能还会被设置一个启动窗口（Starting Window）。一个被设置了启动窗口的Activity窗口要等到它的启动窗口显示了之后才可以显示出来。



<Activity创建过程中的重要方法：>
ActivityThread.handleLanuchActivity()来启动整个Activity;

  1.ActivityThread.performLanchActivity()-->return a activity;
	activity.attach(Context context,ActivityThread aThread);
  		WindowPolicy.makeNewWindow()->return a PhoneWindow;
		window.setCallBack(this);->add activity to the window it attached;
	activity.onCreate()-PhoneWindow.setContentView(int layoutResId)
	installDecor()-->return a DecorView; add the viewTree layoutResId described into decorView;
	activity.performStart()->mInstrumention.callActivityOnStart()->activity.onStart();
	
  2.ActivityThread.handleResumeActivity()-->
	ActivityThraed.performResumeActivity();->activity.performResume()->mInstrumention.callActivityOnResume()->activity.onResume();
	
	LocalWindowManagerImp->,hold:
		ArrayList<View> mViews;
		ArrayList<ViewRootImp> mRoots;
		ArrayList<WindowManager.LayoutParams> mParams;
	mWindowManager.add(decor,params);
		mViews.add(decor);
		ViewRootImp root=new ViewRootImp(decor.getContext,display);
		mRoots.add(root);
		mParams.add(params); DecorView,ViewRootImp,LayoutParams attach by the same index in  the list;
		root.setView(view, wparams, panelParentView);
			requestLayout();--> 
				checkThread();
					check current Thread is the thread create ViewRootImp;false->throw Expection;
				scheduleTraversals()-->doTraversal()-->performTraversals()
					decor.measure();
					decor.layout();
					viewRootImp.draw();


A: 从Android应用程序窗口（Activity）的运行上下文环境（Context）的创建过程分析一文又可以知道，与Activity组件所关联的一个PhoneWindow对象是从Activity类的成员函数attach中创建的.


B:   简单来说，ViewRoot相当于是MVC模型中的Controller，它有以下职责：

        1. 负责为应用程序窗口视图创建Surface。

        2. 配合WindowManagerService来管理系统的应用程序窗口。

        3. 负责管理、布局和渲染应用程序窗口视图的UI。


C:   函数首先调用ActivityThread类的成员函数performLaunchActivity来创建要启动的Activity组件。在创建Activity组件的过程中，还会为该Activity组件创建窗口对象和视图对象。Activity组件创建完成之后，就可以将它激活起来了，这是通过调用ActivityThread类的成员函数handleResumeActivity来执行的。

ViewRootImp
D:  --mView：它的类型为View，但它实际上指向的是一个DecorView对象，用来描述应用程序窗口的顶级视图，这一点可以参考前面Android应用程序窗口（Activity）的视图对象（View）的创建过程分析一文。

        --mLayoutRequested：这是一个布尔变量，用来描述应用程序进程的UI线程是否需要正在被请求执行一个UI布局操作。

        --mFirst：这是一个布尔变量，用来描述应用程序进程的UI线程是否第一次处理一个应用程序窗口的UI。

        --mFullRedrawNeeded：这是一个布尔变量，用来描述应用程序进程的UI线程是否需要将一个应用程序窗口的全部区域都重新绘制。

        --mSurface：它指向一个Java层的Surface对象，用来描述一个应用程序窗口的绘图表面。
WindowState:
E:WindowState类的成员变量mFrame是用来描述应用程序窗口的位置和大小的，其中，位置就是通过它所描述的一个Rect对象的成员变量left和top来表示的，它们分别应用窗口在X轴和Y轴上的位置
	WindowState类的成员变量mAnimLayer用来描述一个应用程序窗口的Z轴位置


























