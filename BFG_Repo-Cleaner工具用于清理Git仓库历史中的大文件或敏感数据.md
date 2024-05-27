BFG Repo-Cleaner是一个用于清理Git仓库历史中的大文件或敏感数据的工具，它比`git filter-branch`更快、更简单。开发者通常在意识到敏感数据（如密码、密钥等）被错误地提交到Git仓库后，或需要从仓库历史中移除大文件以减小仓库大小时使用BFG。

### 主要特性

- **快速处理**：BFG被设计为比标凈的`git filter-branch`快10到720倍，能高效地处理大型仓库。
- **简单易用**：BFG提供了更用户友好的命令行选项，使得从仓库中移除大文件或密码变得简单。
- **专注于文件大小和数据**：BFG特别适用于移除大文件或密码/凭证等敏感数据，而不那么侧重于编辑已有提交中的其他类型的数据（如作者修改、提交信息编辑等）。

### 安装

BFG Repo-Cleaner是一个Java程序，因此要求你的系统已经安装了Java Runtime Environment (JRE)。你可以从[BFG的官方网站](https://rtyley.github.io/bfg-repo-cleaner/)或其[GitHub页面](https://github.com/rtyley/bfg-repo-cleaner)下载最新版本的jar文件。

### 使用示例

1. **备份你的仓库**：在使用BFG对仓库进行操作前，强烈建议先创建一个备份。

2. **克隆一个裸仓库**：BFG操作的是裸仓库（bare repository）。你可以使用如下命令克隆：

    ```bash
    git clone --mirror git://example.com/your-repo.git
    ```

3. **移除大文件**：若要移除所有大于50MB的文件，可以运行：

    ```bash
    java -jar bfg.jar --strip-blobs-bigger-than 50M your-repo.git
    ```

4. **移除敏感数据**：若要从所有文件中移除密码`password123`，可以运行：

    ```bash
    java -jar bfg.jar --delete-files password.txt your-repo.git
    ```

5. **使用Git gc清理和压缩仓库**：

    在使用BFG后，你需要运行`git gc`命令来清理残余数据并压缩仓库：

    ```bash
    cd your-repo.git
    git reflog expire --expire=now --all && git gc --prune=now --aggressive
    ```

6. **Force-push到远程仓库**：

    处理完成后，你需要使用force-push（强制推送）将改动提交到远程仓库：

    ```bash
    git push --force
    ```

务必注意，在处理完成后强制推送到远程仓库会改写历史。如果仓库被其他人使用，这可能会对他们造成影响。

### 注意事项

- 使用BFG Repo-Cleaner会重写Git仓库的历史。确保所有协作者都知道并同意这一操作。
- BFG优先于任何保护分支上的改动。确保在操作前仔细检查你要保留的数据。
- 一旦用BFG清理了仓库，请通知所有协作者放弃他们的本地分支和克隆，重新克隆仓库。

BFG Repo-Cleaner提供了一种相对于`git filter-branch`更快捷、更高效的方式来处理仓库历史中的大文件和敏感数据问题。
