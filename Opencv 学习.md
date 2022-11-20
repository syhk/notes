

openCV 模块 ：

core : 最太忙的数据结构

highgui: 视频与图像的读取、显示、存储

imgproc：图像处理的基础方法

features2d: 图像特征双及特征匹配

### 图像的 IO 操作

#### 读取图像

```python
cv.imread()
```

参数：

-  要读取的图像 
-  读取方式的标志 
   -  cv.IMERAD*COLOR:以彩色模式加载图像，任何图像的透明度都将被忽略，这是默认参数 
   -  cv.IMREAD*GRAYSCALE: 以灰度模式加载图像 
   -  cv.IMREAD_UNCHANGED: 包括 plpha 通道的加载图像模式 
   -  可以使用 1 、0 、-1 来代替上面三个模式 
-  参考代码： 
```python
import numpy as np
import  cv2 as cv

# 以灰度图的形式读取图像

img=cv.imread("my.png",0) # 要保证图像存在且路径正确，路径有错误会返回一个 None 值
```
 

#### 显示图像

```python
cv.imshow()
```

参数：

-  显示图像的窗口名称，以字符串类型显示 
-  要加载的图像 

注意：要调用显示图像的 api 后，要调用 **cv.waitKey()** 绘图像绘制留下时间，否则窗口会出现无响应情况，并且图像无法显示出来。

也可以使用 matplotlib 对图像进行展示。

参考代码：

```python
# opencv 中显示
cv.imshwo("my.png",img)

cv.waitKey(0)

# 在matplotlib 对图像进行显示
plt.imshow(img[:,:,::-1])
```

#### 保存图像

```python
cv.imwrite()
```

参数：

-  文件名，需要保存在哪里 
-  要保存的图像 

参考代码：

```python
# 保存图像
cv.imwrite("my1.png",img)
```

完整参考代码：

```python
import numpy as np
import cv2 as cv
import matplotlib.pyplot as plt

# 读取图像
img=cv.imread("../images/s01.png")

# 显示图像
cv.imshow("sy",img)
cv.waitKey(0)
cv.destroyWindow()

# 使用 matplotlib
plt.imshow(img[:,:,::-1])
plt.show()

# 图像保存
cv.imwrite("../images/s02.png",img)
```

上面使用了两种方式显示。

#### 绘制几何图形

#### 绘制直线

```python
cv.line(img,start,end,color,thickness)
```

参数：

-  img 要绘制直线的图像 
-  start,end ：直线的起点和终点 
-  color : 线条的颜色 
-  Thickness : 线条宽度 

#### 绘制图形

```python
cv.circle(img,centerpoint,r,color,thickness)
```

参数：

-  img:一样的 
-  Centerpoint,r :圆心和半径 
-  color ： 颜色 
-  Thickness : 线条宽度，为 -1 时生成闭合图案并填充颜色 

#### 绘制矩形

```python
cv.rectangle(img,leftupper,rightdown,color,thickness)
```

参数：

-  img 
-  leftupper,rightdown 矩形的左上角和右下角坐标 
-  color 
-  Thickness 

#### 向图像中添加文字

```python
cv.putText(img,text,station,font,fontsize,color,thickness,cv.LINE_AA)
```

参数：

-  img 
-  text :要写入的文本数据 
-  font 字体 
-  fontsize 字体大小 

示例代码：

```python
#    绘制几何图形
# 创建一个空白的图像
img=np.zeros((512,512,3),np.uint8)

# 绘制图形
# 直线
cv.line(img,(0,0),(511,511),(255,0,0),(5))
# 圆形
cv.circle(img,(256,256),(60),(247,52,36),-1)
# 矩形
cv.rectangle(img,(100,100),(400,400),(0,225,0),5)
# 文本
cv.putText(img,"shen yang",(100,150),cv.FONT_HERSHEY_PLAIN,5,(255,69,82),4)

# 显示结果
plt.imshow(img[:,:,::-1])
plt.show()
```

#### 获取并修改图像中的像素点

