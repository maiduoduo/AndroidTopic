
<br/><br/><br/>
# Android项目涉及框架及使用说明 #
> **2019/4/3   <br/>                丁成磊**
> <br/>**整理皆是项目中使用到的频率高的或者参考度高的开源库，如有不尽之处或者不对请指正详明**
> <br/>**开源库都会注明出处**<br/>
> <br/>**开源库在某些业务需求繁冗，给开发时间成本增加，为了提高效率，提高性能，我们建议引用及延伸，但是不能一味依赖开源，要逐渐成为一个造轮子的人，支持原生，更要自我扩展<br/>   
> 还是那句话，不能过度依赖第三方，最明显问题，过度依赖带来的类库冲突，超出一个Dex文件存储上限，打包异常问题。等等问题，包括资产文件冲突重名都是泛滥使用第三方引起的不良反应，当然有时是不可逆的操作，要合理运用。**
> **不做拿来主义**     
> **安卓原生是个庞大工程，层出不穷的开放性方案，让我从无到有，现在却面对着如何甄选适合的，稳定的，扩展性高的第三方支持库，所以良性使用引入库，才能让项目被侵入性的可能性越低，当然百家之言百家之所在。
我们时刻谨记着，别人的成果我们不能做拿来主义原封不动的据为己有，知识驱动进步靠每个行业人的贡献与不竭追求**




