# 教程在 Directadmin - Eldernode 博客上安装 ImageMagick

> 原文：<https://blog.eldernode.com/install-imagemagick-on-directadmin/>

![Tutorial Install ImageMagick on Directadmin](img/ef9ed5a72b70dee669560638142c7ca1.png)

ImageMagick 是一个免费、开源、多平台库和二进制文件的伟大集合。ImageMagick 用于处理光栅图形和矢量图形，具有许多显示、转换、修改和编辑功能。这个程序可以处理 200 多种不同的图像格式。该程序可以通过命令行界面(CLI)或其库用其他语言编写的程序来使用。在这篇文章中，我们将教你关于在 Directadmin 上安装 ImageMagick 的教程。购买 [DirectAdmin VPS](https://eldernode.com/directadmin-vps-server/) 服务器可以参考 [Eldernode](https://eldernode.com/) 中的套装。

## **如何在 Directadmin 上一步步安装 ImageMagick**

Imagick 或 ImageMagick 是由 John Christy 于 1987 年开发的开源软件。他提供了这样一个工具，能够创建、编辑或转换位图图像。今天，这个特性涵盖了 200 多种可以做大事的图像格式。有趣的是，要在 [WordPress](https://blog.eldernode.com/tag/wordpress/) 中编辑图像，你需要一个 PHP 的图像优化库，Imagick 会为你做同样的事情。

### **ImageMagick 是做什么的？**

ImageMagick 是一个创建、编辑或转换位图图像的工具。ImageMagick 中的用户可以阅读各种图像格式，包括 DPX、EXR、GIF、JPEG、JPEG-2000、PDF、PhotoCD、PNG、Postscript、SVG 和 TIFF，或者以各种格式保存图像。

使用这个程序，您可以调整大小、翻转、镜像、旋转、扭曲、裁剪和转换图像、调整图像颜色、应用各种特殊效果、绘制文本、线条、绘制多边形和椭圆形。该软件还可以使用编辑工具将质量不理想的照片转换为透明照片。

该软件的其他应用包括将图像从一种格式转换为另一种格式的能力。例如 PNG 到 JPEG。ImageMagick 以 DPX、EXR、GIF、JPEG、JPEG-2000、PDF、PhotoCD、PNG、Postscript、SVG 格式保存图像。从一组图像创建 GIF 动画序列是这个程序的另一个应用。在图像中插入描述性或艺术性文本、使用准确的颜色配置文件进行颜色管理以及方便地访问图像区域之外的像素是 ImageMagick 可以完成的一些事情。

在下一节中，我们将教你如何在 [DirectAdmin](https://blog.eldernode.com/tag/directadmin/) 上安装 ImageMagick。

### **在 Directadmin** 上安装 ImageMagick

要在 Directadmin 上安装 ImageMagick，只需按照以下步骤操作。在第一步中，您需要执行以下命令:

```
yum install ImageMagick
```

```
yum install ImageMagick-devel
```

执行上述命令后，您现在必须按顺序执行以下 8 个命令:

```
cd /usr/local/src
```

```
wget http://pecl.php.net/get/imagick-3.0.1.tgz
```

```
tar zxf imagick-3.0.1.tgz
```

```
cd imagick-3.0.1
```

```
phpize
```

```
./configure
```

```
make
```

```
make install
```

请注意，扩展显示在安装路径的末尾。您必须将该路径添加到 **php.ini** 文件和 **extension_dir** 字段中。比如下面这个命令:

```
extension_dir = “/usr/local/lib/php/extensions/no-debug-non-zts-20060613”
```

然后，您需要将以下代码添加到 **php.ini** 服务器文件的末尾。

```
extension=imagick.so
```

**最后重启**Apache 服务。

```
service httpd restart
```

## 结论

ImageMagick 是一个开源库，允许用户调整图像大小，裁剪照片，并对图像进行自定义更改。上述库由流行的编程语言支持，如 C++、PHP、Python、Ruby、Perl 等。在本文中，我们试图向您介绍在 Directadmin 上安装 ImageMagick 教程。还需要注意的是，如果你愿意，可以参考文章[介绍并在 Cpanel](https://blog.eldernode.com/introducing-and-install-imagemagick-on-cpanel/) 上安装 ImageMagick。