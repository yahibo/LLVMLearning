# LLVMLearning

1、编译为可执行文件
``` c
#include <stdio.h>

int main() {
 printf("Hello, world\n");
 return 0;
}
```

```
clang hello.c -o hello
```

2、查看驱动程序为完成你的命令调用的所有其他工具
```
clang -### hello.c -o hello
```


3、将多个文件编译为一个可执行文件
main.c:
``` c
#include <stdio.h>
int sum(int x, int y);
int main() {
    int r = sum(3, 4);
    printf("r = %d\n", r);
    return 0;
}
```

sum.c:
``` c
int sum(int x, int y) {
	return x + y;
}
```
```
clang main.c sum.c -o sum
```

4、为每个C源文件生成LLVM位码文件，后缀为.bc
```
clang -emit-llvm -c main.c -o main.bc
clang -emit-llvm -c sum.c -o sum.bc
```

5、为每个C源文件生成LLVM汇编文件，后缀为.ll
```
clang -emit-llvm -S main.c -o main.ll
clang -emit-llvm -S sum.c -o sum.ll
```

6、从位码文件生成目标文件
llc -filetype=obj main.bc -o main.o
llc -filetype=obj sum.bc -o sum.
clang main.o sum.o -o sum













