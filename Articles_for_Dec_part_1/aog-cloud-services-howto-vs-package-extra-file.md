<properties
                pageTitle="如何将外部文件打包到云服务包中"
                description="通过 Visual Studio 将额外的文件打包到Cloud Services Package"
                services="cloud-services"
                documentationCenter=""
                authors=""
                manager=""
                editor=""
                tags="云服务,Cloud Services Package,Visual Studio"/>

<tags
                ms.service="cloud-services-aog"
                ms.date="12/15/2016"
                wacn.date="12/15/2016"/>

# 如何将外部文件打包到云服务包中

在使用 Visual Studio 开发 .NET 云服务应用程序时，经常需要往项目中添加一些额外的资源文件，脚本或者 DLL 动态库，而时常会忘记修改这些文件的  `Copy to output directory`属性，为此将会出现无法预料的异常，并且有时会在本地运行正常，而发布到云服务上时，就会出现问题。这通常是因为没有修改额外添加的引用或者资源文件的 `Copy to output directory` 属性而导致的。

**解决方法：**

在 Visual Studio 中, 将额外添加的引用或者资源文件的 `Copy to output directory` 属性设置为 `copy always` 或者是 `copy if newer`。

**设置操作步骤：**  

1.	首先加入一个外部的文件到解决方案中；

![import-extra-file](./media/aog-cloud-services-howto-vs-package-extra-file/import-extra-file.png)

2.	设置文件的 `build action` 属性为 `content`；

![build-action](./media/aog-cloud-services-howto-vs-package-extra-file/build-action.png)

3.	设置文件的 `copy to Output Directory` 属性为 `Copy if newer`；

![copy-to-output-directory](./media/aog-cloud-services-howto-vs-package-extra-file/copy-to-output-directory.png)

设置完成后，在程序打包时，就会将该文件一起打包到云服务包中，从而避免不可预见的异常产生。