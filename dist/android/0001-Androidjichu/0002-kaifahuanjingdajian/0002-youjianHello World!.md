至此，我们终于算是完全算是双脚步入Android开发的大门了，但我们现在还只能算是金字塔底端的那群人，还需要不断地实践、实践、再实践。而且上面所讲的是作为一个真正Android开发人员必须要深刻理解的东西，如果您还没有达到深刻的程度那请你回去再浏览一遍，然后跟着我的这个系列继续深入学习，在接下来的文章我将更多的是利用实例来解析这些东西。下面我再次用Hello World程序来分析一下Android应用程序，主要内容如下：
```  
“Hello World！”显示浅析
“Hello World！”的手术（一）
“Hello World！”的手术（二）
“Hello World！”的手术（三）
```
#### 1、“Hello World！”浅析
首先我们再次简单地新建一个Hello World项目，它的布局文件res\layout\main.xml的代码如下：
```   
<?xml version="1.0" encoding="utf-8"?> 
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<TextView  
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="@string/hello"
    />
</LinearLayout> 
```
元素<TextView>的android:text就是我们在屏幕上显示的“Hello World, HelloWorld！”，android:text的值是“@string/hello”，它是如何在屏幕上显示“Hello World, HelloWorld！” 的呢？。
在main.xml文件中以这种格式： 
```  
@[package:]string/some_name (where some_name  is the name of a specific string) 
```
引用res/values/strings.xml文件中的字符串，其中some_name是要引用的字符串的名字。strings.xml文件代码如下： 
```  
<?xml version="1.0" encoding="utf-8"?> 
<resources> 
    <string name="hello">Hello World, HelloWorld!</string>
    <string name="app_name">HelloWorld</string>
</resources> 
```
由此可见，上面那个<TextView>的android:text引用的字符串是“Hello World, HelloWorld!”。接着想象一下，“Hello World, HelloWorld！”何时加载显示的呢？
Note：这种<TextView>的text的值是存储在strings.xml中的，而不是硬编码的。这样的好处是，当我们在strings.xml中修改hello的具体值时，不需要在main.xml中修改。
我们来看下src目录下skynet.com.cnblogs.www包（就是新建Hello World项目时定义的包名）中的HelloWorld.java文件，代码如下： 
```  
import android.app.Activity;
import android.os.Bundle;
public class HelloWorld extends Activity {
	 /** Called when the activity is first created. */
	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.main);
	}
}
```
可以看出在创建活动时，即void onCreate(Bundle saveInstanceState)中，在HelloWorld中重写了它，在该方法中首先调用了父类的onCreate方法且接着调用了setContentView(R.layout.main)方法。是的，就是在这个方法中，根据main.xml文件将其显示出来，因为R.layout.main是表示布局资源文件main.xml编译后的对象，Eclipse插件会自动在R.java文件中创建这个引用。main.xml中定义了<TextView>，然后根据它的android:text属性引用到strings.xml文件中的<string name="hello">Hello World, HelloWorld!</string>元素，然后将它显示到屏幕上。
从活动的生命周期可以知道，任何一个活动启动的一个方法是onCreate()方法，在这里做一些初始化的工作，诸如创建视图、绑定数据列表等。在HelloWorld项目中，就是调用setContentView进行初始化工作，将Hello World, HelloWorld!显示到屏幕上。
至此，我们对Hello World的认识更加深入了一点！下面我们让拿起手术刀对它进行一个手术。
#### 2、Hello World的手术（一）
我们将main.xml文件中的<TextView>元素删掉，取而代之用一个Button来显示“Hello World！”。代码如下： 
```  
<?xml version="1.0" encoding="utf-8"?> 
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<Button 
	android:layout_width="fill_parent" 
    android:layout_height="fill_parent"
    android:text="@string/hello"
    />
</LinearLayout> 
```
其它的地方不用修改，直接运行即可！

