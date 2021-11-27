# LLVMLearning - 第一章

## 构建和配置LLVM

1、在llvm源码附近创建安装文件和构建文件
```
mkdir llvm_install
mkdir llvm-build
cd llvm-build
```

2、使用cmake生成llvm构建所需要的文件
```
cmake ../llvm-project/llvm
```

3、开始构建
```
cmake --build .
```
--build选项告诉cmake调用基础构建工具（make、ninja、xcodebuild、msbuild等）

4、安装
```
cmake --build . --target install
```
可以通过调用构建目录中生成的cmake_install.cmake脚本在安装时设置不同的安装前缀：
```
cmake -DCMAKE_INSTALL_PREFIX=/tmp/llvm -P cmake_install.cmake
```

5、使用CMake和ninja来构建
如果没有该工具，使用brew工具
```
brew install cmake
brew install ninja
```
进入build文件，启动CMake
```
cmake /llvmsource -G Ninja -DCMAKE_BUILD_TYPE="Debug" -DCMAKE_INSTALL_PREFIX="../llvm_install"
```
使用ninja或make安装
```
ninja && ninja install
```
或
```
make && make install
```

安装完成后配置环境变量
```
vim 

















