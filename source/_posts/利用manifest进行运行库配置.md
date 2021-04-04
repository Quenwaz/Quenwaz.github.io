---
title: 利用manifest进行运行库配置
date: 2019-01-15 13:06:02
categories: 编码
tags: 
    - C++
    - Visual Studio
---


## 利用manifest进行运行库配置
### 目的
使发布的可执行程序，在windows各个版本中能正确打开程序。且无需额外安装**vcredist**对运行库进行部署。
### Assembly Manifest机制
Assembly Manifest 机制仅仅适用于项目属性运行库为**Multi-threaded Debug DLL (/MDd) 或 Multi-threaded DLL (/MD)**, 在Visual Studio中新建项目，默认会将.manifest文件嵌入到应用程序当中。
manifest文件示例:
```
<?xml version='1.0' encoding='UTF-8' standalone='yes'?>
<assembly xmlns='urn:schemas-microsoft-com:asm.v1' manifestVersion='1.0'>
    <dependency>
        <dependentAssembly>
            <assemblyIdentity type='win32' name='Microsoft.VC80.CRT' version='8.0.50608.0' processorArchitecture='x86' publicKeyToken='1fc8b3b9a1e18e3b' />
        </dependentAssembly>
    </dependency>
</assembly>
```
因为dll是动态加载共享库，同一dll可能被多个程序所使用。而存在不同的程序调用相同的dll,而其版本存在差异，操作系统就无法分辨。如果加载了错误版本的dll，将导致程序无法正常启动。
而(.manifest)可以以**强文件名的方式定位到所需dll所在目录**,具体实现利用manifest文件中节点
assemblyIdentity 的属性进行组合目录名，可以到**C:\Windows\WinSxS**找到其中有所需的dll。

另外，对于运行库dll本身存在自己的manifest清单文件描述，主要用于描述其强文件签名(hash),确保文件正确性。

最后，关于系统升级dll的兼容性问题，因为系统升级可能导致其系统库版本进行升级，版本号存在变化。那么清单文件中描述的版本在新升级的系统中将无法适用，所以引进了**policy的策略文件来确认映射关系**进行版本重定向。具体重定向描述位置位于**C:\WINDOWS\WinSxS\Policies\Policy**
#### 运行库查找规则
根据MSDN的说明，在本地查找并加载遵循一下规则：
- 在应用程序本地文件夹中查找名为 <assemblyName>.manifest 的清单文件。在此示例中，加载程序试图在exe所在的文件夹中查找Microsoft.VC80.CRT.manifest。如果找到该清单，加载程序将从应用程序文件夹中加载 crt dll。如果未找到 crt dll，加载将失败。
- 尝试在 appl.exe 本地文件夹中打开文件夹 <assemblyName>，如果存在此文件夹，则从中加载清单文件 <assemblyName>.manifest。如果找到该清单，加载程序将从 <assemblyName> 文件夹中加载 crt dll。如果未找到 crt dll，加载将失败。

### 解决方案
#### 前提
确保当前项目Runtime Library 是**Multi-threaded DLL**，否则无需配置。
#### 项目配置

**项目属性->清单工具->输入输出**  中将**清单嵌入**设为**否**，那样就会帮你生成manifest文件而不会将它嵌入

如果你需要额外的配置
在**链接器->清单文件->附加清单依赖项**中加入
例如:
```
type='win32' name='Microsoft.Windows.Common-Controls' version='6.0.0.0' processorArchitecture='X86' publicKeyToken='6595b64144ccf1df' language='*'
```
#### 运行库移植
在各个版本中的vs安装目录中找到**redist**目录，将各个版本需要的运行库及对应的manifest(**低版本存在**)移动到发布的exe同级目录。



> **参考资料**<br/>
> 1. [vc2008不安装vcredist发布程序](https://blog.csdn.net/pizi0475/article/details/7791150?locationNum=9&fps=1)
> 2. [ Assembly Manifest](https://blog.csdn.net/xiaojianpitt/article/details/4269641)




---
<small><font color= "gray">本文[Quenwaz](https://quenwaz.github.io)版权所有，转载请说明出处</font></small>