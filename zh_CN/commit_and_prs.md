# 代码提交与合并

绝大多数的 UKUI 项目使用 git 进行代码管理，使用 Github 进行代码的分发与合作开发。为了便于所有人都可以有序的参与 UKUI 的所有项目，我们希望能够约束 UKUI 项目的代码提交记录、冲突合并和 PRs 创建等各种行为。

## UKUI git 工作流

UKUI 项目使用经典的 Github 工作流，这里仅作简单的描述。更为准确的流程定义可以阅览 [Understanding the Github flow](https://guides.github.com/introduction/flow/index.html) 。

![Github Flow](/static/github-flow.png)

> Local Commit -> Forked Repository -> Pull Request -> CI/Review -> Mature

## Commit 规范

Commit 是协作开发的起点和最小单位，尽管大多数时候我们允许更加个性化的提交与提交信息，但是仍有一个推荐的 Commit 规则。

我们希望 Commit 是更加细化的切片，在总体历史上呈现均匀的状态，一次提交中包含大量新增代码和功能面临回滚或是检出特定历史版本通常是令人头痛的事情，尽可能确保单次提交实现了单个模块/方法的实现与修改。

另一方面，优秀的提交信息是好项目的共同特点，我们在 UKUI 项目中在不同的场景使用两种不同风格的提交信息，区别在于看起来一种简短一些，一种更为详细丰富。为了方便所有参与者理解项目，原则上以英文为各类信息说明的选择。

首先描述简短的提交，在添加或修改了简单实现时，您通常没有必要作出太多说明，只需要用一句简单的话说明变更内容，如:

* `Add CPU arch filter scheduler support`
* `Remove deprecated modules: A, B`
* `Fix issue #233`

当您实现了一些复杂模块或者功能，但是并未添加文档说明时，您可以通过一个较长的提交信息更好的向审核者或是任何感兴趣的人进行说明。更长的提交风格应该遵循如下规则:

1. 遵循主题-内容-引用（可选）的大纲结构。
2. 主题行保持单行，具有合理的大写，并保持在 50 字以内。
3. 主题行、内容、段落之间保留空行。
4. 内容单行保持在 70 字以内。
5. 确保引用的 Issue 或文档是合法可访问的。

例如:

```text
Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 70
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```


提交PR的原则：
* 一个PR解决一个问题/添加一个特性;
* 清晰描述PR的目的;
* Fork后与上游保持同步;

## 分支规范

首先，您对您 fork 的分支具有自由命名的权利，但我们仍然推荐您使用 issue id 、新增功能或拟解决问题的名词来进行分支的命名如 `issue#233` 、`feature-thumbnail` 等。

对于上游仓库，一个重要的原则是永远确保主分支的稳定性，它不应该存在任何的构建错误，便于项目的持续分发。

## 冲突与合并

在多人合作的项目中，文件修改造成的冲突不可避免，同时，由于git采用commit log的方式管理项目，即使没有文件冲突，也可能会有commit log的冲突，我们需要合理的处理这些冲突才能完成合并。

对于项目的合并，一个显而易见的争议是使用 merge 还是 rebase ，尽管有一些反对意见认为 rebase 区别于 merge 会修改项目的真正历史，为了避免过多无用及零碎的合并历史，我们还是推荐使用 rebase 。所以为了不产生严重的历史编辑行为，我们对 rebase 制定了一些约定：

1. 禁止产生修改上游或是您已经 push 过的历史
2. 当上游项目更新时，应及时基于最新的master进行rebase并解决代码冲突
3. 当与上游代码冲突时，不能以破坏上游代码的结构和功能为代价解决冲突
4. 待补充

## Pull Request

您所创建的单个 PR 建议只包含单个功能/特性的实现或修复，因为针对不同的功能/特性 review 者可能是不同的人。

如果您希望您的代码尽快同步到上游，请确保您的 PR 符合上述"冲突与合并"章节中的要求。

同时您的 PR 说明应该符合 PR 信息的规范，只需填写 PR 模板中的信息即可，这个模板目前还在制定当中。

一些 UKUI 项目使用了 Github Action 、Travis CI 等构建服务，您可以通过这些服务快速测试自己的代码是否通过构建检查，在创建 PR 时，请检查您的代码是否通过 PR 行为的构建检查。

## 一个例子

以 ukui-session-manager 为例提供一个开发整体流程的实例。

### Fork 上游仓库

简单的点击 Github 的 frok 按钮，然后克隆到本地，并添加上游仓库地址。

```sh
git clone https://github.com/YOUR-GITHUB-USERNAME/ukui-session-manager.git
git remote add upstream https://github.com/ukui/ukui-session-manager.git
```

或者下载上游代码的 zip 压缩文件解压到本地后添加您自己的仓库地址，这时您可能需要确保自己与上游完成同步。

```sh
git remote add upstream https://github.com/ukui/ukui-session-manager.git
git pull upstream master
```

### 创建工作分支

创建独立的工作分支实现并提交您的新功能。

```sh
git checkout -b feature-test
# After modified code
git add .
git commit -m "Add new feature: test"
```

## 创建 PR

创建 PR 前确保您的工作分支与上游主分支或是开发分支没有冲突，如果有冲突，rebase 上游的主分支或是开发分支。

```sh
git rebase -i upstream/master
# 处理冲突(如果存在)
git add .
git commit
```

推送到您 fork 的仓库。

```sh
# 添加您的远程仓库(如果没有)
git remote add origin https://github.com/YOUR-GITHUB-USERNAME/ukui-session-manager.git
git push -u origin feature-test
```

在您的 Github 仓库界面点击 `create pull request` 按钮，选择需要合并的分支，填写 PR 信息完成创建。
