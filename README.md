# 硬件学习仓库

## 前言

首先大家可能会听学长学姐提及我们IoT组有一个硬件的学习仓库，但是大家可能会质疑他的权威性，首先可以告诉大家，这个仓库他是历届学长学姐的经验总结出来的，是一个可参考的仓库，它相当于是给大家有一个清晰的学习规划，我们会尽可能的使他变得更全面！

## Hi3861的学习（OpenHarmony南向开发）

    首先我们要了解什么是基于IDE开发，什么是基于命令行开发？

- >  IDE（集成开发环境） 就是电脑上编程时用的应用  

- >  命令行就一般指在终端进行开发

那么根据不同的习惯有不同的开发方式，这里我们主要介绍一下IDE（集成开发环境），尤其是windows的开发方式

### windows下的DevEco Device Tool开发环境搭建

1. 下载DevEco Device Tool最新`Windows`版本软件包。

2. 解压DevEco Device Tool压缩包，双击安装包程序，单击下一步进行安装。

3. 设置DevEco Device Tool的安装路径，请注意安装路径不能包含中文字符，不建议安装到`C`盘目录，单击下一步。

    ![Alt text](./图床/1.png)

4. 然后按照上面的提示进行点击，其中会有检查`python`和`vscode`的步骤，记住！python的版本最好是`3.8`或`3.9`

5. 打开`Visual Studio Code`，进入`DevEco Device Tool`工具界面。至此，DevEco Device Tool Windows开发环境安装完成。

### 创建工程并获取源码

1. 打开DevEco Device Tool，进入Home页，点击New Project创建新工程。

2. 在新工程的配置向导页，配置工程相关信息，包括：
    > 工程名：（随便写）  
    > SOC：Hi3861  
    > 工程路径：选择合适的路径  

    ![Alt text](./图床/2.png)

3. 工程配置完成后，下载SDK（如果没有，点击SDK旁边的下载），点击`Confirm`，DevEco Device Tool会自动启动OpenHarmony源码的下载。由于OpenHarmony稳定版本源码包体积较大，请耐心等待源码下载完成。
    > SDK是软件开发工具包，辅助开发某一类软件的相关文档、范例和工具的集合

4. 完成以后，源码环境已经搭建完成，下面我们就可以愉快的进行Hi3861的开发了！

5. **注意：** 如果没有源码下载成功，我在Hi3861文件夹中提供的有SDK的压缩包和源码环境的压缩包，可以自行进行导入

### 附：导入SDK和工程源码
    说明：在下载时可能会出现下载失败，这里可以将这种情况解决
1. 选择工程源码，这里我们先下载`Dev-Tool`和`hi3861_hdu_iot`(SDK)，这里可直接通过网盘进行下载[Hi3861环境](https://cloud.189.cn/t/r2QBNr2umqUv )（访问码：r3kc）
    > `Dev-Tool` 在 `hi3861环境/DevTools/DevTools_Hi3861V100.zip`
    > `hi3861_hdu_iot` 在 `hi3861环境/hi3861_hdu_iot/0001.zip`

2. 将两个压缩包解压后，先将工程源码导入到DevEco Device Tool，
### 说明
    

目前正在更新Hi3861的资料