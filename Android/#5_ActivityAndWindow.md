  
## Window��Activity;(�������޵Ĳ���)


 WindowManagerService������°������·�ʽ��������Щ������Ҫ��ʾ���Լ�Ҫ��ʾ�����

       1. ÿһ��Activity���ڵĴ�С��������Ļ�Ĵ�С����ˣ�ֻҪ��ÿһ��Activity��������һ����ͬ��Z��λ�ã�Ȼ��Ϳ���ʹ��λ��������ģ�����ǰ�������Activity���ڣ����ǿɼ��ġ�

       2. ÿһ���Ӵ��ڵ�Z��λ�ö������ĸ����ڴ󣬵��Ǵ�СҪ�ȸ�����С����ʱ��Activity���ڼ������������Ӵ��ڶ�����ͬʱ��ʾ������

       3. ���ڷ�ȫ��Activity������˵����������Ļ���Ϸ�����һ������������ʾ״̬�������������������ƶ�����Ļ��˵����Ϊװ������decoration����������Activity������˵����Ϊ���ݱ߳�����Content Inset����

       4. ���뷨����ֻ������Ҫ��ʱ��Ż���֣���ͬ���ǳ�������Ļ��װ��������˵Activity���ڵ����ݱ߳����ġ�

       5. ���ڱ�ֽ���ڣ���������Ҫ��ֽ��Activity���ڵ��·�����ʱ��Ҫ��Activity�����ǰ�͸���ģ������Ϳ��Խ�������ı�ֽ����һͬ��ʾ������

       6. ����Activity�������л����̣�ʵ���Ͼ���ǰһ��������ʾ�˳���������һ��������ʾ��ʼ�����Ĺ��̣����ڶ�������ʾ���̣����ڵĴ�С����һ���仯�Ĺ��̣������͵���ǰ������Activity���ڵĴ�С���ٶ�������Ļ�Ĵ�С��������Ǿ��п���ͬʱ�����ڿɼ���״̬����ʵ�ϣ�Activity���ڵ��л��������൱���ӵģ���Ϊ����Ҫ��ʾ��Activity���ڿ��ܻ��ᱻ����һ���������ڣ�Starting Window����һ�����������������ڵ�Activity����Ҫ�ȵ���������������ʾ��֮��ſ�����ʾ������



<Activity���������е���Ҫ������>
ActivityThread.handleLanuchActivity()����������Activity;

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


A: ��AndroidӦ�ó��򴰿ڣ�Activity�������������Ļ�����Context���Ĵ������̷���һ���ֿ���֪������Activity�����������һ��PhoneWindow�����Ǵ�Activity��ĳ�Ա����attach�д�����.


B:   ����˵��ViewRoot�൱����MVCģ���е�Controller����������ְ��

        1. ����ΪӦ�ó��򴰿���ͼ����Surface��

        2. ���WindowManagerService������ϵͳ��Ӧ�ó��򴰿ڡ�

        3. ����������ֺ���ȾӦ�ó��򴰿���ͼ��UI��


C:   �������ȵ���ActivityThread��ĳ�Ա����performLaunchActivity������Ҫ������Activity������ڴ���Activity����Ĺ����У�����Ϊ��Activity����������ڶ������ͼ����Activity����������֮�󣬾Ϳ��Խ������������ˣ�����ͨ������ActivityThread��ĳ�Ա����handleResumeActivity��ִ�еġ�

ViewRootImp
D:  --mView����������ΪView������ʵ����ָ�����һ��DecorView������������Ӧ�ó��򴰿ڵĶ�����ͼ����һ����Բο�ǰ��AndroidӦ�ó��򴰿ڣ�Activity������ͼ����View���Ĵ������̷���һ�ġ�

        --mLayoutRequested������һ��������������������Ӧ�ó�����̵�UI�߳��Ƿ���Ҫ���ڱ�����ִ��һ��UI���ֲ�����

        --mFirst������һ��������������������Ӧ�ó�����̵�UI�߳��Ƿ��һ�δ���һ��Ӧ�ó��򴰿ڵ�UI��

        --mFullRedrawNeeded������һ��������������������Ӧ�ó�����̵�UI�߳��Ƿ���Ҫ��һ��Ӧ�ó��򴰿ڵ�ȫ���������»��ơ�

        --mSurface����ָ��һ��Java���Surface������������һ��Ӧ�ó��򴰿ڵĻ�ͼ���档
WindowState:
E:WindowState��ĳ�Ա����mFrame����������Ӧ�ó��򴰿ڵ�λ�úʹ�С�ģ����У�λ�þ���ͨ������������һ��Rect����ĳ�Ա����left��top����ʾ�ģ����Ƿֱ�Ӧ�ô�����X���Y���ϵ�λ��
	WindowState��ĳ�Ա����mAnimLayer��������һ��Ӧ�ó��򴰿ڵ�Z��λ��


























