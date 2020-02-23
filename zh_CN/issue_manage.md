# Issue 管理

## 角色定义

为了更好的管理 issue，我们将参与者划分为以下角色

| 角色         | 职能                          |
| ------------ | ----------------------------- |
| 测试人员     | 对 UKUI 各项目进行测试        |
| 开发人员     | UKUI 各项目的实际开发与维护者 |
| 社区参与人员 | 社区爱好者、参与者            |

## Issue 分类标签

在中大型项目中，不同的参与者对项目参与程度不同。为了便于每位参与者发现并解决 issue，我们需要对 issue 进行分类。UKUI 项目默认有 9 个约定俗成的分类标签。

| 标签名           | 描述                                                            |
| ---------------- | --------------------------------------------------------------- |
| kind/bug              | 该 issue 是参与者提出的未知 bug                                 |
| kind/documentation    | 该 issue 文档相关的问题                                         |
| kind/duplicate        | 该 issue 是参与者提出的已知 bug                                 |
| kind/enhancement      | 该 issue 请求添加新功能特性或者优化已有功能                     |
| kind/good-first-issue | 该 issue 对新接触项目的开发人员或者测试人员有帮助               |
| kind/help-wanted      | 该 issue 需要寻求其他人员的帮助，或者项目新参与人员提的帮助问题 |
| kind/invalid          | 该 issue 是错误的或不可用的                                     |
| kind/question         | 该 issue 是存疑的或需要提供更多信息                             |
| kind/wontfix          | 该 issue 在可见的项目排期内是不计划实现的功能需求或者修复       |

## Issue 状态标签

事实上，不同的 issue 对项目的影响程度或者说需要解决的急迫程度大有不同。为了对 issue 的优先级以及当前状态进行追踪， UKUI 约定了许多有用的标签。

### 优先级标签

| 标签名 | 描述                                                                         |
| ------ | ---------------------------------------------------------------------------- |
| priority/high   | 高优先级的，重要紧急需要尽快处理的 issue                                     |
| priority/medium | 中等优先级的，重要不紧急的 issue                                             |
| priority/low    | 低优先级的，不重要不紧急，一般用与需要小问题，或者需要确认排期计划的 issue   |

优先级标签的颜色值：

* `priority/high`: #ff0000
* `priority/medium`: #ff3366
* `priority/low`: #aa8899

### Issue 状态标签

| 标签名    | 描述                                                      |
| --------- | --------------------------------------------------------- |
| status/confirmed | 项目维护者已确认 issue 描述的问题存在                     |
| status/resloved  | 项目维护者已处理 issue 描述的问题，审阅无误后 close issue |
| status/reopen    | 标记为 reslove 的 issue 再次出现问题                      |
| status/need-assign | 该 issue 需要指定参与者负责                             |

issue 状态标签的颜色：

* `status/need-assign`: #ffff00
* `status/confirmed`: #00ffff
* `status/resolved`: #00ff00
* `status/reopen`: #ff6666

## 标签扩展

如果因为项目需要，而另外添加新标签，新标签不应该与已有的标签产生歧义，且应该说明该标签的用法以及规范，并更新到本文档，以保证项目参与人员的统一认知。

## Issue 工作流

### 测试人员 issue 工作流

1. 对于内测发现的 bug ，提交之后打上 `kind/bug` 标签，同时标明优先级 ( `priority/high` / `priority/medium` / `priority/low` )
2. 对于其他人提交的 issue ，根据提交内容，打上 `kind/bug` / `kind/enhancement` / `kind/invalid` / `kind/documentation`，并对 bug 进行验证，验证通过则评估其优先级，添加或者修改优先级标签;
3. 如果能明确责任人可直接在 issue 内分配到人，否则等待开发者自己领取或者项目负责人去分配;
4. 对于标注为 `status/resolved` 标签的 issue ，进行回归测试，如果验收通过则关闭该 issue ，不通过则将 `status/resolved` 标签替换为 `status/reopen`;
5. 对于验收通过的 issue 再次出现，需要将该 issue 重新打开，并打上 `status/reopen` 标签;

### 开发人员 issue 工作流

1. 对标注为 `kind/bug` 的 issue 进行自我验证，验证通过，打上 `status/confirmed` 标签;
2. 对于 `status/confirmed` 的 bug 进行领取(分配给自己);
3. 对于分配到自己名下的 issue ，如果不能复现或者有疑问，则打上 `kind/question` 标签让 issue 提交者提供更多信息;
4. 如果分配的 bug 已经修复，则改为 `status/resolved` 标签，让测试人员回归测试。如果自己名下有 `status/reopen` 的 issue , 请及时处理。
5. 对于暂时没时间处理需要别人帮助的 issue ，打上 `kind/help-wanted` 标签。
6. 对于可见的项目排期内不会去实现的功能需求或者修复的问题，打上 `kind/wontfix`。

### 社区参与人员 issue 工作流

1. 所有人发现问题或需求可提交 issue;
2. 可协助确认bug(在issue下回复);
3. 可对未分配到人的 issue 进行认领(不在 ukui 项目内的人员，在 issue 下进行回复即可)，优先认领 `kind/help-wanted` 的 issue.