可以通过行和列坐标值获取这个图像像素点的像素值，对BGR 图像，它返回一个蓝绿红值的数组，对于灰度图像，仅返回相应的强度值，使用相同的方法对像素值进行修改。

#### 获取图像的属性

图像的属性包括行数、列数、通道数、图像数据类型、像素数等

| 属性 | API |  |
| --- | --- | --- |
| 形状 | img.shape |  |
| 图像大小 | img.size |  |
| 数据类型 | img.dtype |  |


#### 图像通道的拆分与合并

需要在B,G,R 通道图像上单独工作，可以使用拆分,用完再合并

```python
b,g,r=cv.split(img)
# 合并通道
img=cv.merge((b,g,r))
```

#### 色彩空间的改变

广泛使用的转换方法有两种： BGR-> Gray 和 BGR -> HSV。

```python
cv.cvtColor(input_image,flag)
```

参数：

-  input_image: 进行颜色空间转换的图像 
-  flag 转换类型 
   -  cv.COLOR_BRG2GRAY:BGR-> Gray 
   -  cv.COLOR_BRG2HSV: BGR->HSV 

### 算数操作

#### 图像的加法

```python
cv.add(x,y) # x 和 y 都是图像
```

两个图像要相加需要具有相同的大小和类型，或者第二个图像可以是标量值。

openCV 的加法是饱和操作，而 Numpy  加法是模运算。

```python
img1=cv.imread("../images/s01.png")
img2=cv.imread("../images/s02.png")

img3=cv.add(img1,img2)


plt.imshow(img3[:,:,::-1])
plt.show()

img4=img1+img2 # 使用 numpy 直接相加

plt.imshow(img4[:,:,::-1])
plt.show()
```

#### 图像的混合

这其实也是加法，但是不同的是两幅图像的权重不同，也要求两幅图像是**相同大小**的。

```python

img1=cv.imread("../images/s01.png")
img2=cv.imread("../images/s02.png")

# 图像混合
img5=cv.addWeighted(img1,0.3,img2,0.7,0)
```

#### 几何变换

#### 图像缩放

就是对图像进行放大或者缩小。

```python
cv.resize( src,disze,fx=0,fy=0,interpolation=cv.INTER_LINEAR)
```

参数：

-  src 输入图像 
-  dsize 绝对尺寸，直接指定调整后图像的大小 
-  fx,fy 相对尺寸，将 dsize 设置为 None，fx 与 fy 设置比例因子 
-  interpolation 设置插值方法 

```python
#  几何变换
import  numpy as np
import cv2 as cv
import matplotlib.pyplot as plt
img=cv.imread("../images/s01.png")
# 绝对尺寸
res=cv.resize(img,(2*500,2*200))
# 相对尺寸
res1=cv.resize(img,None,fx=0.5,fy=0.5)
plt.imshow(img[:,:,::-1])
plt.show()
```

#### 图像平移

就是按规定移动图像

```python
cv.warpAffine(img,M,dsize)
```

参数：

-  img 
-  m : 2*3移动矩阵 
-  dsize 输出图像的大小 （是以宽度和高度的形式， width=列数， height=行数） 

```python
# 图像移动
img1=cv.imread("../images/s02.png")

N=np.float32([[1,0,100],[0,1,50]])

img2=cv.warpAffine(img1,N,(1000,1000))

plt.imshow(img2[:,:,::-1])
plt.show()
```

#### 图像旋转

以 cv 都是 cv2 的别名

```python
cv2.getRotationMatrix2D(center, angle,scale)
```

参数：

-  center: 旋转中心 
-  angle: 旋转角度 
-  scale：缩放比例 

返回：

-  M ： 旋转矩阵 
   - 调用 cv.warpAffine 完成图像的旋转

```python
img1=cv.imread("../images/s02.png")

N=np.float32([[1,0,100],[0,1,50]])

img2=cv.warpAffine(img1,N,(1000,1000))

plt.imshow(img2[:,:,::-1])
plt.show()
```
