# LLVM-clang使用

创建一个C文件
``` c
#include <stdio.h>

#define HIBO_AGE 30

int main(int argc, const char * argv[]) {
    int a = 10;
    int b = 11;
    int c = a + b + HIBO_AGE;
    return 0;
}
```

1、查看编译过程：
```
clang -ccc-print-phases main.m
```

结果：
```
+- 0: input, "main.m", objective-c
+- 1: preprocessor, {0}, objective-c-cpp-output
+- 2: compiler, {1}, ir
+- 3: backend, {2}, assembler
+- 4: assembler, {3}, object
+- 5: linker, {4}, image
6: bind-arch, "x86_64", {5}, image
```

2、运行程序
```
clang -E main.m
```


3、将C编译为可执行文件
```
clang hello.c -o hello
-S -c参数照常工作
```

4、接下来，将C文件编译成LLVM位码文件
```
clang -O3 -emit-llvm hello.c -c -o hello.bc
```

5、运行程序
```
./hello 或 lli hello.bc
```

6、查看LLVM汇编代码
```
llvm-is < hello.bc | less 
```

7、使用LLC代码生成器将程序编译为本地程序集
```
llc hello.bc -o hello.s
```

8、将代码生成很多token
```
clang -fmodules -E -Xclang -dump-tokens main.m
```

9、生成语法树（AST，Abstract Syntax Tree）
```
clang -fmodules -fsyntax-only -Xclang -ast-dump main.m 
```





