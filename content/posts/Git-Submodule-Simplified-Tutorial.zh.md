+++
title = 'Git Submodule 简明教程'
date = 2024-07-26T00:54:14+08:00
# draft = true
+++

为方便说明，本文将本地的主模块称为“主模块”，主模块对应的 GitHub 仓库称为“主仓库”；本地的子模块称为“子模块”，子模块对应的 GitHub 仓库称为“子仓库”。

### 添加子模块与子仓库

1. 将主仓库克隆到本地：
    ```
    git clone <main_repo_url>
    ```
2. 在主模块中添加子模块：
    ```
    git submodule add <submodule_url> <submodule_repo_name>
    ```
    - 添加成功后，主模块目录中会出现子模块文件夹和 `.gitmodules` 文件。打开 `.gitmodules`，可以看到其中保存的子模块及子仓库信息。
3. 将主模块的本地文件推送到 GitHub。

### 更新子模块与子仓库

主仓库和子仓库的更新不会自动同步，需要手动操作。常见情况有以下两种。

#### 根据子仓库更新主仓库

1. 如果已经修改子模块并将改动推送到子仓库，在主模块中运行以下命令来更新子模块：
    ```
    git submodule update --remote
    ```
    这样即可将本地文件同步到子模块的最新版本。
2. 将更新推送到主仓库。

#### 根据主仓库更新子仓库

1. 在本地子模块目录中，使用以下命令更新子模块：
    ```
    git submodule update
    ```
    这会根据主仓库中提交的版本更新本地文件。
2. 将更新推送到子仓库。

### 删除子模块与子仓库

1. 取消子模块的初始化：
    ```
    git submodule deinit <submodule-name>
    ```
2. 删除子模块目录：
    ```
    git rm -r <submodule-name>
    ```
3. 在主模块目录中，删除 `.git` 目录下与子模块有关的文件：
    ```
    rm -rf .git/modules/<submodule-name>
    ```
    **注意**：必要时可以使用 `-f` 选项强制删除。
