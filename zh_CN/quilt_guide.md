# 补丁管理工具 --- quilt

UKUI 的主线测试环境为 Ubuntu/Debian，构建后的软件包通常为 deb 格式。构建 deb 包之后，您可以使用 quilt 管理您所创建的软件包补丁。

在使用 quilt 时，补丁被以逻辑栈的方式组织在一起。你可以 apply (=push)、un-apply (=pop) 或简单地刷新它们然后再放入栈内。	

## 前置条件

quilt 只在存在 `.pc` 和 `patched` 的目录正常工作，这通常可以通过 `quilt import some_package.diff.gz` 自动创建。

### quilt 配置

#### quiltrc 配置文件

你需要指定一些 quilt 的设置，位于 `"$HOME"/.quiltrc` 。

```
QUILT_PATCHES=debian/patches
QUILT_NO_DIFF_INDEX=1
QUILT_NO_DIFF_TIMESTAMPS=1
QUILT_REFRESH_ARGS="-p ab"
QUILT_DIFF_ARGS="--color=auto" # If you want some color when using `quilt diff`.
QUILT_PATCH_OPTS="--reject-format=unified"
QUILT_COLORS="diff_hdr=1;32:diff_add=1;34:diff_rem=1;31:diff_hunk=1;33:diff_ctx=35:diff_cctx=33"
```

#### 环境变量

需要在环境变量指定 quilt 的补丁目录及参数

```bash
export QUILT_PATCHES=debian/patches
export QUILT_REFRESH_ARGS="-p ab --no-timestamps --no-index" 
```

#### 自动探测（可选）

通过检测目录中是否存在 debian 目录，自动完成 quilt 工作环境的设置。

```bash
d=. ; while [ ! -d $d/debian -a `readlink -e $d` != / ]; do d=$d/..; done
if [ -d $d/debian ] && [ -z $QUILT_PATCHES ]; then
        # if in Debian packaging tree with unset $QUILT_PATCHES
        QUILT_PATCHES="debian/patches"

        if ! [ -d $d/debian/patches ]; then mkdir $d/debian/patches; fi
fi
```

## quilt 工作流

### 创建新补丁

`apt-get source` 会根据软件包的格式决定是否应用补丁。对于某些软件包，您将必须按照此处指定的方式应用补丁。对于其他人，它们将被自动应用。

创建新的补丁只需要通过几个简单的步骤。

首先，将所有已存在的补丁应用到源。

```bash
quilt push -a
```

之后，创建新的补丁：

```bash
quilt new new-patch-name.diff
```

在修改文件之前必须先 `add` 让 quilt 追踪该文件。

```bash
quilt add README
```

然后修改添加追踪后的文件，并更新 quilt 的记录

```bash
quilt refresh
```

提交一个详细补丁的描述也是必要的：

```bash
quilt header -e
```

最后完成编辑，恢复源到最初的状态。

```bash
quilt pop -a
```

### 修改已存在的 patch

quilt 支持管理多个 patch，所以您可以修改所有 patch 中已存在的一个。编辑已存在 patch 是非常容易的，`push` 之后编辑文件进行更新即可。

```bash
quilt push existed-patch-name.diff
quilt refresh existed-patch-name.diff
```

### 更新应用失败的 patch

通常应用 patch 到新的上游分支时会导致 patch 的应用失败，这时需要执行  `push -f` 操作，然后检查 _* .rej_ 文件并使相关文件包含应具有的修改。

### 删除已合并的 patch

当新的上游分支已合并或修复了您的已存 patch，您需要移除您的对应 patch。只需要简单的执行：

```bash
quilt refresh
# 如果需要移除 patch 的文件
quilt remove /path/to/target/file
quilt refresh
```

### 更新 patch 堆栈顶部 patch 的描述

在 `quilt header -e` 修改描述之后，正如前文所述，移除所有 patch 并恢复源到最初的状态：`quilt pop -a` 。

## 参考文档

1. [quilt(1)](https://manpages.debian.org/stretch/quilt/quilt.1.en.html)
2. [Debian Wiki / UsingQuilt](https://wiki.debian.org/UsingQuilt)
