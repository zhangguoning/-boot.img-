## 修改Android  boot.img 文件以获取所有应用的永久debug权限
前提：手机已ROOT、安装了busyBox（最好安装，不是必须）

众所周知，一个安装在手机的 release 的 app 想要可以调试，必须在其 AndroidManifest 中设置 android:debuggable="true",这样就开启了这个应用的debug权限，但是这就需要反编译->修改->重新打包->重新签名->安装 是比较麻烦的，而且如果你是反编译别人的应用， 那在这个过程中你未必会一帆风顺，那么有没有更简单的办法呢？答案当然是肯定的！
			 
       1. adb shell // 进入Android  shell 模式
       2. su // 切换至root用户
       3. cat default.prop // 查看系统配置属性
如果 ro.debuggable=1 则表示这个手机上的所有应用都可以被调试。下面就说说如何修改这个值（因为这个值不能直接修改，即使你修改了重启之后也会还原的）。
	
       1. 找到你刷机的时候用的刷机包ROM，一般是zip格式。然后直接解压了，拿到 boot.img 文件，这个文件其实就是启动时候的引导文件，
       就是它会把 / default.prop 的值还原的
       2.  http://forum.xda-developers.com/showthread.php?t=2073775  下载Android.Image.Kitchen.v3.2-Win32.zip ，这个工具
       可以把boot.img 解包、重打包。
       3.  使用Android.Image.Kitchen 解包 boot.img ，进入解包后的文件目录，打开 default.prop 修改 ro.debuggable=1 
       4.  使用Android.Image.Kitchen 重新打包boot.img ,然后把你新打包出来的boot.img 丢进ROM中替换之前的那个
       5.  重新压缩ROM为zip文件
       6.  对ROM签名：这个可以自己使用命令签名， 也可以使用别人做好的已工具（蘑菇Rom助手、autosign 等都行）
       7.  好了，使用上面重新打包好的ROM再刷一次机吧


其他： 其实从上面的方式可以看出我们只是修改了boot.img ,但是需要重新刷机才行， 那有没有不重新刷机就能让boot.img生效的呢？也是有的，完成上面第 4 步之后我们就得到了一个修改后的boot.img，直接重启到 recovery 模式下，直接刷入这个boot.img 也可以，但是会有一个副作用（至少在我的手机上是这样）：没有ROOT 权限了，目前还没有解决这个问题
