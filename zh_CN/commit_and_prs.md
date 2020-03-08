这将包括代码提交信息和 PRs 的规范：（正在完成..）

提交PR的原则：
* 一个PR解决一个问题/添加一个特性;
* 清晰描述PR的目的;
* Fork后与上游保持同步;


与上游分支保持同步的两种方法：
1. 简单粗暴：
将自己fork的项目删掉，重新fork并提交pr。

2. 提交pr前解决冲突,以ukui-session-manager为例：
```
# 本地为https://github.com/ll-eleven/ukui-session-manager.git
# 上游为https://github.com/ukui/ukui-session-manager.git
git remote add upstream https://github.com/ukui/ukui-session-manager.git
git fetch upstream
git merge upstream/master
# 如有冲突解决冲突
git push
```
