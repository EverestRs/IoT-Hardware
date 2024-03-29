- [硬件学习仓库](#硬件学习仓库)
  - [前言](#前言)
  - [Hi3861的学习（OpenHarmony南向开发）](#hi3861的学习openharmony南向开发)
    - [windows下的DevEco Device Tool开发环境搭建](#windows下的deveco-device-tool开发环境搭建)
    - [创建工程并获取源码](#创建工程并获取源码)
    - [附：导入SDK和编译工具包](#附导入sdk和编译工具包)
    - [验证环境](#验证环境)
  - [快速入门](#快速入门)
    - [确定目录结构](#确定目录结构)
    - [编写业务代码](#编写业务代码)
    - [添加新组件](#添加新组件)
    - [修改单板配置文件](#修改单板配置文件)
    - [编译](#编译)
    - [烧录](#烧录)
    - [串口查看](#串口查看)
  - [Hi3861的简单编译流程说明](#hi3861的简单编译流程说明)
    - [构建系统](#构建系统)
    - [整体编译流程](#整体编译流程)
    - [组件的定义](#组件的定义)
    - [开发需要掌握的编译文件内容](#开发需要掌握的编译文件内容)
    - [附：BUILD.gn分析（未完成待续）](#附buildgn分析未完成待续)
  - [Hi3861的基础内核开发](#hi3861的基础内核开发)
    - [什么是内核？](#什么是内核)
      - [Openharmony内核](#openharmony内核)
      - [LiteOS-M内核](#liteos-m内核)
        - [KAL抽象层](#kal抽象层)
    - [线程管理](#线程管理)
      - [线程状态的介绍](#线程状态的介绍)
      - [单线程使用流程](#单线程使用流程)
      - [多线程使用流程](#多线程使用流程)
      - [多线程的封装](#多线程的封装)
    - [软件定时器](#软件定时器)
      - [软件定时器的介绍](#软件定时器的介绍)
      - [软件定时器使用流程](#软件定时器使用流程)
    - [互斥锁](#互斥锁)
      - [互斥锁介绍](#互斥锁介绍)
      - [互斥锁的使用流程](#互斥锁的使用流程)
      - [互斥锁案例体验](#互斥锁案例体验)
    - [信号量](#信号量)
      - [信号量介绍](#信号量介绍)
      - [信号量的使用流程](#信号量的使用流程)
    - [消息队列](#消息队列)
      - [消息队列介绍](#消息队列介绍)
      - [消息队列使用流程](#消息队列使用流程)
    - [说明](#说明)



# 硬件学习仓库

## 前言

首先大家可能会听学长学姐提及我们IoT组有一个硬件的学习仓库，但是大家可能会质疑他的权威性，首先可以告诉大家，这个仓库他是历届学长学姐的经验总结出来的，是一个可参考的仓库，它相当于是给大家有一个清晰的学习规划，我们会尽可能的使他变得更全面！

## Hi3861的学习（OpenHarmony南向开发）

    首先我们要了解什么是基于IDE开发，什么是基于命令行开发？

- >  IDE（集成开发环境） 就是电脑上编程时用的应用  

- >  命令行就一般指在终端进行开发

那么根据不同的习惯有不同的开发方式，这里我们主要介绍一下IDE（集成开发环境），尤其是windows的开发方式

### windows下的DevEco Device Tool开发环境搭建

- 下载[DevEco Device Tool](https://device.harmonyos.com/cn/develop/ide#download)最新`Windows`版本软件包。

- 解压DevEco Device Tool压缩包，双击安装包程序，单击下一步进行安装。

- 设置DevEco Device Tool的安装路径，请注意安装路径不能包含中文字符，不建议安装到`C`盘目录，单击下一步。

    ![Alt text](./图床/1.png)

- 然后按照上面的提示进行点击，其中会有检查`python`和`vscode`的步骤，记住！python的版本最好是`3.8`或`3.9`

- 打开`Visual Studio Code`，进入`DevEco Device Tool`工具界面。至此，DevEco Device Tool Windows开发环境安装完成。

### 创建工程并获取源码

- 打开DevEco Device Tool，进入Home页，点击New Project创建新工程。

- 在新工程的配置向导页，配置工程相关信息，包括：
    > 工程名：（随便写）  
    > SOC：Hi3861  
    > 工程路径：选择合适的路径  

    ![Alt text](./图床/2.png)

- 工程配置完成后，下载SDK（如果没有，点击SDK旁边的下载），点击`Confirm`，DevEco Device Tool会自动启动OpenHarmony源码的下载。由于OpenHarmony稳定版本源码包体积较大，请耐心等待源码下载完成。
    > SDK是软件开发工具包，辅助开发某一类软件的相关文档、范例和工具的集合

- 完成以后，源码环境已经搭建完成，下面我们就可以愉快的进行Hi3861的开发了！

- **注意：** 如果没有源码下载成功，我在Hi3861文件夹中提供的有SDK的压缩包和源码环境的压缩包，可以自行进行导入

### 附：导入SDK和编译工具包
    说明：在下载时可能会出现下载失败，这里可以将这种情况解决
- 选择工程源码，这里我们先下载`Dev-Tool`（工具包）和`hi3861_hdu_iot`（SDK），这里可直接通过网盘进行下载[Hi3861环境](https://cloud.189.cn/t/r2QBNr2umqUv )（访问码：r3kc）
    > `Dev-Tool` 在 `hi3861环境/DevTools/DevTools_Hi3861V100.zip`  
    > `hi3861_hdu_iot` 在 `hi3861环境/hi3861_hdu_iot/0001.zip`

- 将两个压缩包解压后，先将工程源码导入到DevEco Device Tool
    ![Alt text](./图床/3.png)  
  
- 然后点击DevEco Device Tool的工程配置，查看工具包是否安装，没有安装可以点击下载，下载成功就配置好了，如果下载失败请继续跟着文档往下走
    ![Alt text](./图床/4.png)

- 点击hi3861，点击`compiler_bin_path`
    ![Alt text](./图床/5.png)

- 这里主要是配置编译工具的路径，其实主要是`env_start.bat`的作用
    ![Alt text](./图床/7.png)  
    ![Alt text](./图床/6.png)

- 这里我打开一下`env_start.bat`，给你们看一下hi3861的编译环境（命令行），基于Hb框架
    ![Alt text](./图床/8.png)

- 到此我们的SDK和工具包已经配置完成！下面我们就可以愉快的进行Hi3861的开发了！
    > 如果还是有问题请提交一个issue！

### 验证环境
    说明：这里我们对于工具包的验证只有编译层面的，对于烧录我们不做验证

- 源码什么都不需要更改，我们点击`DevEco Device Tool`工具界面，在下方的`project tasks`中点击`Rebuild`，然后开始编译，出现以下画面则证明编译成功！  
    > 源码一开始的`applcations`子系统编译的是`wifi_iot_sample_app`组件，其组件的Build.gn只有编译`startup`这个业务模块，所以我们初学者不需要对源码进行修改就可以验证环境的完备性

    ![Alt text](./图床/9.png)

## 快速入门

### 确定目录结构

开发者编写业务代码时，务必先在`./applications/sample/wifi-iot/app`路径下新建一个目录（或一套目录结构），用于存放业务源码文件。

例如：在`app`下新增业务`my_first_app`，其中`hello_world.c`为业务代码，`BUILD.gn`为编译脚本，具体规划目录结构如下：
```
.
└── applications
    └── sample
        └── wifi-iot
            └── app
                └── my_first_app
                  │── hello_world.c
                  └── BUILD.gn
.
└── applications
    └── sample
        └── wifi-iot
            └── app
                └── my_first_app
                  │── hello_world.c
                  └── BUILD.gn
```

### 编写业务代码

新建`./applications/sample/wifi-iot/app/my_first_app`下的`hello_world.c`文件，在hello_world.c中新建业务入口函数HelloWorld，并实现业务逻辑。并在代码最下方，使用OpenHarmony启动恢复模块接口`SYS_RUN()`启动业务。（SYS_RUN定义在`ohos_init.h`文件中）
```c
#include <stdio.h>
#include "ohos_init.h"
#include "ohos_types.h"

void HelloWorld(void)
{
    printf("[DEMO] Hello world.\n");
}
SYS_RUN(HelloWorld);
```

编写用于将业务构建成静态库的`BUILD.gn`文件。

新建`./applications/sample/wifi-iot/app/my_first_app`下的`BUILD.gn`文件，并完成如下配置。

如步骤1所述，BUILD.gn文件由三部分内容（目标、源文件、头文件路径）构成，需由开发者完成填写。
```
static_library("myapp") {
    sources = [
        "hello_world.c"
    ]
    include_dirs = [
        "//utils/native/lite/include"
    ]
}
```
static_library中指定业务模块的编译结果，为静态库文件libmyapp.a，开发者根据实际情况完成填写。
sources中指定静态库.a所依赖的.c文件及其路径，若路径中包含"//“则表示绝对路径（此处为代码根路径），若不包含”//"则表示相对路径。
include_dirs中指定source所需要依赖的.h文件路径。

### 添加新组件

修改文件`build/lite/components/applications.json`，添加组件`hello_world_app`的配置，如下所示为`applications.json`文件片段，"##start##“和”##end##“之间为新增配置（”##start##“和”##end##"仅用来标识位置，添加完配置后删除这两行）：
> 说明： 本章节操作是以OpenHarmony-v3.1-Release版本为例进行操作的，该版本中，组件配置文件为`build/lite/components/applications.json`；若源码版本大于等于OpenHarmony 3.2 Beta2时，组件配置文件为`build/lite/components/communication.json`。
```json
{
  "components": [
    {
      "component": "camera_sample_communication",
      "description": "Communication related samples.",
      "optional": "true",
      "dirs": [
        "applications/sample/camera/communication"
      ],
      "targets": [
        "//applications/sample/camera/communication:sample"
      ],
      "rom": "",
      "ram": "",
      "output": [],
      "adapted_kernel": [ "liteos_a" ],
      "features": [],
      "deps": {
        "components": [],
        "third_party": []
      }
    },
##start##
    {
      "component": "hello_world_app",
      "description": "hello world samples.",
      "optional": "true",
      "dirs": [
        "applications/sample/wifi-iot/app/my_first_app"
      ],
      "targets": [
        "//applications/sample/wifi-iot/app/my_first_app:myapp"
      ],
      "rom": "",
      "ram": "",
      "output": [],
      "adapted_kernel": [ "liteos_m" ],
      "features": [],
      "deps": {
        "components": [],
        "third_party": []
      }
    },
##end##
    {
      "component": "camera_sample_app",
      "description": "Camera related samples.",
      "optional": "true",
      "dirs": [
        "applications/sample/camera/launcher",
        "applications/sample/camera/cameraApp",
        "applications/sample/camera/setting",
        "applications/sample/camera/gallery",
        "applications/sample/camera/media"
      ],
```
### 修改单板配置文件

修改文件`vendor/hisilicon/hispark_pegasus/config.json`，新增`hello_world_app`组件的条目，如下所示代码片段为`applications`子系统配置，"##start##“和”##end##“之间为新增条目（”##start##“和”##end##"仅用来标识位置，添加完配置后删除这两行）：
```
      {
        "subsystem": "applications",
        "components": [
##start##
          { "component": "hello_world_app", "features":[] },
##end##
          { "component": "wifi_iot_sample_app", "features":[] }
        ]
      },
```

### 编译
按照上面的步骤点击`Rebuild`，显示`success bulid`的字样。
    ![Alt text](./图床/9.png)

### 烧录
我们将bearpi（hi3861开发板）连接电脑后点击`hi3861`，然后点击`upload_port`，选择带有ch340的串口，如果没有ch340的标识，可能你没有安装ch340的串口驱动，这里我提供以下网盘下载链接 [CH340驱动下载](https://cloud.189.cn/t/zyAny2JjiAzq) （访问码：rgd6）
   ![Alt text](./图床/10.png)

**注意：选好port后就可以点击`project tasks`的`Upload`按钮，在出现`continue`的时候，按下bearpi上的复位按钮**

### 串口查看
点击`Monitor`后按下bearpi的复位按键，就可以查看打印出来的Hello world啦


## Hi3861的简单编译流程说明

### 构建系统
> （借鉴文章：[从本质上学会基于HarmonyOS开发Hi3861（主要讲授方法）](https://ost.51cto.com/posts/1860)）

在基于OpenHarmony开发Hi3861之前，需要对整个开发环境及过程有一个全局上的了解，首先还是从这一张最经典的框架图给大家：
  ![Alt text](./图床/框架图.png)
目前我们对Hi3861的开发主要涉及上图中的内核抽象层、系统能力子系统、DXF子系统、公共基础库子系统（提供KV存储、文件操作、定时器、IoT外设控制等能力供OpenHarmony各业务子系统及上层应用使用）、系统服务框架子系统（用于提供面向服务编程和对外提供能力用于分布式任务调度）

该构建系统由python脚本配合gn、ninja组成，若是为了开发Demo或者应用，不必细究编译构建系统的具体实现细节，只需要做到会使用即可。

当我们输入python build.py wifiiot指令，python脚本开始读取build目录中与wifiiot设备相关的各项参数信息并构造编译指令如下：
  > gn工具所在目录`/gn`
  > gen源码所在目录`/out/wifiiot`
  - `--root=. --dotfile=build/lite/.gn --args='product = "wifiiot" ohos_build_type = "release"'` 这条指令用于生成一些xxx.ninja文件，这些文件将在下一阶段指导ninja编译源码生成烧录文件
  - ninja工具所在目录`/ninja` 源码所在目录`/out/wifiiot`  `-w dupbuild=warn -C`这条指令用于根据前面生成的xxx.ninja文件调用工具链编译源码最终生成烧录文件
  - gn用于根据每个目录下的BUID,gn文件搜寻编译生成烧录文件所需的依赖文件，所以我们只要学会如何写BUILD.gn文件即可

### 整体编译流程
在上面我们提到了子系统，组件，业务模块等，这些都是我们openharmony框架的概念，这里我们不做过多的赘述。我们主要讲一讲openharmony是怎么知道我们要编译那一个文件夹。

首先，给大家看一下openharmony启动流程。
  ```
  阶段1：core 内核启动

  阶段2：core system service 内核系统服务

  阶段3：core system feature 内核系统特性

  阶段4：system startup 系统启动

  阶段5：system service 系统服务

  阶段6：system future 系统特性

  阶段7：application-layer service 应用层服务

  阶段8：application-layer feature 应用层特性
  ```
上述八个阶段是从编译器开始build文件，然后一步一步

传入到主板让主板进行编译的过程。

openharmony是怎么执行这8个阶段的，首先ohos_init.h定义了8个宏，用于让一个函数以“优先级2”在系统启动过程的1-8阶段执行。这里涉及到一个优先级问题，在系统启动的某个阶段，会有多个函数被调用，优先级决定了调用顺序。
```
优先级范围：0-4。

优先级顺序：0, 1, 2, 3, 4。
```

上述就是程序编译的优先顺序。
即函数会被标记为入口，在系统启动过程的1-8阶段，以“优先级2”被调用。

**ohos_init.h定义的8个宏**  
```
CORE_INIT()： 阶段1. core

SYS_SERVICE_INIT()： 阶段2. core systemservice

SYS_FEATURE_INIT()： 阶段3. core system feature

SYS_RUN()： 阶段4. system startup

SYSEX_SERVICE_INIT()： 阶段5. system service

SYSEX_FEATURE_INIT()： 阶段6. system feature

APP_SERVICE_INIT()： 阶段7. application-layer service

APP_FEATURE_INIT()： 阶段8. application-layer feature
```

这里大家可以看一下openharmony的编译相关文件，这个在`src/bulid/lite`路径下
```
build / lite
├── components # 组件描述文件
├── config # 编译相关的配置项
│ ├── component # 组件相关的模板定义
│ ├── kernel # 内核的编译配置参数
│ └── subsystem # 子系统模板
├── figures # readme中的图片
├── hb # hb pip安装包源码
├── make_rootfs # 文件系统镜像制作脚本
├── ndk # Native API相关编译脚本与配置参数
├── platform # ld脚本
├── testfwk # 测试编译框架
└── toolchain # 编译工具链配置，包括编译器路径、编译选项、链接选项等。
```

### 组件的定义
**1. 定义的位置**
`build\lite\components<对应子系统>.json`，我在下面会拿applications组件举例（我在这里放一下图片，图片里对应的就是配置文件）
    ![Alt text](./图床/11.png)


**2. 定义的内容**
组件属性，包括名称、功能简介、是否必选、源码路径、编译目标、RAM、ROM、编译输出、已适配的内核、可配置的特性和依赖等。

**3. 注意事项**
新增组件时需要在对应子系统json文件中添加相应的组件定义。产品所配置的组件必须在某个子系统中被定义过，否则会校验失败。

### 开发需要掌握的编译文件内容
我们不需要完全了解整个的编译流程，我们只需要懂得从子系统开始后面怎么走的就可以了。

- 我们打开`/src/vendor/bearpi/bearpi_hm_nano/config.json`，这个是bearpi的子系统配置文件，我这里只选取部分作为展示。大家可以看到在我注释的地方有两个单词`applications`和  `wifi_iot_sample_app`，有没有比较熟悉，没错，他就是我们在hello world案例中的 applcations 和 app 文件夹！证明我们编写的代码是在applications这一个子系统下的，并且是在wifi_iot_sample_app这个组件下的。
  > subsystem：子系统  
  > component：组件
    ```
    {
    "product_name": "bearpi_hm_nano",
    "ohos_version": "OpenHarmony 1.0",
    "device_company": "bearpi",
    "board": "bearpi_hm_nano",
    "kernel_type": "liteos_m",
    "kernel_version": "",
    "subsystems": [
      {
        "subsystem": "applications",  # 子系统名称
        "components": [
          { "component": "wifi_iot_sample_app", "features":[] }  # 组件名称
        ]
      },
      {
        "subsystem": "iot_hardware",
        "components": [
          { "component": "iot_controller", "features":[] }
        ]
      },
      {
        "subsystem": "hiviewdfx",
        "components": [
          { "component": "hilog_lite", "features":[] }
        ]
      },
      {
        "subsystem": "distributed_schedule",
        "components": [
          { "component": "samgr_lite", "features":[] }
        ]
      },
      {
        "subsystem": "security",
        "components": [
          { "component": "hichainsdk", "features":[] },
          { "component": "deviceauth_lite", "features":[] },
          { "component": "huks", "features":
            [
              "disable_huks_binary = false",
              "disable_authenticate = false",
    ```
- 不过为什么组件是wifi_iot_sample_app这一大长串的，但是我们是在app文件夹下添加我们的业务代码呢？好问题！这就要看另一个文件了，`build/lite/components/applications.json`，我截取里面包含wifi_iot_sample_app的这一段，可以看到`dirs`指向`applications/sample/wifi-iot/app`的app文件夹，他的`targets`也是app文件夹的位置，也就是说这个组件的名字虽然叫`wifi_iot_sample_app`，但是实际起作用的是app文件夹下的业务代码模块。
  > dirs：路径  
    targets：目标

  ```
      "adapted_kernel": [ "liteos_a" ],
      "features": [],
      "deps": {}
    },
    {
      "component": "wifi_iot_sample_app",     # 组件名字
      "description": "Wifi iot samples.",
      "optional": "true",
      "dirs": [
        "applications/sample/wifi-iot/app"    # 目标文件夹路径
      ],
      "targets": [
        "//applications/sample/wifi-iot/app"  # 目标文件夹
      ],
      "rom": "",
      "ram": "",
      "output": [],
      "adapted_board": [ "hi3861v100" ],
      "adapted_kernel": [ "liteos_m" ],
      "features": [],
      "deps": {
          "components": [
            "utils_base"
        ]
      }
    },
    {
      "component": "kit_framework",
      "description": "",
      "optional": "true",
      "dirs": [
        "applications/kit_framework"
  ```
- 然后我们在去看app文件夹下的`BUILD.gn`文件，这里你可能会对这个文件产生疑惑，我会对其讲解一下，这里我们先专心编译流程。这里我们看到lite_component("app")的app，说明这个gn文件是是配置app文件夹的，下面的startup就是我们需要编译的业务模块。
  我们在上面的时候创建hello wprld业务的时候在hello world的文件夹里面也创建了一个Build.gn文件，其实我们在app的Build.gn文件中的`features`板块中应该填写`业务文件夹名称 : 业务Build.gn名称`，但是startup的Build.gn名字与文件夹的名字重了，所以可以只用写一个startup
  ```
  # app的Build.gn
    import("//build/lite/config/component/lite_component.gni") # 配置 Go 语言的工具链的脚本文件

    lite_component("app") {    # 编译文件夹名称是app
        features = [
            "startup",   # startup的Build.gn名字与文件夹的名字重了，所以可以只用写一个startup
        ]
    }
  ```
  ```
  # startup的Build.gn
    source_set("startup") {    # 业务Build.gn名称是startup
      sources = [

      ]

      include_dirs = [ ]
  }
  ```

- 因为startup的Build.gn不是很完整，这里我们使用demolink的BUILD.gn做讲解，`static_library`是静态库的意思，在这里面填的就是业务Build.gn的名字，`sources`板块是我们在这个业务模块里需要编译的源文件，`include_dirs`是我们所有源文件需要的头文件（库文件）的路径
  ```
    static_library("example_demolink") {      # 业务Build.gn的名字是example_demolink
      sources = [
          "helloworld.c"    # 需要编译的源文件
      ]

      include_dirs = [        # 源文件需要的头文件（库文件）的路径
          "//utils/native/lite/include",
          "//domains/iot/link/libbuild"
      ]
  }
  ```
- 最后我们总结一下，一开始我们是在`config.json`里面子系统的组件到`applications.json`里面的`wifi_iot_sample_app`的app文件夹，再到app文件夹下的Build.gn，再到业务模块里面的Build.gn

### 附：BUILD.gn分析（未完成待续）
- 这里以led_example.c程序为例，给大家分析BUILD.gn文件，希望大家能举一反三：  
在applications\sample\wifi-iot\app目录中有一个BUILD.gn文件，大家可以将该文件理解为一个管理者，它管理app目录中的每个子目录，通过这个BUILD.gn文件中的features字段可以决定哪一个子目录中的BUILD.gn中指定的源文件会被编译到烧录文件中，如下图所示：
  > 前面带`#`的是注释掉的，不会被编译，没有`#`的是要被编译的文件

  ![Alt text](./图床/12.png)

- 假设我们要将app/B1_led目录中的led_example.c文件编译到烧录程序中，需要打开app/B1_led/BUILD.gn文件查看该文件中的源代码被为静态库的名称，可以看到名称为`myled`，如下图所示：
  ![Alt text](./图床/13.png)

- 这时我们将app/BUILD.gn文件中后面加上`B1_led:myled`就大功告成啦！如下图所示：
  > 别忘了要把之前的文件前加个`#`

  ![Alt text](./图床/14.png)

- .gn文件的feature字段格式为：模块源文件所在路径:模块名称
请注意：.gn文件中的空白都是空格，不是Tab键（制表符），如果您输入了制表符，在生成ninja文件时就会产生如下图所示错误：
  ![Alt text](./图床/15.png)

- 未完成待续......

## Hi3861的基础内核开发
**内核开发文档编写主要借鉴：**
> [OpenHarmony智能开发套件[内核编程·上]](https://ost.51cto.com/posts/23938)  
> [OpenHarmony智能开发套件[内核编程·下]](https://ost.51cto.com/posts/23997)  
> [BearPi-HM_Nano案例开发](https://gitee.com/bearpi/bearpi-hm_nano/blob/master/applications/BearPi/BearPi-HM_Nano/sample/README.md#/bearpi/bearpi-hm_nano/blob/master/applications/BearPi/BearPi-HM_Nano/sample/A1_kernal_thread/README.md)  
> [openharmony编写“Hello World”程序](https://docs.openharmony.cn/pages/v4.0/zh-cn/device-dev/quick-start/quickstart-ide-3861-helloworld.md/)  

### 什么是内核？

 什么是内核？或者说内核在一个操作系统中起到一个什么样的作用？相信初次接触这个词的伙伴们也会有同样的疑问。不过不用担心，我会尽可能地通俗地介绍内核的相关知识，以便大家能够更好地去体会内核编程。

​ 我们先来看一张图，这是OpenHarmony官网发布的技术架构图
![Alt text](./图床/16.png)

 我们可以看到最底层叫做内核层，有Linux，LiteOS等。内核在整个架构，或者操作系统中起到一个核心作用，他负责管理计算机系统内的资源和硬件设备，提供给顶层的应用层一个统一规范的接口，从而使得整个系统能够完成应用与硬件的交互。

​ 具体点来说，内核可以做以下相关的工作：
  >  1. 进程管理  
  >  2. 内存管理  
  >  3. 文件资源管理  
  >  4. 网络通信管理  
  >  5. 设备驱动管理  

​当然不局限于这些，这里只是给出具体的例子供大家理解，如果实在难以理解，那么我再举一个例子，进程。可能你没听过进程，但你一定打开过任务管理器。
  ![Alt text](./图床/17.png)

  这些都是进程，一个进程又由多个线程组成。那么CPU，内存，硬盘，网络这些硬件层面资源是怎么合理分配到我们软件的各个进程中呢？这就是内核帮助我们完成的事情，我们并不关心我们设备上的应用在哪里执行，如何分配资源，内核会完成这些事情。我们日常与软件交互，而内核会帮助我们完成软件和硬件的交互。

#### Openharmony内核
明白了什么是内核后，我们来看看OpenHarmony的内核是怎么样设计的吧。

​OpenHarmony采用的是多内核设计 有基于Linux内核的标准系统，有基于LiteOS-A的小型系统，也有基于LiteOS-M的轻量系统。他们分别适配不同的设备，比如说智能手表就是轻量级别的，智能汽车就是标准级别的等等。本篇并不介绍标准系统和小型系统，轻量系统更加适合初学者。也就是Hi3861所搭载的系统。

#### LiteOS-M内核

​ 下面是一张LiteOS-M的架构图
  ![Alt text](./图床/18.png)
目录结构如下：
> 注意：这里说的是openharmony完整源码，我们在学习bearpi的时候不需要完整的源码，完整源码下有：linux，liteos_a，liteos_m，uniproton，四个内核
```
/kernel/liteos_m
├── arch                 # 内核指令架构层目录
│   ├── arm              # arm 架构代码
│   │   ├── arm9         # arm9 架构代码
│   │   ├── cortex-m3    # cortex-m3架构代码
│   │   ├── cortex-m33   # cortex-m33架构代码
│   │   ├── cortex-m4    # cortex-m4架构代码
│   │   ├── cortex-m55   # cortex-m55架构代码
│   │   ├── cortex-m7    # cortex-m7架构代码
│   │   └── include      # arm架构公共头文件目录
│   ├── csky             # csky架构代码
│   │   └── v2           # csky v2架构代码
│   ├── include          # 架构层对外接口存放目录
│   ├── risc-v           # risc-v 架构
│   │   ├── nuclei       # 芯来科技risc-v架构代码
│   │   └── riscv32      # risc-v官方通用架构代码
│   └── xtensa           # xtensa 架构代码
│       └── lx6          # xtensa lx6架构代码
├── components           # 可选组件
│   ├── backtrace        # 栈回溯功能
│   ├── cppsupport       # C++支持
│   ├── cpup             # CPUP功能
│   ├── dynlink          # 动态加载与链接
│   ├── exchook          # 异常钩子
│   ├── fs               # 文件系统
│   ├── lmk              # Low memory killer 机制
│   ├── lms              # Lite memory sanitizer 机制
│   ├── net              # Network功能
│   ├── power            # 低功耗管理
│   ├── shell            # shell功能
│   └── trace            # trace 工具
├── drivers              # 驱动框架Kconfig
├── kal                  # 内核抽象层
│   ├── cmsis            # cmsis标准接口支持
│   └── posix            # posix标准接口支持
├── kernel               # 内核最小功能集支持
│   ├── include          # 对外接口存放目录
│   └── src              # 内核最小功能集源码
├── testsuites           # 内核测试用例
├── tools                # 内核工具
├── utils                # 通用公共目录
```
具体请参考：[轻量系统内核概述](https://docs.openharmony.cn/pages/v4.0/zh-cn/device-dev/kernel/kernel-mini-overview.md/)

下面重点介绍KAL抽象层 和 基础内核的操作

##### KAL抽象层
​ 相信大家还是会有疑惑，什么是KAL抽象层？  
​ Kernel Abstraction Layer

​ 在刚刚的内核中我们提到了，内核主要完成的是软件与硬件的交互，他会给应用层提供统一的规范接口，而KAL抽象层正是内核对应用层提供的接口集合。应用程序可以通过KAL抽象层完成对硬件的控制交互。

​ 抽象层是因为他隐藏了与硬件接口具体的交互逻辑，开发人员只需要关心如何操作硬件，而无需关心硬件底层的细节，大大提高了可移植性和维护性。

​ 以我的角度去看，KAL简单来说就是一堆接口，帮助你去操控硬件。CMSIS与POSIX就是具有统一规范的一些接口。通过他们我们就可以去控制一些基础的内核，线程，软件定时器，互斥锁，信号量等等。概念就先简单介绍这么多，感兴趣的伙伴们可以上官网查看更多的关于OpenHarmony内核的信息。下面我会带着大家编码操作，从实际去体会内核编程。


### 线程管理
在我们初次接触到openharmony轻量系统开发（hi3861开发）的时候，第一个接触的肯定是如何使用线程来完成我们的任务，那么接下来就让我们开始利用线程来跟他说一声“Hello,openharmony!”

​ 在管理线程前，我们需要了解线程，线程是调度的基本单位，具有独立的栈空间和寄存器上下文，相比与进程，他是轻量的。举一个实际的例子，动物园卖票。

​ 对于动物园卖票这件事本身而言是一个进程，而每一个买票的人可以看作一个线程，在多个售票口处，我们并发执行，并行计算，共同消费动物园的门票，像享受共同的内存资源空间一样。为什么要线程管理呢？你我都希望买到票，但是票有限，我们都不希望看到售票厅一篇混乱，因此对线程进行管理是非常重要的一件事情。

#### 线程状态的介绍
首先介绍一下线程的几个状态，他们分别有：
- 创建
- 就绪（Ready）：该任务在就绪队列中，只等待CPU。  
- 运行（Running）：该任务正在执行。  
- 阻塞（Blocked）：该任务不在就绪队列中。包含任务被挂起（suspend状态）、任务被延时（delay状态）、任务正在等待信号量、读写队列或者等待事件等。  
- 终止（Dead）：该任务运行结束，等待系统回收资源。  
- 任务状态迁移示意图  
  ![Alt text](./图床/19.png)  
  这里我不展开任务状态迁移说明，因为这个不重要，如果你有兴趣了解，可以移步：[轻量系统任务管理](https://docs.openharmony.cn/pages/v4.0/zh-cn/device-dev/kernel/kernel-mini-basic-task.md/)

#### 单线程使用流程
  ​ 回忆第一个helloworld.c的例子  

  ​ 我们要编写的程序样例就在源码根目录下的：`applications/sample/wifi-iot/app/`  

  ​ 下面将具体演示如何编写程序样例。  

  1. 新建样例目录
  `applications/sample/wifi-iot/app/thread_demo`  

  2. 新建源文件和gn文件  
    `applications/sample/wifi-iot/app/thread_demo/Thread.c`  
    `applications/sample/wifi-iot/app/thread_demo/BUILD.gn`  
    假设你现在创建好了文件夹和里面的`Thread.c`，`BUILD.gn`。    

3. 介绍线程
   - 首先我们如何创建线程？
     - 我们想想我们在创建变量时是怎么做的，是不是先写数据类型后跟名字，像：`int a;`
     - 那么线程的创建逻辑跟这个差不多，类似：`osThreadAttr_t attr;`
     - `osThreadAttr_t`是什么？他是一个结构体，定义在`cmsis_os2.h`里面，内容如下：
        ```c
        typedef struct {
          /** Thread name */
          const char                   *name;
          /** Thread attribute bits */
          uint32_t                 attr_bits;
          /** Memory for the thread control block */
          void                      *cb_mem;
          /** Size of the memory for the thread control block */
          uint32_t                   cb_size;
          /** Memory for the thread stack */
          void                   *stack_mem;
          /** Size of the thread stack */
          uint32_t                stack_size;
          /** Thread priority */
          osPriority_t              priority;
          /** TrustZone module of the thread */
          TZ_ModuleId_t            tz_module;
          /** Reserved */
          uint32_t                  reserved;
        } osThreadAttr_t;
        ```
        - 这是线程的结构体，它具有以下属性：
          > `name`：线程的名称。  
            `attr_bits`：线程属性位。  
            `cb_mem`：线程控制块的内存地址。  
            `cb_size`：线程控制块的内存大小。  
            `stack_mem`：线程栈的内存地址。  
            `stack_size`：线程栈的大小。  
            `priority`：线程的优先级。  
            `tz_module`：线程所属的TrustZone模块。  
            `reserved`：保留字段。  
    - 再者如何让线程启动？
      - **回答：** `osThreadNew((osThreadFunc_t)Thread,NULL,&attr)`
      - 他的函数原型长下面这样，这是创建线程的接口函数，他有三个参数，一个返回值，**不需要我们掌握，我们会用就行**
        > `func`：是线程的回调函数，你创建的这个线程会执行这段函数的内容。  
          `arguments`：线程回调函数的参数。  
          `attr`：线程的属性，也就是我们之前创建的线程   
          返回值：线程的id 如果id不为空则说明成功。
        ```c
        osThreadId_t osThreadNew(osThreadFunc_t func, void *argument, const osThreadAttr_t *attr)
        {
            UINT32 uwTid;
            UINT32 uwRet;
            LosTaskCB *pstTaskCB = NULL;
            TSK_INIT_PARAM_S stTskInitParam;

            if (OS_INT_ACTIVE) {
                return NULL;
            }

            if ((attr == NULL) || (func == NULL) || (attr->priority < osPriorityLow1) ||
                (attr->priority > osPriorityAboveNormal6)) {
                return (osThreadId_t)NULL;
            }

            (void)memset_s(&stTskInitParam, sizeof(TSK_INIT_PARAM_S), 0, sizeof(TSK_INIT_PARAM_S));
            stTskInitParam.pfnTaskEntry = (TSK_ENTRY_FUNC)func;
        #ifndef LITEOS_WIFI_IOT_VERSION
            stTskInitParam.uwArg = (UINT32)argument;
        #else
            stTskInitParam.auwArgs[0] = (UINT32)argument;
        #endif
            stTskInitParam.uwStackSize = attr->stack_size;
            stTskInitParam.pcName = (CHAR *)attr->name;
            stTskInitParam.usTaskPrio = OS_TASK_PRIORITY_LOWEST - ((UINT16)(attr->priority) - LOS_PRIORITY_WIN); /* 0~31 */

            uwRet = LOS_TaskCreate(&uwTid, &stTskInitParam);

            if (LOS_OK != uwRet) {
                return (osThreadId_t)NULL;
            }

            pstTaskCB = OS_TCB_FROM_TID(uwTid);

            return (osThreadId_t)pstTaskCB;
        }
        ``` 
    - 最后我们如何将线程终止呢？
      - `osStatus_t osThreadTerminate (osThreadId_t thread_id);`
      - 显然我们只要传入线程的id就会让该线程终止，返回值是一个状态码
      - 我这里给出全部的状态码
        ```c
        typedef enum {
          /** Operation completed successfully */
          osOK                      =  0,
          /** Unspecified error */
          osError                   = -1,
          /** Timeout */
          osErrorTimeout            = -2,
          /** Resource error */
          osErrorResource           = -3,
          /** Incorrect parameter */
          osErrorParameter          = -4,
          /** Insufficient memory */
          osErrorNoMemory           = -5,
          /** Service interruption */
          osErrorISR                = -6,
          /** Reserved. It is used to prevent the compiler from optimizing enumerations. */
          osStatusReserved          = 0x7FFFFFFF
        } osStatus_t;
        ``` 
4. 编写源码
    - `Thread.c`
        ```c
        //先导入头文件
        #include <stdio.h>
        #include "ohos_init.h"
        #include "ohos_types.h"

        //定义线程优先级
        #define myThread_priority 24  

        //创建功能实现的函数
        void Hello_openharmony(void)
        {
          while(1)
          {
            printf("Hello openharmony!\n");
            osDelay(100);
          }
        }

        //创建一个静态函数来定义和管理线程
        static void Task(void)
        {
          //创建线程
          osThreadAttr_t attr;

          attr.name = "myThread";    //建立线程当中的第一个任务
          attr.attr_bits = 0U;           //线程属性位
          attr.cb_mem = NULL;       //用户指定的控制块指针
          attr.cb_size = 0U;        //用户指定的控制块大小，单位：字节
          attr.stack_mem = NULL;    //用户指定的线程栈指针
          attr.stack_size = 1024*4;        //线程栈大小，单位：字节
          attr.priority = myThread_priority;       //线程优先级

          //线程启动任务
          osThreadId_t ID = osThreadNew((osThreadFunc_t)Hello_openharmony, NULL, &attr);
          if (ID == NULL) {  
              printf("Failed to create hello_openharmony Thread!\n");
          }

          // 休眠5秒
          osDelay(500);
          // 终止线程
          osStatus_t status = osThreadTerminate(ID);
          printf("[Thread Test] printThread stop, status = %d.\r\n", status);
        }
        APP_FEATURE_INIT(Task);
        ```
    - `BUILD.gn`
      ```
      static_library("my_first_thread"){  # 取名字
          sources = [
              "Thread.c"   # 要编译的源文件
          ]
          include_dirs = [
              "//utils/native/lite/include"  # 头文件的路径
          ]
      }
      ``` 

#### 多线程使用流程

其实他跟单线程是一样的，只不过我们可以省略一部分线程结构体的定义，类似于：
  ```c
  osThreadAttr_t attr;
  attr.attr_bits=0U;
  attr.cb_mem=NULL;
  attr.cb_size=0U;
  attr.stack_mem=NULL;
  attr.stack_size=1024*4;
  attr.priority=25;

  attr.name=Thread_1;
  if (osThreadNew((osThreadFunc_t)Thread_1,NULL,&attr)==NULL){
      printf("Failed to create Theard_1!\r\n");
  }

  attr.name=Thread_2;
  if (osThreadNew((osThreadFunc_t)Thread_2,NULL,&attr)==NULL){
      printf("Failed to create Thread_2!\r\n");
  }

  attr.name=Thread_3;
  if (osThreadNew((osThreadFunc_t)Thread_3,NULL,&attr)==NULL){
      printf("Failed to create Thread_3!\r\n");
  }
  ```
  这里我只是把名字重新定义了一下，但是我创建了3个线程，如果我想改变第2个的线程的优先级，第3个线程的栈大小（一般不做更改），怎么改？我在下面给个修改示例：
  ```c
  osThreadAttr_t attr;
  attr.attr_bits=0U;
  attr.cb_mem=NULL;
  attr.cb_size=0U;
  attr.stack_mem=NULL;
  attr.stack_size=1024*4;
  attr.priority=25;

  attr.name=Thread_1;
  if (osThreadNew((osThreadFunc_t)Thread_1,NULL,&attr)==NULL){
      printf("Failed to create Theard_1!\r\n");
  }

  attr.name=Thread_2;
  attr.priority = 24;   //更改优先级为24
  if (osThreadNew((osThreadFunc_t)Thread_2,NULL,&attr)==NULL){
      printf("Failed to create Thread_2!\r\n");
  }

  attr.name=Thread_3;
  attr.stack_size=216*4;  //更改栈大小，但是一般不做更改
  if (osThreadNew((osThreadFunc_t)Thread_3,NULL,&attr)==NULL){
      printf("Failed to create Thread_3!\r\n");
  }
  ```

#### 多线程的封装
在处理业务的时候，我们一般是多线程的背景，下面我将创建线程函数封装起来，方便大家创建多线程
```c
osThreadId_t newThread(char *name, osThreadFunc_t func, void *arg){
    // 定义线程和属性
    osThreadAttr_t attr = {
        name, 0, NULL, 0, NULL, 1024, osPriorityNormal, 0, 0
    };
    // 创建线程
    osThreadId_t tid = osThreadNew(func, arg, &attr);
    if(tid == NULL){
        printf("[newThread] osThreadNew(%s) failed.\r\n", name);
    }
    return tid;
}
```
线程部分先体会到这里，想要探索更过线程相关的API，这里提供了API网站，供大家参考学习。
1. [CMSIS_OS2 Thread API](https://arm-software.github.io/CMSIS_5/RTOS2/html/os2MigrationFunctions.html#mig_threadMgmt)  
2. [Openharmony官方CMSIS_API文档](https://docs.openharmony.cn/pages/v4.0/zh-cn/device-dev/reference/kernel/cmsis/_c_m_s_i_s-_r_t_o_s.md/)  

### 软件定时器

#### 软件定时器的介绍
​ 下面我们介绍软件定时器，老样子我们先来介绍以下软件定时器。软件定时器是一种在软件层面上实现的计时器机制，用于在特定的时间间隔内执行特定的任务或触发特定的事件。它不依赖于硬件定时器，而是通过软件编程的方式实现。举一个例子，手机应用。

​ 当你使用手机上的某个应用时，你可能会注意到，如果你在一段时间内没有进行任何操作，应用程序会自动断开连接并要求你重新登录。这是为了保护你的账号安全并释放服务器资源。类似的设定都是有软件定时器实现的，下面进行实际操作，让大家体会一下软件定时器。

#### 软件定时器使用流程
1. 新建样例目录
    `applications/sample/wifi-iot/app/time_demo`  

2. 新建源文件和gn文件
    `applications/sample/wifi-iot/app/time_demo/time.c`   
    `applications/sample/wifi-iot/app/time_demo/BUILD.gn`

3. 介绍定时器
    - 如何创建软件定时器？
        ```c
        osTimerId_t osTimerNew (osTimerFunc_t func, osTimerType_t type, void *argument, const osTimerAttr_t *attr);
        ``` 
        >  `func`: 软件定时器的回调函数  
            `type`：软件定时器的种类  
            `argument`：软件定时器回调函数的参数
            `attr`：软件定时器的属性  
            返回值：返回软件定时器的id， id为空则说明软件定时器失败  

        软件定时器的种类有两个，分为一次性定时器和周期性定时器，一次性在执行完回调函数后就会停止计数，而周期性定时器会重复触发，每次触发重新计时。根据不同的需求我们可以选择使用不同的软件定时器。（下面是定时器的类型）
        ```c
        typedef enum {
          /** One-shot timer */
          osTimerOnce               = 0,  //一次性定时器
          /** Repeating timer */
          osTimerPeriodic           = 1   //周期性定时器
        } osTimerType_t;
        ```
    - 如何启动软件定时器？
      ```c
      osStatus_t osTimerStart (osTimerId_t timer_id, uint32_t ticks);
      ``` 
      > `timer_id`：软件定时器的参数，指定要启动哪个软件定时器  
        `ticks`：等待多少个ticks执行回调函数，在Hi3861中 100个ticks为1秒  
        返回值：软件定时器的状态码，在线程部分已经展示给大家了全部的状态码  

    - 如何停止定时器?
      ```c
      osStatus_t osTimerStop (osTimerId_t timer_id);
      ``` 
      > 这个函数很简单，只需要传软件定时器的id，即可停止软件计时器，并且返回他的状态码
    - 如何删除定时器？
      ```
      osStatus_t osTimerDelete (osTimerId_t timer_id);
      ``` 
    - 编写源码
      - `time.c`
        ```c
        //导入头文件
        #include <stdio.h>
        #include <string.h>
        #include <unistd.h>

        #include "cmsis_os2.h"
        #include "ohos_init.h"

        //定义定时器触发的任务函数
        void Time1back(void)
        {
            printf("This is BearPi Timer1Callback!\n");
        }
        void Time2back (void)
        {
            printf("This is BearPi Timer2Callback!\n");
        }

        //在这里面开展定时器的流程
        static void time_example_2(void)
        {
            osTimerId_t ID1,ID2;   //定义定时器的ID，类似于osThreadAttr_t attr;
            uint32_t timerDelay;   //uint32_t == (unsigned int) 32B==4字节
            osStatus_t status;     //定义状态量

            ID1=osTimerNew(Time1back, osTimerOnce, NULL, NULL);  //(任务函数名，定时器类型，仅限于osTimerOnce(单次定时器)或osTimerPeriodic，定时器回调函数参数，定时器属性，当前不支持该参数，参数可为NULL )
            if (ID1 != NULL)
            {
                timerDelay=100U; //（100U == 1S）

                status= osTimerStart(ID1,timerDelay);  
                // 定时器开始运行函数 osOK，表示执行成功。
                //  osErrorParameter，表示参数错误。
                //  osErrorResource，表示其他错误。
                //  osErrorISR，表示在中断中调用本函数。
                //osTimerStop,osTimerDelete也是一样。
                if (status != osOK) {
                    printf("Failed to start Timer1!\r\n");
                }
            }
            osDelay(100U);   //延迟1秒

            //STOP（暂停定时器）
            status = osTimerStop(ID1);  //暂停定时器并将返回值存入状态量
            if (status == osOK){
                printf("the time1 is stop success\r\n");
            }
            else{
                printf("the time1 is stop failed\r\n");
            }

            //Delete（删除定时器）
            status = osTimerDelete(ID1);   //删除定时器并将返回值存入状态量
            if (status == osOK){
                printf("the time1 is delect success\r\n");
            }
            else{
                printf("the time1 is delect failed\r\n");
            }
            
            ID2=osTimerNew(Time2back, osTimerPeriodic, NULL, NULL);
            if (ID2 != NULL)
            {
                timerDelay=300U;

                status= osTimerStart(ID2,timerDelay);
                if (status != osOK) {
                    printf("Failed to start Timer2!\r\n");
                }
            }
            osDelay(600U);
            status = osTimerStop(ID2);
            if (status == osOK){
                printf("the time2 is stop success!\r\n");
            }
            else{
                printf("the time2 is stop failed\r\n");
            }
            status = osTimerDelete(ID2);
            if (status == osOK){
                printf("the time2 is delect success\r\n");
            }
            else{
                printf("the time2 is delect failed\r\n");
            }
        }
        APP_FEATURE_INIT(time_example_2);
        ``` 
      - `BUILD.gn`
        ```
        static_library("mytime") {
            sources = [
                "time.c"
            ]
            include_dirs = [
                "//utils/native/lite/include"
            ]
        }
        ``` 
      - 执行效果
         ![Alt text](./图床/26.png)
软件定时器的API相对较少，这里还是提供所有的软件定时器API
1. [CMSIS_OS2 Timer API](https://arm-software.github.io/CMSIS_5/RTOS2/html/os2MigrationFunctions.html#mig_timer)

### 互斥锁
在学习互斥锁之前，我们有必要去再看一下线程的状态这一小节，也就是我们要了解线程的生命周期，防止我们对线程的生命周期不了解而对互斥锁的学习感到困难。

#### 互斥锁介绍
互斥锁的应用场景时处理多线程，资源访问的问题。这里还是给大家举一个例子：动物园卖票。

​ 在我们的程序设计中，往往会有共有资源，每个线程都可以进来访问这些资源。这里的共有资源就是我们的门票，一个售票口就是一个线程，当有人来窗口买票，我们的门票就会减少一张。当然一个动物园的流量时巨大的，我们不能只设立一个售票口，这样的效率是很低的。京东淘宝抢购秒杀也一样，我们必须设立多个窗口，在同一时刻为多个人处理业务。多线程解决了效率问题但也带来了安全隐患。

​ 我们假设一个这样的场景，动物园仅剩一张门票，但是有两个在不同的窗口同时付了钱，当售票员为他们拿票的时候就会发现，少了一张票，两个人都付了钱都想要那张票，就陷入了一个死局，无奈动物园只能让他们都进去。但是在程序中体现的可能就是一个bug，动物园的门票剩余数为：-1。

​ 售票的逻辑很简单，只要票数大于零，还有票能卖，那就在此基础上减掉一。问题就在于，**两个线程同时在count = 1的时候通过了if判断，都执行的count–；那么就会出现了count = -1，也就是票数为负 的bug结果。**

​ 互斥锁的出现就很好地解决了一个问题，他能够阻止上面两个线程同时访问资源的同步行为，也就是说当一个线程进入这个if语句后，别的线程都不能进入。形象起来说，就像对这段代码加了锁，只有有钥匙的线程才能够访问它。  
![Alt text](./图床/23.png)
![Alt text](./图床/20.png)


#### 互斥锁的使用流程
1. 新建样例目录  
`applications/sample/wifi-iot/app/mutex_demo`  

2. 新建源文件和gn文件  
`applications/sample/wifi-iot/app/mutex_demo/mutex.c`  
`applications/sample/wifi-iot/app/mutex_demo/BUILD.gn`  

3. 介绍互斥锁
   - 如何创建互斥锁？
      >  与线程的定义一样，互斥锁也封装成了一个结构体

      ```c
      typedef struct {
        /** Mutex name */
        const char                   *name;
        /** Reserved attribute bits */
        uint32_t                 attr_bits;
        /** Memory for the mutex control block */
        void                      *cb_mem;
        /** Size of the memory for the mutex control block */
        uint32_t                   cb_size;
      } osMutexAttr_t;
      ```
      > `name`：互斥锁的名称  
        `attr_bits`：保留的属性位  
        `cb_mem`：互斥锁控制块的内存  
        `cb_size`：互斥锁控制块内存的大小 

    - 如何获取互斥锁的ID？
       ```c
       osMutexId_t osMutexNew(const osMutexAttr_t *attr);
       ```
    - 线程如何获取互斥锁？
       ```c
       osStatus_t osMutexAcquire(osMutexId_t mutex_id, uint32_t timeout);
       ```
       ​ 传入互斥锁id，设置我们的延迟时间，当线程获取到我们的互斥锁时，返回的状态是osOK。
        下面是全部的状态码（我这里在介绍一次，以免遗忘）
        ```
        typedef enum {
        /** Operation completed successfully */
        osOK                      =  0,
        /** Unspecified error */
        osError                   = -1,
        /** Timeout */
        osErrorTimeout            = -2,
        /** Resource error */
        osErrorResource           = -3,
        /** Incorrect parameter */
        osErrorParameter          = -4,
        /** Insufficient memory */
        osErrorNoMemory           = -5,
        /** Service interruption */
        osErrorISR                = -6,
        /** Reserved. It is used to prevent the compiler from optimizing enumerations. */
        osStatusReserved          = 0x7FFFFFFF
      } osStatus_t;
        ```
    - 如何释放互斥锁？
      ```c
      osStatus_t osMutexRelease(osMutexId_t mutex_id);
      ``` 
      ​ 当我们的线程执行完业务后需要把锁释放出来，让别的线程获取锁，执行业务。当然这个过程是线程之间的竞争，一个线程可能一直得不到锁，一个线程也可能刚释放锁又获得锁，我们可以添加休眠操作，提高锁在各个线程间的分配。
      其他API请参考：[Mutex API](https://arm-software.github.io/CMSIS_5/RTOS2/html/os2MigrationFunctions.html#mig_mutex)

4. 编写源码
    - `mutex.c`
      ```c
      /*  实现效果
      整体实现的效果是高优先级任务与低优先级任务抢占互斥锁资源，首先因为高优先级和中优先级都延迟
      了1秒，所以低优先级任务第一个执行，在低优先级执行了1秒，中优先级执行（1秒打印一次），高优
      先级因为低优先级占据互斥锁资源而无法启动，在低优先级再执行2秒（共3秒）后释放互斥锁资源，
      高优先级得到资源开始执行，期间中优先级一直执行打印，高优先级与低优先级3秒交替打印
      */
      #include<stdio.h>
      #include <string.h>
      #include <unistd.h>

      #include "cmsis_os2.h"
      #include "ohos_init.h"

      osMutexId_t muxe_id;     //创建互斥锁ID

      //创建高优先级线程的回调函数
      void HIGH_Thread(void)
      {
          osDelay(100U);
          while(1){
              osMutexAcquire(muxe_id,osWaitForever);  //获取互斥锁资源
              printf("HIGH_Thread is running!\r\n");
              osDelay(300U);
              osMutexRelease(muxe_id);   //释放互斥锁资源
          }
      }

      //创建中优先级线程的回调函数
      void MID_Thread(vboid)
      {
          osDelay(100U);
          while(1){
              printf("MID_Thread is running!\r\n");
              osDelay(100U);
          }
      }

      //创建低优先级线程的回调函数
      void LOW_Thread(void)
      {
          while(1){
              osMutexAcquire(muxe_id,osWaitForever);  //获取互斥锁资源
              printf("LOW_Thread is running!\r\n");
              osDelay(300U);
              osMutexRelease(muxe_id);   //释放互斥锁资源
          }
      }

      static void muxe_example(void)
      {
          // 创建互斥锁，互斥锁的attr结构体可以为NULL（无）
          muxe_id=osMutexNew(NULL);  //获取互斥锁的ID
          if (muxe_id == NULL){
              printf("Failed to create Muxe!\r\n");
          }

          osThreadAttr_t attr;
          attr.attr_bits=0U;
          attr.cb_mem=NULL;
          attr.cb_size=0U;
          attr.stack_mem=NULL;
          attr.stack_size=1024*4;

          attr.name = "Thread_1";
          attr.priority = 24;     //高优先级
          if (osThreadNew((osThreadFunc_t)HIGH_Thread,NULL,&attr) == NULL){
              printf("Failed to create Thread_1!\r\n");
          }

          attr.name = "Thread_2";  //中优先级
          attr.priority = 25;
          if (osThreadNew((osThreadFunc_t)MID_Thread,NULL,&attr) == NULL){
              printf("Failed to create Thread_2!\r\n");
          }

          attr.name = "Thread_3";
          attr.priority = 26;  //低优先级
          if (osThreadNew((osThreadFunc_t)LOW_Thread,NULL,&attr) == NULL){
              printf("Failed to create Thread_3!\r\n");
          }
      }
      APP_FEATURE_INIT(muxe_example);
      ``` 
    - `BUILD.gn`
      ```
      static_library("mymuxe") {
          sources = [
              "muxe_example.c"
          ]
          include_dirs = [
              "//utils/native/lite/include"
          ]
      }
      ``` 
    - 执行效果
      > 代码编译烧录代码后，按下开发板的RESET按键，通过串口助手查看日志，中优先级任务一直正常运行，而高优先级和低优先级的任务因为互相抢占互斥锁，交替运行。 
      ```
      LowPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing.
      HighPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing.
      LowPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing.
      HighPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing.
      MidPrioThread is runing
      ``` 
#### 互斥锁案例体验
我这里粘贴一个佬写的代码，大家可以学习借鉴一下！
- `mutex_zoor.c`
  ```c
  #include <stdio.h>
  #include <unistd.h>
  #include "ohos_init.h"
  #include "cmsis_os2.h"

  // 模拟动物园门票数
  static int count = 100;

  // 售票业务线程
  void outThread(void *args){
      // 获取互斥锁
      osMutexId_t *mid = (osMutexId_t *)args;
      // 每个线程都在不停地买票
      while(1){
          // 获取锁，进入业务流程
          if(osMutexAcquire(*mid, 100) == osOK){
              if(count > 0){
                  count--;
                  // 设置提示信息
                  printf("[Mutex Test] Thread %s get a value, the less is %d.\r\n", osThreadGetName(osThreadGetId()), count);
              } else {
                  // 告知这些线程已经没有门票卖了，线程结束
                  printf("[Mutex Test] The value is out!\r\n");
                  osThreadTerminate(osThreadGetId());
              }
          }
          // 释放锁
          osMutexRelease(*mid);

          osDelay(5);
      }
  }
  // 创建线程封装
  osThreadId_t createThreads(char *name, osThreadFunc_t func, void *args){
      osThreadAttr_t attr = {
          name, 0, NULL, 0, NULL, 1024, osPriorityNormal, 0, 0
      };
      osThreadId_t tid = osThreadNew(func, args, &attr);
      return tid;
  }

  // 主函数实现多线程的创建，执行买票业务
  void mutexMain(void){
      // 创建互斥锁
      osMutexAttr_t attr = {0};

      // 获取互斥锁的id
      osMutexId_t mid = osMutexNew(&attr);

      if(mid == NULL){
          printf("[Mutex Test] Failed to create a mutex!\r\n");
      }

      // 创建多线程
      osThreadId_t tid1 = createThreads("Thread_1", (osThreadFunc_t)outThread, &mid);
      osThreadId_t tid2 = createThreads("Thread_2", (osThreadFunc_t)outThread, &mid);
      osThreadId_t tid3 = createThreads("Thread_3", (osThreadFunc_t)outThread, &mid);

      osDelay(1000);

  }

  // 测试线程
  void MainTest(void){
      osThreadId_t tid = createThreads("MainTest", (osThreadFunc_t)mutexMain, NULL);
  }

  APP_FEATURE_INIT(MainTest);
  ``` 
  - 结果展示
    ![Alt text](./图床/21.png) 

- 可能有的伙伴们看到这里不太清晰，会觉得这段代码真的上锁了吗(下面这一段代码)
  ```c
   if(osMutexAcquire(*mid, 100) == osOK){
            if(count > 0){
                count--;
                printf("[Mutex Test] Thread %s get a value, the less is %d.\r\n", osThreadGetName(osThreadGetId()), count);
            } else {
                printf("[Mutex Test] The value is out!\r\n");
                osThreadTerminate(osThreadGetId());
            }
        }
  ``` 
- 那么我们可以不使用互斥锁再次执行这段代码
  ```c
  while(1){
        // 获取锁，进入业务流程
        // if(osMutexAcquire(*mid, 100) == osOK){
            if(count > 0){
                count--;
                printf("[Mutex Test] Thread %s get a value, the less is %d.\r\n", osThreadGetName(osThreadGetId()), count);
            } else {
                printf("[Mutex Test] The value is out!\r\n");
                osThreadTerminate(osThreadGetId());
            }
        //}
        // 释放锁
        //osMutexRelease(*mid);
        //osDelay(5);
    }
  ``` 
  - 结果展示
    ![Alt text](./图床/22.png) 

### 信号量

#### 信号量介绍
​ 对大部分初学者而言，这又是一个新名词，什么是信号量？其实他跟我们上篇介绍的互斥锁很像。互斥锁是在多线程中允许一个线程访问资源，信号量是在多线程中允许多个线程访问资源。  
![Alt text](./图床/24.png)

​ 初学者一定会感到困惑，为了解决多线程访问资源的风险我们限制只能有一个线程在某一时刻访问资源，现在这个信号量怎么有允许多个线程访问资源呢。笔者刚开始也比较困惑，结合一些案例理解后，也是明白了这样的设计初衷。实际上，信号量，互斥锁本就是两种不同的多形成同步运行机制，在特定的应用场景下，有特定的需求，而信号量，互斥锁可以满足不同的需求，具体是什么需求呢，举个例子给大家。

​ 卖票，我们的确需要互斥锁解决多线程可能带来的错误，那么如果是验票呢，为了提高效率，我们开设多个入口同时验票且不会发生冲突，信号量就做到了限制线程数量访问资源的作用。如果我们不限制并发的数量，我们的程序占用资源可能会非常大，甚至崩溃，就像检票的入口没有被明确入口数量一样，门口的人们会乱成一片。
#### 信号量的使用流程
1. 新建样例目录
  `applications/sample/wifi-iot/app/semaphore_demo`  

2. 新建源文件和gn文件  
  `applications/sample/wifi-iot/app/semaphore_demo/semaphore.c`  
  `applications/sample/wifi-iot/app/semaphore_demo/BUILD.gn`

3. 介绍信号量
   - 如何创建信号量？
     - 我们回顾一下在创建互斥锁的时候会有ID这样一个说法，那么信号量也是如此。
     - 先创建ID
       ```c
       osSemaphoreId_t osSemaphoreNew(uint32_t max_count, uint32_t initial_count, const osSemaphoreAttr_t *attr);
       ``` 
       > `max_count`：最大容量量，我们的资源最大能被多少线程访问  
         `initial_count`：初始容纳量，我们当前实际能有多少线程访问资源，因为一个信号对应一个线程的许可。  
         `attr`：信号量属性，一般填NULL  
         返回值：信号量的id  
     - 获取信号量
       ```
       osStatus_t osSemaphoreAcquire(osSemaphoreId_t semaphore_id, uint32_t timeout);
       ``` 
       > `semaphore_id`：信号量的id  
          `timeout`：等待时长，我们往往会在timoeout处设置为 oswaitForever  
          返回值：信号量的状态  
      - 释放信号量
        ```c
        osStatus_t osSemaphoreRelease(osSemaphoreId_t semaphore_id);
        ``` 
        > 很简单，传入信号量的id，就可以释放一个信号量出来。

      其他的API请参考：[Semaphore API](https://arm-software.github.io/CMSIS_5/RTOS2/html/os2MigrationFunctions.html#mig_sem)

4. 编写源码
    - `semaphore.c`
      ```c
      #include <stdio.h>
      #include <string.h>
      #include <unistd.h>

      #include "cmsis_os2.h"
      #include "ohos_init.h"

      osSemaphoreId_t ID;

      void Thread_1(void)
      {
          while(1){
            //释放两次ID信号量，使得Thread_2和Thread_3能同步执行
            //此处若只释放一次信号量，则Thread_2和Thread_3会交替运行
              osSemaphoreRelease(ID);
              osSemaphoreRelease(ID);
              printf("Release success!\r\n");
              osDelay(100);
          }
          
      }
      void Thread_2(void)
      {
          while(1){
            
              //申请ID信号量
              osSemaphoreAcquire(ID,osWaitForever);
              printf("Thread_2 Acquire success!\r\n");
              osDelay(1);
          }
      }
      void Thread_3(void)
      {
          while(1){
              //申请ID信号量
              osSemaphoreAcquire(ID,osWaitForever);
              printf("Thread_3 Acquire success!\r\n");
              osDelay(1);
          }
      }
      void semphore (void)
      {
          osThreadAttr_t attr;
          attr.attr_bits=0U;
          attr.cb_mem=NULL;
          attr.cb_size=0U;
          attr.stack_mem=NULL;
          attr.stack_size=1024*4;
          attr.priority=25;

          attr.name=Thread_1;
          if (osThreadNew((osThreadFunc_t)Thread_1,NULL,&attr)==NULL){
              printf("Failed to create Theard_1!\r\n");
          }

          attr.name=Thread_2;
          if (osThreadNew((osThreadFunc_t)Thread_2,NULL,&attr)==NULL){
              printf("Failed to create Thread_2!\r\n");
          }

          attr.name=Thread_3;
          if (osThreadNew((osThreadFunc_t)Thread_3,NULL,&attr)==NULL){
              printf("Failed to create Thread_3!\r\n");
          }

          ID=osSemaphoreNew(4,0,NULL);
          if (ID == NULL){
              printf("Failed to create ID!\r\n");
          }
      }
      APP_FEATURE_INIT(semphore);
      ``` 
    - `BUILD.gn`
      ```
      static_library("mysemaphore_2") {
          sources = [
              "semaphore_2.c"
          ]
          include_dirs = [
              "//utils/native/lite/include"
          ]
      }
      ``` 
    - 执行效果
      ![Alt text](./图床/25.png)

### 消息队列

#### 消息队列介绍 

#### 消息队列使用流程

### 说明
    

目前正在更新Hi3861的资料