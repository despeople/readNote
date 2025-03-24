# sizeof使用指南

`sizeof()` 是一个在 C 语言中非常有用的运算符，用于获取数据类型或变量在内存中所占用的字节数。它可以用于以下情况：

1. **数据类型：** 对于基本数据类型、结构体、联合体等，`sizeof()` 可以返回它们在内存中所占用的字节数。

   ```c
   printf("Size of int: %zu bytes\n", sizeof(int));
   printf("Size of double: %zu bytes\n", sizeof(double));
   ```

2. **变量：** 对于变量，`sizeof()` 返回变量所占用的字节数。

   ```c
   int x;
   printf("Size of x: %zu bytes\n", sizeof(x));
   ```

3. **指针：** 对于指针，`sizeof()` 返回指针本身所占用的字节数，而不是指针指向的对象所占用的字节数。

   ```c
   int *ptr;
   printf("Size of pointer: %zu bytes\n", sizeof(ptr));
   ```

4. **数组：** 对于数组，`sizeof()` 返回整个数组所占用的字节数。

   ```c
   cint arr[5];
   printf("Size of array: %zu bytes\n", sizeof(arr));
   ```

   但是，如果将 `sizeof()` 应用于数组的名称，则它将返回整个数组的大小，而不是数组中元素的个数。因此，通常在获取数组元素个数时会使用 `sizeof(array) / sizeof(array[0])` 的方式。


`sizeof` 运算符的原理在编译时确定，而不是在运行时。编译器根据数据类型或者变量来计算它们所占用的内存空间大小，并在编译过程中将这个信息直接编码进生成的机器代码中。