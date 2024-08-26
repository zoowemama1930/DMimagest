## 搭建图床实现markdown文档自动关联GitHub仓库图片

​       日常使用强大的笔记软件typroa也存在一些常见的问题，例如，图片占用了较大的本地存储空间，共享本地文档的同时需共享图片，图片的转发和加载降低数据共享的效率。

为了解决以上问题，可采取搭建图床实现云端持久化存储图片与markdown文档，编辑markdown文档的同时上传图片到云端，markdown文档关联云端图片的链接地址，实现图文的持久化存储与网络共享，提高学习与工作效率。（此方案需要挂梯子）

对于薅羊毛的小伙伴，这可作为临时的解决方案。github仅提供1GB的免费存储空间，1GB以上的存储需求可付费使用。当然，对于跨客户端撰写markdown文档的用户，多本地存储的信息不利于完整查阅文档，利用github+picgo+typroa 搭建图床实现图文数据共享是最佳的临时解决方案。完成markdown文档撰写导出pdf或html格式，节省大量图片的存储空间，但缺点是无法二次编辑。

十全九美的方案离不开资本统治。

适用场景：博客发文，共享网络文档。

### github仓库

#### 新建github仓库 

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123103.png)

仓库请选为公共仓库，typroa无法获取私有仓库的图片。 如果担心公共仓库不安全，也可以使用其他的方案，必须将github仓库克隆到本地，将图片存储在本地，再统一提交到私有仓库。使用python替换typroa文档中的本地图片链接地址为github仓库网络链接地址。本文仅适用于存储私密度不高的信息，不展开深入研究。

<img src="https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123045.png" style="zoom:25%;" />

#### 创建token身份验证

picgo上传图片到github需要身份验证

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123236.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123315.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123338.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123356.png)



![1724646887477](C:%5CUsers%5Clamp3%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1724646887477.png)

注：记得保存生成的token校验码



###  安装picgo

官网：https://github.com/Molunerfinn/PicGo

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123929.png)

图床设置

github仓库地址：zoowemama1930/DMimagest

设定自定义域名：https://raw.githubusercontent.com/zoowemama1930/DMimagest/main

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826123916.png)

开启picgo-server

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826124129.png)

快捷键设置

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826124010.png)

测试图片上传

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826124043.png)

### typroa设置

这里介绍的是typroa1.2.4版本，建议使用2.2.0以上版本，图像设置的相关功能更齐全。

typroa编辑文档插入图片的同时上传图片到github仓库。图片地址出处来源于github仓库，实现网络图片共享。

文件---偏好设置---图像

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826125509.png)

### typroa实操

1）截图完敲快捷键ctl+z上传图片到GitHub

2） ctl+c将图片粘贴到typroa文档（必须是图片上传完毕再粘贴）

3）此时图片显示github仓库存储链接地址。如果操作失败则显示图片所在本地位置

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826125540.png)

4）将markdown文档上传至GitHub查看效果（命名相同的图片或文档无法上传）

左边是文档中的所有图片信息，右边是文档内容，图文并茂，完美收工。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240826130806.png)