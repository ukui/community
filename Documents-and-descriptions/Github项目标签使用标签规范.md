# Github项目标签使用规范
  为了在以后的工作中大家对项目的状态有简洁明了的统一认知，以便于更顺畅的沟通，更好的协作，现将Github项目Issue中的标签、各标签的含义、使用规范整理出来，同时也方便了大家对项目状态的查看以及Issue状态管理。  

1. 项目默认标签  

 ![img]{1.png}
 默认有如下8个标签:bug、duplicate、enhancement、good first issue、help wanted、invalid、question、wontfix.  
  
  1)bug:一般人员发现的bug，如果项目issue中没有记录，可以提到项目的issue中，并打上本标签。  

  2)duplicate:提issue前需要先确认没有与要提的问题一样的，如果因为其他原因发现已有重复的问题或者请求，打上本标签。  
 
  3)enhancement:提出新功能特性或者已有功能优化的需求，打上本标签，同样需要跟项目开发人员以及管理人员沟通确认。  

  4)good first issue:对于新接触项目的开发或者测试人员有帮助的issue打上本标签。  

  5)help wanted:该问题需要寻求其他人员的帮助，或者项目新参与人员提的帮助问题，打上本标签。  

  6)invalid:不对的或者不可用的issue，打上本标签。  

  7)question:存疑的、需要提供更多信息的issue，比如需要bug的复现步骤、复现环境、条件等补充信息，打上本标签。  

  8)wontfix:在可见的项目排期内不会去实现的功能需求或者修复的问题，打上本标签，需要跟提出该issue的人员进行确认。  

  **备注**:有的项目默认还会多一个documentation标签，标示是文档类型的issue  
 
2. 项目新增标签  

  因为项目issue状态管理的需要，增加以下标签：  
  ![img]{2.png}  

  1)区分优先级的3个标签:low, medium, high  

  high:高优先级的，重要且需要紧急或者尽快处理的issue  
  medium:中等优先级的，重要但不紧急的issue  
  low:低优先级，既不重要也不紧急，一般用与需要改善的小问题，或者需要确认排期计划的issue  
  
  各标签颜色值：  
    high(#ff0000), medium(#ff3366), low(#aa8899)  

  2)用于区分bug或者项目任务状态的3个标签:  

  confirmed, resolved, reopen  
  confirmed:issue责任人确认问题存在，打上本标签。  
  resolved:责任人已经处理完，需要issue提出者或者其他人验收,验收没有问题则close，有问题的则改为reopen。  
  reopen:标记为resoled的bug未解决，或者已关闭的bug再次出现，打上本标签。  

  各标签颜色值：  
   need assign(#ffff00), confirmed(#00ffff),resolved(#00ff00), reopen(#ff6666)  

**注意** :新增的标签是需要项目创建人员或者管理人员添加的，建议统一使用上述给出的颜色值标示。  
  
3. 标签使用人员以及流程说明  

**测试人员:**  

  (1) 对于内测发现的bug，提交之后打上bug标签，同时标明优先级(high/medium/low);  

  (2) 对于其他人提交的issue，根据提交内容，打上bug/enhancement/invalid/documentation，并对bug进行验证，验证通过则评估其优先级，添加或者修改优先级标签;  

  (3)如果能明确责任人可直接在issue内分配到人，否则等待开发者自己领取或者项目负责人去分配;  

  (4) 对于标注为resolved标签的issue，进行回归测试，如果验收通过则关闭该issue，不通过则将resolved标签替换为reopen。  

  (5) 对于验收通过的issue再次出现，需要将该issue重新打开，并打上reopen标签。  
  
**开发人员:**  

  (1) 对标注为bug的issue进行自我验证，验证通过，打上confirmed标签;  

  (2) 对于confirmed的bug进行领取(分配给自己)  

  (3) 对于分配到自己名下的issue，如果不能复现或者有疑问，则打上question标签让issue提交者提供更多信息  

  (4) 如果分配的bug已经修复，则改为resolved标签，让测试人员回归测试。如果自己名下有reopen的issue,请及时处理。  

  (5) 对于暂时没时间处理需要别人帮助的issue，打上help wanted标签。  

  (6）对于可见的项目排期内不会去实现的功能需求或者修复的问题，打上wontfix  

  
**社区参与人员:**  

  (1) 所有人发现问题或需求可提交issue。  

  (2) 可协助确认bug(在issue下回复)  

  (3) 可对未分配到人的issue进行认领(不在ukui项目内的人员，在issue下进行回复即可)，优先认领help wanted的issue.  


4. 标签扩展注意事项  

    如果因为项目需要，而另外添加新标签，新标签不应该与已有的标签产生歧义，且应该说明该标签的用法以及规范，并更新到本文档，以保证项目参与人员的统一认知。  