![img](http://emanual.github.io/md-android/img/basic_env/03_helloworld.jpg)  
![img](http://emanual.github.io/md-android/img/basic_env/03_helloworld2.jpg)  

从这个例子我们可以看出我们上节的分析是对的，接下来我们继续对它做一个手术。
#### 3、Hello World的手术（二）
我们这次不用Button来显示“Hello World！”，我们这次用一个图片来显示。首先我们准备一张png的图片，如下：

![img](http://emanual.github.io/md-android/img/basic_env/03_helloworld3.jpg)  

接下来我们将它复制到， res/drawable-dhpi中。然后我们查看R.java文件我们会发现，R中自动加了一行：（关于R文件，你可以参考Android 开发之旅：HelloWorld项目的目录结构中的1.2、gen文件夹） 
```  
/* AUTO-GENERATED FILE.  DO NOT MODIFY. 
  * 
  * This class was automatically generated by the 
  * aapt tool from the resource data it found.  It 
  * should not be modified by hand. 
  */
public final class R {
	public static final class attr {
	}
	public static final class drawable {
		public static final int hello = 0x7f020000;// 这行是Android自动新加的
		public static final int icon = 0x7f020001;
	}
	public static final class layout {
		public static final int main = 0x7f030000;
	}
	public static final class string {
		public static final int app_name = 0x7f040001;
		public static final int hello = 0x7f040000;
	}
}
```
drawable- hdpi、drawable- mdpi、drawable-ldpi的区别：
(1)drawable-hdpi里面存放高分辨率的图片,如WVGA (480x800),FWVGA (480x854)
(2)drawable-mdpi里面存放中等分辨率的图片,如HVGA (320x480)
(3)drawable-ldpi里面存放低分辨率的图片,如QVGA (240x320)
系统会根据机器的分辨率来分别到这几个文件夹里面去找对应的图片。
删除刚才我们添加的<Button>元素，添加一个<ImageView>元素。
```   
<?xml version="1.0" encoding="utf-8"?> 
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
<ImageView 
	android:id="@+id/imageview"
	android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/hello"
/> 
</LinearLayout> 
```
运行之后，结果如下所示：

![img](http://emanual.github.io/md-android/img/basic_env/03_helloworld4.jpg)  
 
我们看到上面的ImageView有一个id属性。其实，每个View对象都有一个关联的ID，来唯一标识它。当应用程序被编译时，这个ID作为一个整数引用。但是ID通常是在布局XML文件中作为字符串分配的，在元素的id属性。这个XML属性对所有的View对象可用且会经常用到。XML中的ID语法如下：android:id="@+id/my_button"。
字符串前的@符号表示XML解析器应该解析和扩展剩下的ID字符串，并把它作为ID资源。+符号表示这是一个新的资源名字，它必须被创建且加入到我们的资源（R.java文件，R是Resource）。Android框架提供一些其他的ID资源。当引用一个Android资源ID时，你不需要+符号，但是你必须添加android包名字空间，如下：android:id="@android:id/empty"。
#### 4、Hello World的手术（三）
上面两个手术都是基于main.xml的，在布局资源文件中声明相应的控件并设置相关属性，如TextView、Button、ImageView等，然后在onCreate()方法中调用setContentView(R.layout.main)方法来输出。其实上面的几个实验我们都可以编程地进行，下面就以编程地用ImageView显示“Hello World！”，别的也是类似来完成。
首先，我们将main.xml文件中的<ImageView>删掉，且将res/values/strings.xml中的<string name="hello">Hello World, HelloWorld!</string>删掉（其实第3节，用图片显示就没有用到它）；然后，在HelloWorld.java文件中 import android.widget.ImageView;且删掉onCreate()方法中调用setContentView(R.layout.main)方法，因为我们不需要根据main.xml布局资源文件来显示而是编程地Hello World。
接下来我们在onCreate()方法中声明一个ImageView对象，并设置它的图像资源，最后将这个ImageView对象传给setContentView()方法，代码如下： 
```  
import android.app.Activity;
import android.os.Bundle;
import android.widget.ImageView;
public class HelloWorld extends Activity {
	 /** Called when the activity is first created. */
	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		ImageView imageHello = new ImageView(this);
		imageHello.setImageResource(R.drawable.hello);
		setContentView(imageHello);
	}
}
```
运行之后，同样会得到Hello World的手术（二）的结果，只是图片的显示位置不太一样。在这里注意上面那行粗体代码，如果是TextView或者Button的话，应该调用setText()方法来设置”Hello World!”。


![img](http://emanual.github.io/md-android/img/basic_env/03_helloworld5.jpg)  

Note：构建ImageView对象时传递了一个this参数，表示与当前上下文（context）关联。这个Context由系统处理，它提供诸如资源解析、获取访问数据库和偏好等服务。因为Activity类继承自Context，且因为你的HelloWorld类是Activity的子类，它也是一个Context。因此，你可以传递this作为你的Context给ImageView引用。