![](https://goss4.vcg.com/creative/vcg/800/version23/VCG41496385810.jpg)



## 写在前面 ##

- 开发环境01：AndroidStudio2.2.3   gradle	: gradle-2.14.1-all.zip    
- 	开发环境02：AndroidStudio3.2.0   gradle	: gradle-4.6-all.zip    <br/> <br/>


# 目录
-----------------------------

* [框架类库](#框架类库)
* [注](#注)
* [开发者常用网站](#开发者常用网站)
* [开发软件辅助工具](#开发软件辅助工具)
* [常用的配置](#常用的配置)
* [推荐书籍](#推荐书籍)
* [颜色取色参考表](#颜色取色参考表)










## 框架类库 ##
### 一.ButterKnife（资源快速注入框架） ###
> 官文出处：[https://github.com/JakeWharton/butterknife](https://github.com/JakeWharton/butterknife)<br/>
> 配置：（这里配置是官网最新版版本10.1.0配置，8.4-8.8版本配置依然参试官网）  

 
<br/>
**module配置** 

<pre style="background:#222;color:#35b558">
	apply plugin: 'com.android.library'   
	apply plugin: 'com.jakewharton.butterknife'   
	dependencies {    
		implementation 'com.jakewharton:butterknife:10.1.0'     
		annotationProcessor 'com.jakewharton:butterknife-compiler:10.1.0'    
	} 	
</pre>					
<br/>
<br/>
**project的build.gradle配置**   
<pre style="background:#222;color:#35b558">
	buildscript {	
		repositories {
				mavenCentral()
				google()
		}
		dependencies {
			    classpath 'com.jakewharton:butterknife-gradle-plugin:10.1.0'
		}
	}		
</pre>
<br/>
<br/>
**使用** 
<pre style="background:#222;color:#ffb558"> 
class ExampleActivity extends Activity {
    //绑定控件资源地址，拿到控件
	@BindView(R.id.user) EditText username;
	@BindView(R.id.pass) EditText password;
	//绑定string.xml资源文件下字符串资源
	@BindString(R.string.login_error) String loginErrorMessage;

	//注册监听
	 @OnClick(R.id.submit) 
		public void submit() {
	 }
	 @Override 
	 public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.simple_activity);
		//注册声明
		ButterKnife.bind(this);
		// TODO Use fields...
	 }
}     					
</pre>

 



<br/>
<br/>
**注意**	    


-  **Android Studio 3.0集成ButterKnife8.8.1版本出现问题以及解决方法**

	        
	> 参考：[https://blog.csdn.net/oYESIDO/article/details/79917959](https://blog.csdn.net/oYESIDO/article/details/79917959)    
	> Androidstudio3.0以上版本ButterKnife注入8.6-8.8版本目前官网具体有说明关于引入及注意事项，请参阅。总体来说降级一下,注释掉一行。
	> 有兴趣研究原理可参考：[https://blog.csdn.net/xsf50717/article/details/78428986](https://blog.csdn.net/xsf50717/article/details/78428986)

- 界面中对于已绑定布局文件：鼠标点击在布局文件上，快捷键：**Alt+Insert-->Generate butterknife injections**可以快速注入引入控件绑定。     
- 在Activity 类中绑定 ：ButterKnife.bind(this);
	> 必须在setContentView();之后绑定；且父类bind绑定后，子类不需要再bind。          
	> 在非Activity 类（eg：Fragment、ViewHold）中绑定： ButterKnife.bind(this，view);这里的this不能替换成getActivity（）。       
- 在Activity中不需要做解绑操作，在Fragment 中必须在onDestroyView()中做解绑操作。        
- 使用ButterKnife修饰的方法和控件，不能用private or static 修饰，否则会报错。错误: @BindView fields must not be private or static    
- setContentView()不能通过注解实现。（其他的有些注解框架可以）    
- 使用Activity为根视图绑定任意对象时，如果你使用类似MVC的设计模式你可以在Activity 调用ButterKnife.bind(this, activity)，来绑定Controller。    
- 使用ButterKnife.bind(this，view)绑定一个view的子节点字段。

	> 如果你在子View的布局里或者自定义view的构造方法里 使用了inflate,你可以立刻调用此方法。   
	> 或者，从XMLinflate来的自定义view类型可以在onFinishInflate回调方法中使用它。     
      
- **Library中使用的时候用R2代替R，BindView(R2...)，注意在做分支语句请使用if-else代替switch-case不然会报错。**      
				[https://github.com/JakeWharton/butterknife/issues/771      ](https://github.com/JakeWharton/butterknife/issues/771      )           
				[https://blog.csdn.net/pouloghost/article/details/80901364   ](https://blog.csdn.net/pouloghost/article/details/80901364   )  



- --- ---------- ---              ------------ ---------- --------              ---------------

<br/>

###二、SystemBarTint 沉浸式 ###

  > 地址：[https://github.com/jgilfelt/SystemBarTint  ](https://github.com/jgilfelt/SystemBarTint  )      
  >文档：[https://www.jianshu.com/p/7ceb6cd44839](https://www.jianshu.com/p/7ceb6cd44839)

Android从4.4开始支持这种显示效果，直接看对比图：

![](https://img-blog.csdn.net/20160121101207297)

从上图可以看到左边淘宝APP最顶部的状态栏背景是黑色的，而右边的360手机助手那个位置不是黑色，就是用了沉浸式状态栏这种效果。

**引入**
<pre style="background:#d7ccc8;color:#3e2723">
 dependencies {
	compile 'com.readystatesoftware.systembartint:systembartint:1.0.3'
 }
</pre>
**基类activity中使用**
<pre style="background:#d7ccc8;color:#3e2723">@Override
protected void onCreate(Bundle savedInstanceState) {
   super.onCreate(savedInstanceState);
   setContentView(R.layout.activity_main);
   // create our manager instance after the content view is set
   SystemBarTintManager tintManager = new SystemBarTintManager(this);
   // enable status bar tint
   tintManager.setStatusBarTintEnabled(true);
   // enable navigation bar tint
   tintManager.setNavigationBarTintEnabled(true);
}
</pre>
   > 注意：清单文件必须配置activity的主题为：theme:Theme.appcompat...


###三、图片选择器 Imagepicker ###
> imagepicker是一款用于在Android设备上获取照片（拍照或从相册、文件中选择）、压缩图片的开源工具库，目前最新版本V1.2.0。

- 从相册里面选择图片或者拍照获取照片
- 浏览选择的本地或者网络图片
- 保存图片

<br/>
 <br/>                   
<div align=center><img  src="https://raw.githubusercontent.com/917386389/imagepickerdemo/master/app/src/4.gif"/></div>

  > **官文入口**：[https://github.com/jeasonlzy/ImagePicker  ](https://github.com/jeasonlzy/ImagePicker  )     
  > **文档**：[https://www.jianshu.com/p/f7082aa7b735](https://www.jianshu.com/p/f7082aa7b735)


**用法**

使用前，对于Android Studio的用户，可以选择添加:
		


			compile 'com.lzy.widget:imagepicker:0.6.1'  //指定版本


		

**功能和参数含义**

### 温馨提示:目前库中的预览界面有个原图的复选框,暂时只做了UI,还没有做压缩的逻辑

 | 配置参数|参数含义|
|:--:|--|
|multiMode|图片选着模式，单选/多选|
|selectLimit|多选限制数量，默认为9|
|showCamera|选择照片时是否显示拍照按钮|
|crop|是否允许裁剪（单选有效）|
|style|有裁剪时，裁剪框是矩形还是圆形|
|focusWidth|矩形裁剪框宽度（圆形自动取宽高最小值）|
|focusHeight|矩形裁剪框高度（圆形自动取宽高最小值）|
|outPutX|裁剪后需要保存的图片宽度|
|outPutY|裁剪后需要保存的图片高度|
|isSaveRectangle|裁剪后的图片是按矩形区域保存还是裁剪框的形状，例如圆形裁剪的时候，该参数给true，那么保存的图片是矩形区域，如果该参数给fale，保存的图片是圆形区域|
|imageLoader|需要使用的图片加载器，自需要实现ImageLoader接口即可|




#### **代码参考** ####

  >更多使用，请下载demo参看源代码
  
------------------------------------------------

###四、material-dialogs ###
 **MaterialDialog：一个漂亮、流畅、可定制的对话框。**   
#### 示例：  ####

                 
<div align=left ><img src="https://img-blog.csdn.net/2018072709451964?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NjUyNzI2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70"/></div>
 <br/>  
<div align=right ><img src="https://img-blog.csdn.net/20180727104018379?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NjUyNzI2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70"/></div>
	
####官网：    [afollestad/material-dialogs](https://github.com/afollestad/material-dialogs)
#### 使用 ：[https://github.com/afollestad/material-dialogs/blob/master/documentation/CORE.md](https://github.com/afollestad/material-dialogs/blob/master/documentation/CORE.md)   

#### 依赖：   
**注意：Activity 中使用该功能需要 Activity 继承 AppCompat 主题才能正确使用此库。**  
  
- 核心模块的依赖：    
	       implementation 'com.afollestad.material-dialogs:core:2.6.0'                       
		    核心模块包含该库的所有主要类，包括 MaterialDialog。 您可以使用核心创建基本、列表、单/多选项、进度、输入等对话框。

- 公共模块的依赖：      
	compile 'com.afollestad.material-dialogs:commons:0.9.6.0'   
	新版本已经细化各个库的引入，不需要的库不需要连带引入，降低开发负载。 
 
        公共模块包含不是每个人都需要的扩展库。             
        这包括 ColorChooserDialog、FolderChooserDialog、Material Preference类        
		和 MaterialSimpleListAdapter / MaterialSimpleListItem。
	> **Input:**输入模块包含对核心模块的扩展，例如文本输入对话框：
	
	        dependencies {
  					...
  					implementation 'com.afollestad.material-dialogs:input:2.6.0'
			}    

	> **Files**:文件模块包括扩展到核心模块，例如文件和折叠选择。 
	 
			dependencies {
			  ...
			  implementation 'com.afollestad.material-dialogs:files:2.6.0'
			}

    
	> **Color**：颜色模块包含对核心模块的扩展，例如颜色选择器。    
		
			dependencies {
			  ...
			  implementation 'com.afollestad.material-dialogs:color:2.6.0'
			}


	> **DateTime**：日期模块包含用于创建日期、时间和日期时间选择器对话框的扩展。
	
			dependencies {
			  ...
			  implementation 'com.afollestad.material-dialogs:datetime:2.6.0'
			}
	
	
	           
	> **Lifecycle**：Lifecycle模块包含使对话框与Androidx生命周期一起工作的扩展。    
	 
			dependencies {
			  ...
			  implementation 'com.afollestad.material-dialogs:lifecycle:2.6.0'
			}














###五、网络业务一条龙 ###
	Retrofit2.0+rxjava2+rxandroid+MVP+MeterialDesign+dagger2
	这些业务链本人也未吃透，就不做展示，自行挖掘。

###六、permission ###
**运行时权限** 
   
####官网： [  https://github.com/yanzhenjie/AndPermission](https://github.com/yanzhenjie/AndPermission)
#### 使用 ：[https://blog.csdn.net/yanzhenjie1003/article/details/52503533](https://blog.csdn.net/yanzhenjie1003/article/details/52503533)
  
 - 在旧的权限管理系统中，权限仅仅在App安装时询问用户一次，用户同意了这些权限App才能被安装（某些深度定制系统另说），App一旦安装后就可以偷偷的做一些不为人知的事情了。     
 - 在Android6.0开始，App可以直接安装，App在运行时一个一个询问用户授予权限，系统会弹出一个对话框让用户选择是否授权某个权限给App（这个Dialog不能由开发者定制），当App需要用户授予不恰当的权限的时候，用户可以拒绝，用户也可以在设置页面对每个App的权限进行管理。     
 - 特别注意：这个对话框不是开发者调用某个权限的功能时由系统自动弹出，而是需要开发者手动调用，如果你直接调用而没有去申请权限的话，将会导致App崩溃。         
- 这对于用户来说是好事，但是对于开发者来说我们不能直接调用方法了，我们不得不在每一个需要权限的地方检查并请求用户授权，所以就引出了以下两个问题：

			不需要运行时申请的权限
			需要运行时申请的权限

###七、BUGLY ###

需要官网注册开发者并申请appkey     

文档中心：[https://bugly.qq.com/docs/user-guide/instruction-manual-android/?v=20180709165613](https://bugly.qq.com/docs/user-guide/instruction-manual-android/?v=20180709165613)

> Bugly 是腾讯公司为移动开发者开放的服务之一,这里主要指 Crash 监控、崩溃分析等质量跟踪服务。     
> 异常上报（异常概览、崩溃分析、卡顿分析、高级搜索、异常配置）、运营统计（运营概览、用户分析、渠道分析）、应用升级等     
 
![](https://img-blog.csdn.net/20180323154037114?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTMwMDczMDU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<br/> <br/>

 


###八、banner ###
通常我们会接触比较多的为[youth5201314/banner](https://github.com/youth5201314/banner)和[bingoogolapple/BGABanner-Android](https://github.com/bingoogolapple/BGABanner-Android)

两个库对于banner功能性扩展的很好，包括图片加载，缓存等，对于定制轮播区域宽度高度，以及圆角等图片展示特性，这里首选[youth5201314/banner](https://github.com/youth5201314/banner)，同时[youth5201314/banner](https://github.com/youth5201314/banner)对于自定义指示器有了更大空间的定制性，当然在现实开发过程中，我们完全可以自己去定义banner轮播的规则，利用Viewpager+fragment+imageView+viewgroup动态添加指示器等来实现轮播。对于当前公司项目，二者功能上皆可适用。

|     |**YouthBanner**|**BGABanner**
|---|---|
|**文档**|[https://github.com/youth5201314/banner](https://github.com/youth5201314/banner "https://github.com/youth5201314/banner")|[https://github.com/bingoogolapple/BGABanner-Android](https://github.com/bingoogolapple/BGABanner-Android "https://github.com/bingoogolapple/BGABanner-Android")|
|功能对比|*  灵活控制显示效果，多种配置常量参数 <br/>*  多种轮播动画效果支持选择|* 引导界面导航效果<br/>* 支持根据服务端返回的数据动态设置广告条的总页数<br/>* 支持大于等于1页时的无限循环自动轮播、手指按下暂停轮播、抬起手指开始轮播<br/>* 支持自定义指示器位置和广告文案位置<br/>支持图片指示器和数字指示器<br/>* 支持 ViewPager 各种切换动画<br/>* 支持选中特定页面<br/>* 支持监听 item 点击事件<br/>* 加载网络数据时支持占位图设置，避免出现整个广告条空白的情况<br/>* 多个 ViewPager 跟随滚动 |
|**添加依赖**|Gradle<br/>dependencies{<br/>compile 'com.youth.banner:banner:1.4.10'  //最新版本<br/>}<br/>或者引用本地lib <br/>compileproject(':banner')|<br/>dependencies {<br/>implementation 'com.android.support:support-v4:latestVersion'<br/>implementation 'cn.bingoogolapple:bga-banner:latestVersion@aar'<br/>}|
|**配置数据源**|[配置说明](https://github.com/youth5201314/banner)|[配置说明](https://github.com/bingoogolapple/BGABanner-Android "配置说明")|
|**效果图**|普通模式<br/><br/>![普通模式](https://img-blog.csdn.net/20170320160730367?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGFudGh1c19saQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) <br/><br/> *[数字模式](https://camo.githubusercontent.com/078504c5723b59c8ebe787a059853fa1a603a381/687474703a2f2f6f63656835316b6b752e626b742e636c6f7564646e2e636f6d2f62616e6e65725f6578616d706c65322e706e67)* <br/><br/>![数字模式](https://camo.githubusercontent.com/078504c5723b59c8ebe787a059853fa1a603a381/687474703a2f2f6f63656835316b6b752e626b742e636c6f7564646e2e636f6d2f62616e6e65725f6578616d706c65322e706e67)  <br/><br/>[数字加标题模式](https://camo.githubusercontent.com/fa591b0ea9768e3722fcd690cc97f987867573d9/687474703a2f2f6f63656835316b6b752e626b742e636c6f7564646e2e636f6d2f62616e6e65725f6578616d706c65332e706e67)  ![数字加标题模式](https://camo.githubusercontent.com/fa591b0ea9768e3722fcd690cc97f987867573d9/687474703a2f2f6f63656835316b6b752e626b742e636c6f7564646e2e636f6d2f62616e6e65725f6578616d706c65332e706e67) <br/> <br/>等，其他效果见文档示例demo |![效果示例](https://cloud.githubusercontent.com/assets/8949716/17557718/dc235ec4-5f4a-11e6-92b7-144a2a1a1e3f.gif)|
|**注意**|最新更新到com.youth.banner1.4.10  加入了自定义布局功能，自定义布局的文件不能命名成banner.xml 。因为没有使用自定义布局的控件会自动使用这个布局文件，原因是这个轮播图框架里面的布局文件也是banner.xml命名||
<br/> <br/>




###九、SystemBarTint 沉浸式 ###

> Android 4.4(KitKat)介绍了半透明的系统UI样式地位和导航栏。 
		这些风格的壁纸的基础活动,比如主屏幕发射器,但提供的最低背景保护使他们更有用的其他类型的活动,除非你提供自己的背景在你的 这个库提供了一个简单的方法来创建一个背景系统的“色彩”酒吧,无论是颜色值或可移动。 默认情况下它会给你一种半透明的黑色背景,将用于全出血屏幕持续系统UI内容仍然是重要的——比如当放置在地图或照片网格。 添加三方类库

![](https://camo.githubusercontent.com/fbbeaab2048f78e2d4974bb1559544c9f22eccae/68747470733a2f2f7261772e6769746875622e636f6d2f6a67696c66656c742f53797374656d42617254696e742f6d61737465722f73637265656e73686f742e706e67)

文档中心：[https://blog.csdn.net/weixin_38323279/article/details/79876199](https://blog.csdn.net/weixin_38323279/article/details/79876199)    
官方地址：[https://github.com/jgilfelt/SystemBarTint](https://github.com/jgilfelt/SystemBarTint)    

推荐文章：[使用SystemBarTint搭配NavigationView在Android4.4以上实现透明状态栏](https://www.jianshu.com/p/005c3d45fce3)



###十、分包方案 ###
方法数越界的解决方案   
[https://www.cnblogs.com/chenxibobo/p/6076459.html](https://www.cnblogs.com/chenxibobo/p/6076459.html)

com.android.support:multidex:1.0.1   
文档中心：[https://github.com/TangXiaoLv/Android-Easy-MultiDex](https://github.com/TangXiaoLv/Android-Easy-MultiDex)


###十一、libzxing_light ###

>	目前公司有抽取简洁化的Zxing框架，满足扫描识别，弱光无光情况下的手动开启灯光功能。    
>	若是后期功能性要求比较高，加入图库二维码识别，条形码识别，远景近拉等功能，    
>	可以参考      
>	[https://github.com/zxing/zxing](https://github.com/zxing/zxing)   
>	当然如上zxing扩展性比较繁冗余，可以结合如下文章进行集成实现 （此种抽取方案库，未真正去研究稳定性，请自行抉择）      
>	[https://github.com/yipianfengye/android-zxingLibrary](https://github.com/yipianfengye/android-zxingLibrary)



###十二、BaseRecyclerViewAdapterHelper    BaseQuickAdapter###
研究之前，不如下个demo apk点吧点吧几下看效果再对焦细节。
<br/>
<br/>
<div align=center><img  src="https://pro-icon-qn.fir.im/fd3a1c6ec31fcb5b8e5cbb6c4a077b84aa96eb90?e=1554367134&token=LOvmia8oXF4xnLh0IdH05XMYpH6ENHNpARlmPc-T:nGuWCGeL9vGrxjiYdxJHdHQjJCw="/></div>[点击下载apk](https://fir.im/s91g)

> RecyclerView是Android L版本中新添加的一个用于取代ListView的SDK，具有灵活性和可替代性比ListView更好，RecyclerView同样也用到适配，作为开发者，我们希望有一款通用的适配。     
BaseQuickAdapter 就是一款为Android开发者打造的针对繁琐的适配器的构建的快速开源项目。

文档中心：[https://www.jianshu.com/p/b343fcff51b0](https://www.jianshu.com/p/b343fcff51b0)   
官方网站：[www.recyclerview.org](www.recyclerview.org)

<div align=center><img src="https://raw.githubusercontent.com/chaychan/TouTiaoPics/master/screenshot/home.jpg"/ width="300"></div>

> 像今日头条这种界面多布局的情况很适合使用此列表适配框架。

### 为什么使用
#### BaseQuickAdapter 特点 ####

- 可以减少重复代码
- 添加Item点击事件，长按事件以及子控件的点击事件
- 添加头部、尾部、下拉刷新、上拉加载涵盖多种动画处理，没有更多数据处理
- 添加Item滑动动画。多种动画切换
- 添加新增删除Item动画
- 网格、列表、流式布局随意切换
- 添加空布局(列表无数据时，显示更加友好)
- 拖拽和侧滑删除
- 支持多类型布局
- 字母导航

#### 功能
[源码](https://github.com/HpWens/WRecyclerView)    
	结合RecyclerView使用 
  
* Item点击，长按事件
* 子控件的点击事件
* 添加头部，尾部
* 添加 Item动画
* 设置加载更多
* 添加空布局
* 拖拽和侧滑
* 支持不同类型(多类型布局)
* 流式布局
* 列表切换（九宫格布局与线性列表动态切换）
* 单选

> 当然在RecylerView列表对于item进行标记性UI交互时，会发生错位，这是由于内部复用机制导致。     
> 此万能适配框架，展示的进行选中与非选中id留存进行局部刷新或全文刷新操作，具体请参考demo示例。   


#### BaseQuickAdapter使用  
 --------> [https://www.jianshu.com/p/40457c16e44a](https://www.jianshu.com/p/40457c16e44a)


--------------------------------------
<br/>  
###十三、Arouter ###
文档中心：[https://github.com/alibaba/ARouter](https://github.com/alibaba/ARouter "https://github.com/alibaba/ARouter")

>    一个用于帮助 Android App 进行组件化改造的框架 —— 支持模块间的路由、通信、解耦

**衍生**    

<pre>
我们所使用的原生路由方案一般是通过显式intent和隐式intent两种方式实现的（这里主要是指跳转Activity orFragment）。
在显式intent的情况下，因为会存在直接的类依赖的问题，导致耦合非常严重；而在隐式intent情况下，则会出现规则集中式管理，
导致协作变得非常困难。一般而言配置规则都是在Manifest中的，这就导致了扩展性较差。除此之外，   
使用原生的路由方案会出现跳转过程无法控制的问题，因为一旦使用了StartActivity()就无法插手其中任何环节了，只能交给系统管理，
这就导致了在跳转失败的情况下无法降级，而是会直接抛出运营级的异常。这时候如果考虑使用自定义的路由组件就可以解决以上问题，
比如通过URL索引就可以解决类依赖的问题；通过分布式管理页面配置可以解决隐式intent中集中式管理Path的问题；
自己实现整个路由过程也可以拥有良好的扩展性，还可以通过AOP的方式解决跳转过程无法控制的问题，
与此同时也能够提供非常灵活的降级方式。
</pre>



 
**1.功能介绍**

- 支持直接解析标准URL进行跳转，并自动注入参数到目标页面中
- 支持多模块工程使用
- 支持添加多个拦截器，自定义拦截顺序
- 支持依赖注入，可单独作为依赖注入框架使用
- 支持InstantRun
- 支持MultiDex(Google方案)
- 映射关系按组分类、多级管理，按需初始化
- 支持用户指定全局降级与局部降级策略
- 页面、拦截器、服务等组件均自动注册到框架
- 支持多种方式配置转场动画
- 支持获取Fragment
- 完全支持Kotlin以及混编(配置见文末 其他#5)
- 支持第三方 App 加固(使用 arouter-register 实现自动注册)
- 支持生成路由文档
- 提供 IDE 插件便捷的关联路径和目标类


**2.典型应用**
    
- 从外部URL映射到内部页面，以及参数传递与解析
- 跨模块页面跳转，模块间解耦
- 拦截跳转过程，处理登陆、埋点等逻辑
- 跨模块API调用，通过控制反转来做组件解耦
<br/>  <br/>  
###十四、阿里列表多布局框架 ###
阿里针对布局方案和布局复用的开源框架（VirtualLayout）    
文档中心：[https://www.jianshu.com/p/067abf64ed40](https://www.jianshu.com/p/067abf64ed40 "https://www.jianshu.com/p/067abf64ed40")        
官方地址：[https://github.com/alibaba/vlayout](https://github.com/alibaba/vlayout)    

###十五、Tencent/QMUI_Android ###
腾讯提高 Android UI 开发效率的 UI 库   

[<div align=left ><img src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBzdGFuZGFsb25lPSJubyI/PjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+PHN2ZyB0PSIxNTU0MzQ3MTUxNzM3IiBjbGFzcz0iaWNvbiIgc3R5bGU9IiIgdmlld0JveD0iMCAwIDEwMjQgMTAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHAtaWQ9IjU2OSIgZGF0YS1zcG0tYW5jaG9yLWlkPSJhMzEzeC43NzgxMDY5LjAuaTMiIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB3aWR0aD0iMTUwIiBoZWlnaHQ9IjE1MCI+PGRlZnM+PHN0eWxlIHR5cGU9InRleHQvY3NzIj48L3N0eWxlPjwvZGVmcz48cGF0aCBkPSJNNTE0LjA0OCA0MTcuNzkyYzguMTkyIDguMTkyIDguMTkyIDIwLjQ4IDAgMjguNjcyLTguMTkyIDguMTkyLTIwLjQ4IDguMTkyLTI4LjY3MiAwLTQ5LjE1Mi00OS4xNTItNDAuOTYtMTMzLjEyIDE0LjMzNi0xODguNDE2bDg2LjAxNi04Ni4wMTZjNzEuNjgtNzEuNjggMTg4LjQxNi03MS42OCAyNjAuMDk2IDBzNzEuNjggMTg4LjQxNiAwIDI2MC4wOTZsLTg2LjAxNiA4Ni4wMTZjLTUzLjI0OCA1My4yNDgtMTQzLjM2IDU5LjM5Mi0xODguNDE2IDE0LjMzNi04LjE5Mi04LjE5Mi04LjE5Mi0yMC40OCAwLTI4LjY3MnMyMC40OC04LjE5MiAyOC42NzIgMGMyNi42MjQgMjYuNjI0IDkyLjE2IDIyLjUyOCAxMzEuMDcyLTE0LjMzNmw4Ni4wMTYtODYuMDE2YzU1LjI5Ni01NS4yOTYgNTUuMjk2LTE0Ny40NTYgMC0yMDIuNzUycy0xNDcuNDU2LTU1LjI5Ni0yMDIuNzUyIDBsLTg2LjAxNiA4Ni4wMTZjLTQwLjk2IDQwLjk2LTQ1LjA1NiAxMDAuMzUyLTE0LjMzNiAxMzEuMDcyeiBtMTIyLjg4LTY1LjUzNmM4LjE5Mi04LjE5MiAyMC40OC04LjE5MiAyOC42NzIgMCA4LjE5MiA4LjE5MiA4LjE5MiAyMC40OCAwIDI4LjY3MmwtMjg4Ljc2OCAyODguNzY4Yy04LjE5MiA4LjE5Mi0yMC40OCA4LjE5Mi0yOC42NzIgMC04LjE5Mi04LjE5Mi04LjE5Mi0yMC40OCAwLTI4LjY3MmwyODguNzY4LTI4OC43Njh6IG0tMTk0LjU2IDEzNy4yMTZjOC4xOTIgOC4xOTIgOC4xOTIgMjAuNDggMCAyOC42NzJzLTIwLjQ4IDguMTkyLTI4LjY3MiAwYy0zMC43Mi0zMC43Mi05MC4xMTItMjYuNjI0LTEzMS4wNzIgMTQuMzM2bC04Ni4wMTYgODYuMDE2Yy01NS4yOTYgNTUuMjk2LTU1LjI5NiAxNDcuNDU2IDAgMjAyLjc1MnMxNDcuNDU2IDU1LjI5NiAyMDIuNzUyIDBsODYuMDE2LTg2LjAxNmMzNi44NjQtMzYuODY0IDQwLjk2LTEwMi40IDE0LjMzNi0xMzEuMDcyLTguMTkyLTguMTkyLTguMTkyLTIwLjQ4IDAtMjguNjcyIDguMTkyLTguMTkyIDIwLjQ4LTguMTkyIDI4LjY3MiAwIDQ1LjA1NiA0NS4wNTYgMzguOTEyIDEzNS4xNjgtMTQuMzM2IDE4OC40MTZsLTg2LjAxNiA4Ni4wMTZjLTcxLjY4IDcxLjY4LTE4OC40MTYgNzEuNjgtMjYwLjA5NiAwcy03MS42OC0xODguNDE2IDAtMjYwLjA5Nmw4Ni4wMTYtODYuMDE2YzU1LjI5Ni01NS4yOTYgMTM5LjI2NC02MS40NCAxODguNDE2LTE0LjMzNnoiIHAtaWQ9IjU3MCIgZGF0YS1zcG0tYW5jaG9yLWlkPSJhMzEzeC43NzgxMDY5LjAuaTEiPjwvcGF0aD48L3N2Zz4=" width="20" height="20"/></div>](https://github.com/Tencent/QMUI_Android)

<div align=center ><img src="https://cloud.githubusercontent.com/assets/1190261/26751376/63f96538-486a-11e7-81cf-5bc83a945207.png" width="100" height="100"/></div>



文档中心：[https://github.com/Tencent/QMUI_Android](https://github.com/Tencent/QMUI_Android)    
官方地址：[https://qmuiteam.com/android](https://qmuiteam.com/android)

### 功能特性
----------------------------
**全局 UI 配置**     
只需要修改一份配置表就可以调整 App 的全局样式，包括组件颜色、导航栏、对话框、列表等。一处修改，全局生效。

**丰富的 UI 控件**     
提供丰富常用的 UI 控件，例如 BottomSheet、Tab、圆角 ImageView、下拉刷新等，使用方便灵活，并且支持自定义控件的样式。

**高效的工具方法**     
提供高效的工具方法，包括设备信息、屏幕信息、键盘管理、状态栏管理等，可以解决各种常见场景并大幅度提升开发效率。

**功能列表**     
请查看官网的[功能列表](http://qmuiteam.com/android/page/document.html)

**支持 Android 版本**   
QMUI Android 支持 API Level 14+。

**使用方法**   
请查看官网的[开始使用](http://qmuiteam.com/android/page/start.html)。

### QMUI Demo APP 安装包下载
点击链接下载：[http://cdn.qmuiteam.com/download/android/latest](http://cdn.qmuiteam.com/download/android/latest)

或扫二维码至官网下载：

![QMUI Website](https://camo.githubusercontent.com/2c20e89961909a029b6f534fd5df58cde9aed0c5/687474703a2f2f716d75697465616d2e636f6d2f7468656d65732f716d75692f7075626c69632f7374796c652f696d616765732f696e646570656e64656e742f416e64726f6964446f776e6c6f61645152436f64655f32782e706e67)

注：项目中使用最好不要整体引入，可以根据需要进行组件化引入。
	

-------------------------------------


## 注 ##
安卓原生是个庞大工程，层出不穷的开放性方案，让我从无到有，现在却面对着如何甄选适合的，稳定的，扩展性高的第三方支持库，所以良性使用引入库，才能让项目被侵入性的可能性越低，当然百家之言百家之所在。
我们时刻谨记着，别人的成果我们不能做拿来主义原封不动的据为己有，知识驱动进步靠每个行业人的贡献与不竭追求

> 优秀第三方开源推荐：     

[https://blog.csdn.net/qq_42618969/article/details/81941242](https://blog.csdn.net/qq_42618969/article/details/81941242 "https://blog.csdn.net/qq_42618969/article/details/81941242")

![](https://img-blog.csdn.net/20170428144103234?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYXhpMjk1MzA5MDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



###开发者常用网站 ###

-   GDG: [https://chinagdg.org/resource-list/](https://chinagdg.org/resource-list/)
- 	Androidstudio集聚地：[http://www.android-studio.org/](http://www.android-studio.org/)
- 	https:[//www.androiddevtools.cn/](http://www.android-studio.org/)
- 	Google开发者官网（大部支持中文）：[https://developer.android.google.cn/](http://www.android-studio.org/)
- 	Android 开发者工具：
	> [https://techbeacon.com/app-dev-testing/code-pro-31-tools-android-app-developers](https://techbeacon.com/app-dev-testing/code-pro-31-tools-android-app-developers)
	> [https://blog.csdn.net/jia635/article/details/80238842](https://blog.csdn.net/jia635/article/details/80238842)
- 	Androidstudio插件：[https://blog.csdn.net/jia635/article/details/78811892](https://blog.csdn.net/jia635/article/details/78811892)
- 	开发者同性交友社区：[https://github.com/](https://github.com/)
	> 其中在国内由于某些不可描述的原因，Github访问下载不是太顺畅，请参阅我的博客如何提速GitHub的访问力：[https://blog.csdn.net/Maiduoudo/article/details/81033898](https://blog.csdn.net/Maiduoudo/article/details/81033898)
- 	JSON快速格式化网站：
	> [ https://www.json.cn/]( https://www.json.cn/)    
	> [http://www.bejson.com/](http://www.bejson.com/)
- 	[圆角图标 / 透明圆角图片工具](http://www.atool88.com/roundcorner.php)
- 	进制转换：[https://tool.lu/hexconvert/](https://tool.lu/hexconvert/)
- 	美洽客服云：[https://meiqia.com/](https://meiqia.com/)
- 	Bugly: [https://bugly.qq.com/v2/](https://bugly.qq.com/v2/)
- 	Jpush:[ https://www.jiguang.cn/]( https://www.jiguang.cn/)
- 	Umeng: [https://www.umeng.com/](https://www.umeng.com/)
- 	信鸽：
 	
	[https://www.umeng.com/push?utm_source=bdsem&utm_medium=search&utm_campaign=push](https://www.umeng.com/push?utm_source=bdsem&utm_medium=search&utm_campaign=push)

- 	二维码在线工具：[https://cli.im/deqr](https://cli.im/deqr)
- 	Android组件网站：[http://www.android-doc.com/guide/components/index.html](http://www.android-doc.com/guide/components/index.html)
- 	制作透明底色图片（前提是不想用PS的话）：[http://www.aigei.com/bgremover/](http://www.aigei.com/bgremover/)
- 	Ezgif (Video-Gif): [https://ezgif.com/video-to-gif](https://ezgif.com/video-to-gif)
- 	fir.im-免费app托管及内测（1）：[https://fir.im/](https://fir.im/)
- 	蒲公英-免费app托管及内测（2）：[https://www.pgyer.com/](https://www.pgyer.com/)
- 	Runoob: [http://www.runoob.com/](http://www.runoob.com/)
- 	MD5在线加密解密：[https://cmd5.com/](https://cmd5.com/)	
- 	其他等，个人整理。
## 开发软件辅助工具 ##

-     1.Hijson v2.1.2： 本地json格式化分析工具，支持树结构
-     2.MarkdownPad ：md格式文档编辑
-     3.Notepad++：高级记事本
-     4.EditPlus:高级记事本  https://www.editplus.com/
-     5.PowerDesigner：比较全面的数据库设计工具
-     6.HBuilder:支持HTML5的Web开发IDE。它基于Eclipse，所以顺其自然地兼容了Eclipse的插件
-     7.ColorPix:在线取色器  https://colorpix.en.softonic.com/
-     8.Httpwatch:网页数据抓包软件。httpwatch可集成于ie浏览器和firefox浏览器，软件支持显示网页同时显示网页请求和回应的日志信息。甚至可以显示浏览器缓存和IE之间的交换信息。


## 暴露一些平时常用的配置：(这些请依照Google官方配置进行操作) ##

1. 资源res衍生多文件夹配置
1. so引入配置
1. jar引入配置
1. Ndk引入配置
1. module引入声明配置
2. maven
1. Androidstudio 3.0版本/3.0+ aar包打包生成的不同配置
2. git
3. svn
> 这里不再赘述，请自行面向百度,面向谷歌，面向高级程序猿


## 推荐书籍 ##
《三字经》
## 颜色取色参考表   
[颜色定义、设置、转换、拾取](https://www.jianshu.com/p/3c1fe10aed4f)    
[android颜色](https://www.jianshu.com/p/9867d64b9d2f)

> **为什么要把颜色单拎出来，因为UI设计黄金比例以及审美度，颜色是不可或缺的，如何合理运用颜色为应用添足料，就要学会运用色彩，
拒绝花里胡哨，也拒绝平庸无奇，只要刚刚好。**

<br/><br/>
<br/>
<br/><br/><br/>
















