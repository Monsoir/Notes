# 项目编译过程

## 🛠 Build Phases

将代码转变为可执行文件的最高级别的规则

#### Target Dependencies

build 当前项目的 target 前，先对此处的依赖进行 build

#### Check Pods Manifest.lock

一个 CocoaPods 相关的脚本

#### Compile Sources

参与编译的文件

#### Link Binary With Libraries

包含了所有静态库与动态库，这些库会与编译阶段生成的目标文件进行链接

#### Copy Bundle Resources

将静态资源，如图片和字体，拷贝到 app bundle 中

对于 PNG 格式的图片，可能还会做进一步的优化

#### Code Signing

build 步骤中的最后一步

## 🛠 Build Rules

指定不同的文件该如何进行编译

## 🛠 Build Settings

是对 Build Phases 的每一项更加详细的配置


