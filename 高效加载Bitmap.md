#高效加载bigmap
加载 Bitmap 时很容出现内存溢出，遇到：
```
java.lang.OutofMemoryError:bitmap size exceeda VM budget
```
有缓存策略 这是一个通用的思想
### 如何加载一个Bitmap
BitmapFactory 类提供了四类方法：decodeFile、decodeResource、decoceStream和decodeByteArray，分别用于支持
从文件系统、资源、输入流以及字节数组中加载出一个Bitmap对象。 其中decodeFile和decodeResource又间接的调用了decodeStream方法，这四类方法最终是在android

