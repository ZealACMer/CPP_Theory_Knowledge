`strtok` 和 `strtok_r` 是 C 语言中用于分割字符串的函数。它们根据指定的分隔符将字符串分割成多个子字符串（tokens）。这两个函数的主要区别在于线程安全性和可重入性。

### `strtok` 的使用示例：

```c
#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "a,b;c:d";
    const char *delim = ",;:";
    char *token;

    // 使用 strtok 前需要手动设置 str 的末尾为 '\0'
    strcat(str, "\0");

    token = strtok(str, delim);
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok(NULL, delim); // 继续查找下一个 token
    }

    return 0;
}
```

这段代码将输出：
```
a
b
c
d
```

### `strtok_r` 的使用示例：

```c
#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "a,b;c:d";
    const char *delim = ",;:";
    char *token;
    char *strtok_r_lasts = NULL; // 用于保存 strtok_r 的状态

    token = strtok_r(str, delim, &strtok_r_lasts);
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok_r(NULL, delim, &strtok_r_lasts); // 继续查找下一个 token
    }

    return 0;
}
```

这段代码同样将输出：
```
a
b
c
d
```

### `strtok` 和 `strtok_r` 的区别：

1. **线程安全性**：
   - `strtok`：不是线程安全的。它使用静态变量来保存状态，因此在多线程环境中使用时可能会导致数据竞争和不可预测的行为。
   - `strtok_r`：是线程安全的。它通过一个额外的参数来保存状态，因此可以在多线程环境中安全使用。

2. **可重入性**：
   - `strtok`：不是可重入的。因为它依赖于静态变量，所以在中断的系统调用或信号处理程序中使用 `strtok` 可能会导致问题。
   - `strtok_r`：是可重入的。它不依赖于静态变量，而是使用用户提供的指针来保存状态，因此可以在中断的系统调用或信号处理程序中安全使用。

3. **性能**：
   - `strtok` 和 `strtok_r` 在性能上相似，但 `strtok_r` 可能稍微慢一些，因为它需要额外的操作来处理状态指针。

4. **使用场景**：
   - 如果你的程序是多线程的，或者你需要在信号处理程序中分割字符串，应该使用 `strtok_r`。
   - 对于单线程程序，如果确定不会有线程安全问题，可以使用 `strtok`，因为它的代码更简单。

总的来说，推荐使用 `strtok_r`，因为它提供了更好的线程安全性和可重入性，特别是在现代程序设计中，这些特性非常重要。
