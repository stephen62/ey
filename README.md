# 参考文献

- https://blog.csdn.net/shenziheng1/article/details/54893265

# 环境配置

- VS2017
- QT5
- VTK8.2.0
- ITK5.0.1

# 快速开始
## 配置说明
* 项目内已包含VTK、ITK的相关文件，无需额外安装
* 项目中包含的VTK、ITK相关文件都是Release x64版本，在启动程序时请调整到Release x64
* Release文件夹中包含有支持Qt运行的dll，使用时无需额外配置；但如有需要对界面做修改，推荐安装Qt5.8 msvc2015_64

## 图像生成接口

### 抽象类ImageAlgorithm
* `void imageProcess(int count, ...)` 传入若干参数生成图像
* `void setInputName(const char* input)` 传入文件路径，读取并处理文件
* `vtkDataObject* getOutput()` 输出图像数据

### 调用
重写以上方法后，可在main函数中进行设置
```C++
    MainApplication w;
    ImageAlgorithm* ia = new itkDrr();
    w.setImageAlgorithm(ia);
```

## 图像配准算法接口

### 抽象类RegistrationAlgorithm
* `double getSimilarity()` 输出相似度，取值[0,1]

### 调用
重写以上方法后，可在main函数中进行设置
```C++
    MainApplication w;
    RegistrationAlgorithm* ra = new drrRegistration();
    ra->setDRRFilePath(w.getDRRImage());
    ra->setXRayFilePath(w.getXRayImage());
    w.setRegistrationAlgorithm(ra);
```


# 功能列表

* 打开CT
    * 选择包含CT序列的文件夹来上传CT序列
    * 文件夹路径不能包含中文以及特殊符号
    * CT序列支持DICOM格式
* 打开X-光图像
    * X-光图像支持BMP格式
    * 文件路径不能包含中文以及特殊符号
* 基于ITK的DRR算法
    * 参数包括：像素平移、旋转度、源像距、阈值等
    * 矩阵只支持查看
    * 光源距离与初始像素间隔只支持查看
    * 参数输入方式改为回车响应
* DRR图像与X光图像叠加显示
    * 叠加显示需要将图像的存储格式统一，在代码内我们将DRR图像缓存为了BMP格式
    * 请使用前将DRR图像的分辨率调整为与X光图像相同
* 图像透明度修改
* 基于VTK的3维图像展示
* ITK的DRR图像与VTK的3维图像切换
* 图像配准算法
    * 已提供接口


