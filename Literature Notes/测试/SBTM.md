---
tags:
  - 测试
  - 探索式测试
description: 探索式测试的一种管理方式
---
SBTM=Session-based test management会话式测试管理（简称SBTM）。它是一种[灵活的](https://so.csdn.net/so/search?q=%E7%81%B5%E6%B4%BB%E7%9A%84&spm=1001.2101.3001.7020)测试方法，强调测试人员的自主性和测试的探索性质。
```plantuml
@startuml
测试 --> Mission: 拆分成多个Mission
Mission --> Session: 拆分多个Session
state Session{

Charter -right-> TimeBox
Charter:一个Session所要完成的使命和目标
TimeBox --> ReviewableResults
TimeBox:一段固定的不被打扰的时间段，一般为45-90分钟；
ReviewableResults -left-> Debriefing
ReviewableResults:汇总的关于Session的结果测试报告，常见的是Session Sheet
Debriefing:测试人员与产品负责人和开发团队的汇报交流，就本次测试的发现进行检查，并且看看优化改进的地方
}
note top of Session
在Session中笔记中很重要，它强调在测试中记录你的过程和想法
- 想法（Idea）——用于在执行之前记录您的测试想法，并记录您所进行的测试的类型 
- 问题（Question）——有些东西不是很清楚，你可能会留在Debriefing期间询问，它可能是一个缺陷，也可能是一个新功能。
- 惊讶（Surprise）——在测试过程中让您感到惊讶的事情，需要在稍后的测试过程中进行进一步的调查，因为一个缺陷可能隐藏在惊讶背后。
- 问题（issue）和关注（Concern）——一些不需要提出缺陷但需要在Debriefing期间讨论或在Session期间进一步调查的事情
- 积极（Positives）——测试不一定是消极的，为什么不记录和分享团队的优点。
end note
@enduml
```

一个简报的范例
![[Pasted image 20240307172551.png]]

[参考](https://blog.csdn.net/tester_sc/article/details/106654681?spm=1001.2101.3001.6650.7&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-7-106654681-blog-133688305.235%5Ev43%5Econtrol&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-7-106654681-blog-133688305.235%5Ev43%5Econtrol&utm_relevant_index=14)