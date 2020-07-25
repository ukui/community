# CI/CD 规范

在软件开发中经常会提到 持续集成(Continuous Integration)（CI）和 持续交付(Continuous Delivery)（CD）这两件事务。由于 UKUI 事实上是出于软件分发的上游，为了解决以下需求我们引入了 CI/CD 来为开发者提供便捷。

* 频繁发布
* 自动化流程
* 可重复
* 快速迭代

## 持续集成

在 UKUI 的早期阶段仅针对 Ubuntu Kylin 平台确保软件的正常构建与打包，在社区影响力逐渐扩大时，有了在多种平台及发行版上进行构建测试的需求。我们有一个逐渐会增加构建测试支持的发行版列表。

* Archlinux
* Fedora Latest
* Fedora Rawhide
* Debian Unstable
* Ubuntu Latest (= Ubuntu Kylin Latest)

目前拟做构建测试的架构有

* amd64

对于开源软件，事实上有大量公共 CI/CD 服务可供选择，诸如 GitHub Actions、Travis CI、CircleCI、Azure Pipelines 等。在对这些服务提供商均进行测试后，均衡持续集成、软件开发工作流等多方面的考虑，UKUI 的大部分项目均使用 GitHub Actions 来完成构建测试。

### 发行版基础环境

为了便于社区成员对于 CI 的修改，这里整理了一份目前使用的不同发行版 docker image 的列表。

| 发行版          | image 源                              | image 地址                                      | tag       | container                            |
| --------------- | ------------------------------------- | ----------------------------------------------- | --------- | ------------------------------------ |
| Archlinux       | [docker hub](https://hub.docker.com/) | [archlinux](https://hub.docker.com/_/archlinux) | `latest`  | `docker.io/library/archlinux:latest` |
| Fedora Latest   | [docker hub](https://hub.docker.com/) | [fedora](https://hub.docker.com/_/fedora)       | `latest`  | `docker.io/library/fedora:latest`    |
| Fedora Rawhide  | [docker hub](https://hub.docker.com/) | [fedora](https://hub.docker.com/_/fedora)       | `rawhide` | `docker.io/library/fedora:rawhide`   |
| Debian Unstable | [docker hub](https://hub.docker.com/) | [debian](https://hub.docker.com/_/debian)       | `sid`     | `docker.io/library/debian:sid`       |
| Ubuntu Latest   | [docker hub](https://hub.docker.com/) | [ubuntu](https://hub.docker.com/_/ubuntu)       | `latest`  | `docker.io/library/ubuntu:latest`    |

### 发行版软件依赖包命名规则

在创建 CI 工作流时，最消耗时间的一部分工作就是为不同的发行版添加构建依赖。由于不同的发行版对于软件的开发者库命名规则不一样，所以需要确保添加的依赖包在目标发行版中确切存在。

为了便于社区成员查找不同发行版的软件开发者库，在这里有一份对于普遍情形适用的说明。

### Archlinux

Archlinux 软件仓库中对于软件包开发库的命名较为简短。

* 对于 Qt5 的相关依赖，一般形式为 `qt5-XXX`，诸如 `qt5-tools`、 `qt5-base` 等。
* 对于 KF5 的相关依赖，一般形式为 `kXXX`，诸如 `kwindowsystem`、`kinit` 等。
* 对于 gtk、glib、gsetting 等的相关依赖，一般直接使用原名，诸如 `glib2`、`gtk2` 等。
* 对于 X 相关的库，一般形式为 libxXXX，诸如 `libx11`、`libxi` 等。

所有的 Archlinux 软件包可以在 [Archlinux Packages](https://www.archlinux.org/packages) 进行查询。

### Fedora

Fedora 软件仓库中对于软件包开发库的命名通常以 devel 结尾。

* 对于 Qt5 的相关依赖，一般形式为 `qt5-xxx-devel`，诸如 `qt5-qtbase-devel`、`qt5-qttools-devel` 等。
* 对于 KF5 的相关依赖，一般形式为 `kf5-kxxx-devel`，诸如 `kf5-kconfigwidgets-devel`、`kf5-kcrash-devel` 等。
* 对于 gtk、glib、gsetting 等的相关依赖，通常为原名-devel的形式，诸如 `glib2-devel`、`gsetting-qt-devel` 等。

对于所有 Fedora 软件包推荐在 [pkgs.org](https://pkgs.org/) 进行搜索。

### Ubuntu/Debian

Ubuntu/Debian 软件仓库中对于开发者库的命名通常为 lib软件名-dev，诸如 `libglib2.0-dev`、`libqt5x11extras5-dev`、`libkf5windowsystem-dev` 等。

### 简易模板

这里提供一份包含了编译环境、Qt5 基础环境与 CMake 的模板。

```yaml
name: build

on:
  push:
    branches:
      - master

  pull_request:
    branches:
      - master

jobs:
  archlinux:
    name: on Archlinux Latest
    runs-on: ubuntu-20.04
    container: docker.io/library/archlinux:latest
    steps:
      - name: Checkout project source code
        uses: actions/checkout@v2
      - name: Refresh pacman repository
        run: pacman -Sy
      - name: Install build dependencies
        run: pacman -S --noconfirm base-devel qt5-base qt5-tools cmake 
      - name: Configuration & Make
        run: |
          cmake .;
          make -j$(nproc);
  
  debian-sid:
    name: on Debian Sid
    runs-on: ubuntu-20.04
    container: docker.io/library/debian:sid
    env:
      DEBIAN_FRONTEND: noninteractive
    steps:
      - name: Checkout project source code
        uses: actions/checkout@v2
      - name: Update apt repository
        run: apt-get update -y
      - name: Install build dependcies
        run: apt-get install -y build-essential qt5-default qttools5-dev-tools cmake
      - name: Configuration & Make
        run: |
          cmake .;
          make -j$(nproc);
  
  fedora-latest:
    name: on Fedora Latest
    runs-on: ubuntu-20.04
    container: docker.io/library/fedora:latest    
    steps:
      - name: Checkout project source code
        uses: actions/checkout@v2
      - name: Install build dependencies
        run: dnf install --refresh -y make gcc gcc-c++ cmake cmake-rpm-macros autoconf automake qt5-rpm-macros qt5-qtbase-devel qt5-qttools-devel
      - name: Configuration & Make
        run: |
          cmake .;
          make -j$(nproc);
  
  fedora-latest:
    name: on Fedora Latest
    runs-on: ubuntu-20.04
    container: docker.io/library/fedora:rawhide
    steps:
      - name: Checkout project source code
        uses: actions/checkout@v2
      - name: Install build dependencies
        run: dnf install --refresh -y make gcc gcc-c++ cmake cmake-rpm-macros autoconf automake qt5-rpm-macros qt5-qtbase-devel qt5-qttools-devel
      - name: Configuration & Make
        run: |
          cmake .;
          make -j$(nproc);

  ubuntu-latest:
    name: on Ubuntu 20.04
    runs-on: ubuntu-20.04
    container: docker.io/library/ubuntu:latest
    env:
      DEBIAN_FRONTEND: noninteractive
    steps:
      - name: Checkout project source code
        uses: actions/checkout@v2
      - name: Update apt repository
        run: apt-get update -y
      - name: Install build dependcies
        run: apt-get install -y build-essential qt5-default qttools5-dev-tools cmake
      - name: Configuration & Make
        run: |
          cmake .;
          make -j$(nproc);
      
```

## 持续部署

TODO 这一部分的工作拟定为将 release 版本自动提交到各个发行版的软件仓库，情况较为复杂。
