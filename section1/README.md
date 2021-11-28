# LLVMLearning - 第一章

## 构建和配置LLVM

1、在llvm源码附近创建安装文件和构建文件
```
mkdir llvm_install
mkdir llvm-build
cd llvm-build
```


一下步骤很慢 2个小时，推荐CMake和ninja构建项目

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


推荐：
1、使用CMake和ninja来构建
如果没有该工具，使用brew工具
```
brew install cmake
brew install ninja
```

2、进入build文件，启动CMake
```
cmake -G Ninja ../llvm-project/llvm -DCMAKE_BUILD_TYPE="Debug" -DCMAKE_INSTALL_PREFIX=../llvm-install
```
如果报错：Cmake error :generator: NinjaCmake error :generator: Ninja
由于生成了CMakeCache.txt文件，删除后再进行构建即可
DCMAKE_INSTALL_PREFIX：指定构建保存文件路径

3、使用ninja或make安装
```
ninja && ninja install
```
或
```
make && make install
```

4、安装完成后配置环境变量
```
vim 
```


使用Xcode构建，需要花1个小时

1、构建
```
cmake -G Xcode ../llvm 
```


## 编写一个clang插件

1、进入clang-tools文件中，创建一个hb-plugin文件

2、打开CMakeLists.txt文件，将创建的插件名填写进去
```
add_clang_subdirectory(hb-plugin)
```

4、添加插件文件
```
touch HBPlugin.cpp
```

5、在同级目录下创建CMakeList.txt文件，并添加插件内容
```
add_llvm_loadable_module(HBPlugin HBPlugin.cpp)
```
或
```
add_llvm_loadable_module(HBPlugin
    HBPlugin.cpp
    HBPlugin1.cpp
    HBPlugin2.cpp
)
```

6、使用Xcode构建一个工程，用Xcode编写插件
cd clang-xcode
cmake -GXcode -DLLVM_ENNALBLE-PROJECTS=clang ../llvm-project/llvm


7、开始编码
``` c++
#include <iostream>
#include "clang/AST/AST.h"
#include "clang/AST/ASTConsumer.h"
#include "clang/ASTMatchers/ASTMatchers.h"
#include "clang/ASTMatchers/ASTMatchFinder.h"
#include "clang/Frontend/CompilerInstance.h"
#include "clang/Frontend/FrontendPluginRegistry.h"

using namespace clang;
using namespace std;
using namespace llvm;
using namespace clang::ast_matchers;

namespace HBPlugin {
    class HBConsumer : public ASTConsumer {
        
    };
    class HBAction:public PluginASTAction {
    public:
        unique_ptr<ASTConsumer> CreatASTConsumer(CompilerInstance &ci, StringRef iFile) {
            return unique_ptr<HBConsumer> (new HBConsumer(ci));
        }
    };
}

static FrontendPluginRegistry::Add<HBPlugin::HBAction>
X("HBPlugin", "The HBPlugin is my first clang-plugin");
```






















