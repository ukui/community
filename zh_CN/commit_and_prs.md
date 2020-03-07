这将包括代码提交信息和 PRs 的规范。
正在完成..

*同一项目下，在上一次pr已经被merge后，再次提交pr，且不会重新提交已经被merge的部分的方法：
1.将自己fork过去的项目删掉，重新fork并在新fork的项目下修改再提交pr。
2.以ukui-session-manager为例：
  我本地为https://github.com/ll-eleven/ukui-session-manager.git
  从https://github.com/ukui/ukui-session-manager.git fork过来的
  在终端的项目下：
  git remote add upstream https://github.com/ukui/ukui-session-manager.git
  git fetch upstream
  git merge upstream/master
  git push 
  即可
