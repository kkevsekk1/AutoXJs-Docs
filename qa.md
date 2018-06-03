# Q & A

## 如何定时运行脚本

点击脚本右边的菜单按钮->更多->定时任务即可定时运行脚本，但是必须保持Auto.js后台运行(自启动白名单、电源管理白名单等)。同时，可以在脚本的开头使用`device.wakeUp()`来唤醒屏幕；但是，Auto.js没有解锁屏幕的功能，因此难以在有锁屏密码的设备上达到效果。

## 如何把图片和脚本一起打包，或者打包多个脚本

如果除了单脚本以外还有其他脚本、图片、音乐等资源一起打包，则需要使用项目打包功能。

1. 新建一个文件夹，命名为项目名称。
2. 在该文件夹下新建脚本，或者移动脚本到该文件夹，命名为main.js，表示主脚本。脚本打包后将会从该脚本开始运行。
3. 把其他资源或脚本放在该文件夹，并通过相对路径引用。
4. 通过文件夹菜单的"打包"功能来打包该项目

例如，主脚本要读取同一文件夹下的图片1.png，再执行找图，则可以通过`images.read("./1.png")`来读取，其中"./1.png"表示同一目录1.png图片；ui中的图片控件要引用同一文件夹的2.png图片则为`<img src="file://2.png"/>`。Auto.js内置的函数和模块都支持相对路径，但是，其他情况则需要使用`files.path()`函数来把相对路径转换为绝对路径。

目前Auto.js还不支持项目的图形化管理，后续会加入。

## 如何使打包的应用不显示主界面

需要使用项目打包功能。

1. 新建一个文件夹，命名为项目名称。
2. 在该文件夹下新建脚本，或者移动脚本到该文件夹，命名为main.js，表示主脚本。脚本打包后将会从该脚本开始运行。
3. 在该文件夹下新建一个project.json的文件，其内容如下：
```
{
  "name": "项目名称",
  "versionName": "1.0.0",
  "versionCode": 1,
  "packageName": "org.autojs.example",
  "main": "main.js",
  "launchConfig": {
      "hideLogs": true
  }
}
```
其中，项目名称改为自己的项目名称，"org.autojs.example"改成自己的包名，下面的"launchConfig"表示启动配置，"hideLogs"表示隐藏日志。
4. 通过文件夹菜单的"打包"功能来打包该项目

有关项目打包和配置的更多信息，参见项目与项目配置(待补)。

## Auto.js自带的模块和函数中没有的功能如何实现

由于Auto.js支持直接调用Android的API，对于Auto.js没有内置的函数，可以直接通过修改Android代码为JavaScript代码实现。例如旋转图片的Android代码为：
```
import android.graphics.Bitmap;
import android.graphics.Matrix;

public static Bitmap rotate(final Bitmap src,
                            final int degrees,
                            final float px,
                            final float py) {
    if (degrees == 0) return src;
    Matrix matrix = new Matrix();
    matrix.setRotate(degrees, px, py);
    Bitmap ret = Bitmap.createBitmap(src, 0, 0, src.getWidth(), src.getHeight(), matrix, true);
    return ret;
}
```
转换为JavaScript的代码后为：
```
importClass(android.graphics.Bitmap);
importClass(android.graphics.Matrix);

function rotate(src, degrees, px, py){
    if (degrees == 0) return src;
    var matrix = new Matrix();
    matrix.setRotate(degrees, px, py);
    var ret = Bitmap.createBitmap(src, 0, 0, src.getWidth(), src.getHeight(), matrix, true);
    return ret;
}
```
有关调用Android和Java的API的更多信息，参见[Work with Java](https://developer.mozilla.org/zh-CN/docs/Mozilla/Projects/Rhino/Scripting_Java)。