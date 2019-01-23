# 图片的加载和适配

### 与图片加载有关的

#### 图片占用内存与图片的色彩格式的关系
[Android 一张图片BitMap占用内存的计算](https://www.jianshu.com/p/5752eae2f27a)

Bitmap内存计算:ARGB_8888是最占内存的，因为一个像素占32位，8位=1字节，所以一个像素占4字节的内存。ARGB_4444的一个像素占2个字节。RGB_565的一个像素也是占两个字节。ALPHA_8的一个像素只占一个字节。
一个图片的像素=图片宽度 × 图片长度。
一张图片（BitMap）占用的内存=图片宽度×图片长度×单位像素占用的字节数（图片的像素×单位像素占用的字节数）。

#### 同一图片在不同屏幕的手机、不同的屏幕密度文件夹下占用内存大小
[android 图片占用内存大小及加载解析](https://blog.csdn.net/smileiam/article/details/68946182)

`屏幕尺寸` 含义：手机对角线的物理尺寸 ,单位：英寸（inch），1英寸=2.54cm

`屏幕分辨率`含义：手机在横向、纵向上的像素点数总和,例子：1080x1920，即宽度方向上有1080个像素点，在高度方向上有1920个像素点,单位：px（pixel），1px=1像素点,UI设计师的设计图会以px作为统一的计量单位

`屏幕像素密度` 含义：每英寸的像素点数 ,单位：dpi（dots per ich）,假设设备内每英寸有160个像素，那么该设备的屏幕像素密度=160dpi(drawable文件夹: drawable-mdpi:160,drawable-hdpi:240, drawable-xhdpi:320, drawable-xxhdpi:480)

`密度无关像素` 含义：density-independent pixel，叫dp或dip，与终端上的实际物理像素点无关。单位：dp，可以保证在不同屏幕像素密度的设备上显示相同的效果,在Android中，规定以160dpi（即屏幕分辨率为320x480）为基准：1dp=1px  ==> xhdpi：2.0,hdpi：1.5,mdpi：1.0（基准）,ldpi：0.75

获取图片大小
```
public int getBitmapSize(Bitmap bitmap){
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT){     //API 19
        return bitmap.getAllocationByteCount();
    } else if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB_MR1){//API 12
        return bitmap.getByteCount();
    } else {
        return bitmap.getRowBytes() * bitmap.getHeight(); //earlier version
    }
}
```
bitmap占用内存大小，与手机的屏幕密度、图片所放文件夹密度、图片的色彩格式有关。 
这里总结一下获取Bitmap图片大小的代码： 
手机在加载图片时，会先查找自己本密度的文夹下是否存在资源，不存在则会向上查找，再向下查找，并对图片进行相应倍数的缩放：

  * 如果在与自己屏幕密度相同的文件夹下存在此资源，会原样显示出来，占用内存正好是: 图片的分辨率*色彩格式占用字节数；

  * 若自己屏幕密度相同的文件夹下不存在此文件，而在大于自己屏幕密度的文件夹下存在此资源，会进行缩小相应的倍数的平方；

  * 若在大于自己屏幕密度的文件夹下没找到此资源，则会向小于自己屏幕密度的文件夹下查找，如果存在，则会进行放大相应的倍数的平方，这两种情况图片占用内存为: 
占用内存=图片宽度 X 图片高度/((资源文件夹密度/手机屏幕密度)^2) * 色彩格式每一个像素占用字节数

#### 加载大图
[高效加载大图](http://hukai.me/android-training-course-in-chinese/graphics/displaying-bitmaps/load-bitmap.html)

[Android高效加载大图、多图解决方案，有效避免程序OOM](https://blog.csdn.net/guolin_blog/article/details/9316683)

### 适配
[Android 屏幕适配：最全面的解决方案](https://www.jianshu.com/p/ec5a1a30694b)


