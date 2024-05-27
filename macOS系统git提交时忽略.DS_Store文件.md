`.DS_Store`是Mac OS操作系统自动生成的隐藏文件，用于存储目录的自定义属性，如图标位置和背景颜色等。这些文件对于项目代码没有实际影响，所以通常不希望将它们包括在git仓库中。为了在git提交时忽略`.DS_Store`文件，你可以通过以下几种方式实现：

### 1. 使用`.gitignore`文件

在你的git仓库根目录下创建一个名为`.gitignore`的文件（如果还不存在的话），然后添加下面这行：

```
.DS_Store
```

这将会告诉git忽略掉所有`.DS_Store`文件。

如果你想要忽略所有目录下的`.DS_Store`文件，可以在`.gitignore`文件中使用通配符：

```
**/.DS_Store
```

### 2. 全局`.gitignore`

如果你想要在你的所有git项目中忽略`.DS_Store`文件，你可以设置一个全局的`.gitignore`配置。首先，创建一个全局`.gitignore`文件，比如在你的用户目录下：

```bash
touch ~/.global_gitignore
```

然后，编辑这个文件，添加：

```
.DS_Store
```

最后，告诉git使用这个文件作为全局`.gitignore`配置：

```bash
git config --global core.excludesfile ~/.global_gitignore
```

### 3. 从仓库中移除已跟踪的`.DS_Store`文件

如果`.DS_Store`文件已经被添加到仓库中了，你需要手动移除它们。首先，运行以下命令从git索引中移除`.DS_Store`文件，但保留在本地文件系统中：

```bash
git rm --cached .DS_Store
```

如果`.DS_Store`文件分散在不同的目录下，你可以用下面的命令来移除所有的`.DS_Store`文件：

```bash
find . -name .DS_Store -exec git rm --cached {} +
```

然后，提交更改到你的仓库：

```bash
git commit -m "Remove .DS_Store from repository"
```

通过以上步骤，你可以在git提交时忽略掉`.DS_Store`文件，从而保持仓库的干净整洁。
