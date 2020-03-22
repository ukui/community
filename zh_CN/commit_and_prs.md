这将包括代码提交信息和 PRs 的规范：（正在完成..）

代码提交信息组成：
```
<type>: <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```
1. type：指定当前commit的类型
   * feat: 新功能
   * fix: 修复问题
   * docs: 修改文档
   * style: 修改代码格式，不影响代码逻辑
   * refactor: 重构代码，理论上不影响现有功能
   * perf: 提升性能
   * test: 增加修改测试用例
   * chore: 修改工具相关（包括但不限于文档、代码生成等）
   * deps: 升级依赖
2. subject：简述本次提交做了什么。
3. body：补充说明本地提交的内容，没有则省略
4. footer：
   * 注明本次修改的约束条件，没有则省略。
   * issue ref：对应解决的issue的连接
   * doc ref：涉及到的文档连接

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
