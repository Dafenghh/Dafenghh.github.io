---
title: 'Course Note: Software System Analysis and Design'
comments: true
categories: SystemDesign
tags:
  - notes
abbrlink: 956bf9e0
date: 2019-07-03 12:22:05
---

# intro


## 软件工程的定义

软件工程是：
(1) 将系统化的、规范的、可度量的方法应用于软件的开发、运行和维护，即将工程化方法应用于软件；
(2) 在(1)中所述方法的研究.

## Analysis
> Analysis emphasizes an **investigation** of the problem and requirements, rather than a solution.

## Design

> Design emphasizes a **conceptual solution** (in software and hardware) that fulfils the requirements, rather than its implementation.

## Object-oriented analysis
> Object-oriented analysis emphasizes on finding and describing the objects or concepts in the problem domain. 

## Object-oriented design
> Object-oriented design emphasizes on defining software objects and how they collaborate to fulfill the requirements. 

## Unified Modeling Language

> The UML is standard diagramming language to **visualize** the results of analysis and design.

UML is not
- a process or methodology
- object-oriented analysis and design
- guidelines for design

# 过程模型

## 瀑布模型
![瀑布模型](/img/system_design_notes/waterfall_model.png)

## 统一过程

> The Unified Process (UP) represents a mainstream approach for software development across the spectrum of project scales.

- The process is **scalable**: you need not use the entire framework of the process for every project, only those that are effective.
- The process is **effective**: it has been successfully employed on a large population of projects.
- Improves **productivity** through use of practical methods that you’ve probably used already (but didn’t know it).
- Iterative and incremental approach *allows start of work with incomplete, imperfect knowledge*.


## Unified Process Workflows

> Workflows define a set of activities that are performed
> Workflows cut across the phases, but with different levels of emphasis in each phase

The core workflows
- Business Modeling
- Requirements analysis
- Design
- Implementation
- Test and Integration

## Use Case Driven

- Use Case
  - A prose representation of a **sequence of actions**
  - Actions are performed by one or more *actors* (human or nonhuman) and the system itself
  - These actions lead to valuable results for one or more of the actors, helping the actors to achieve their goals.
- Use cases are expressed from the perspective of the users, in natural language, and should be understandable by all stakeholders
- Use-case-driven means the development team employs the use cases from requirements gathering through code and test

## Architecture Centric

> Software architecture captures decisions about:
- The overall structure of the software system
- The structural elements of the system and their interfaces
- The collaborations among these structural elements and their expected behavior
  

Architecture-centric: software architecture provides the central point around which all other development evolves 
- provides a 'big picture' of the system
- provides an organizational framework for development


## Iterative and Evolutionary

迭代和进化式开发
> An iterative and evolutionary approach allows start of development with incomplete, imperfect knowledge

Advantages:
- Logical **progress** toward a robust architecture（逐步趋向稳定）
- Effective management of **changing requirements**（有效管理需求变化）
- Continuous integration（持续集成）
- Early understanding of the system （尽早接触整个系统）
- Ongoing risk assessment（在线风险评估）

## UP Phases

1. Inception 初始：大体上的构想、业务案例、范围和模糊评估
2. Elaboration 细化：已精化的构想、核心架构的迭代实现、高风险的解决、确定大多数需求和范围以及进行更为实际的评估
3. Construction 构造：对遗留下来的风险较低和比较简单的元素进行迭代实现，准备部署。
4. Transition 移交：进行beta测试和部署。

![RUP Process model](/img/system_design_notes/rup_process_model.png)

# Inception

Envision the product scope, vision, and business case.

初始阶段更关注对基本范围的理解以及10%的需求。

# Evolutionary Requirement

进化式需求

在统一过程中，需求按照"FURPS+"模型进行分裂。
- **Functional**: features, capability, security
- **Usability**: human factors, help, documentation
- **Reliability**: frequency of failure, recoverability, predictability
- **Performance**: response times, throughput, accuracy,availability, resource usage.
- **Supportability**: adaptability, maintainability, internationalization, configurability

The '+' in FURPS+ indicates sub-factors
- Implementation: resource limitation, languages and tools, hardware...
- Interface: constraint imposed by interfacing with external systems
- Operations: system management in its operational setting
- Packaging: a physical box
- Legal (授权): 许可证等


# Use Case

用例是文本形式的情节描述。

用例建模主要是编写文本的活动，而非制图。

## Actors

- Primary actor has user goals fulfilled through using services. (e.g. the cashier)
- Supporting actor provides a service (e.g. the automated payment authorization service is an example)
  - to clarify external interfaces and protocols 明确外部接口和协议
- Offstage actor has an interest in the behavior of the use case, but is not primary or supporting (e.g. a government tax agency)

详述用例(fully dressed use case)是结构化的，展示了更多细节，并且更为深入。

## 用例模板

- 用例名称
- 范围
- 级别
- 主要参与者
- 涉众及其关注点
- 前置条件
- 主成功场景
- 扩展
- 特殊需求
- 技术和数据变元表
- 发生频率
  
## Guideline to Write Use Case
- Write in an essential UI-free style
- Write terse use case
- Write black-box use case
- take an actor and actor-goal perspective

How to find use case?

> Use cases are defined to satisfy the goals of the primary actors. The basic procedure is:

1. Choose the system boundary. Is it just a software application, the hardware and application as a unit, that plus a person using it, or an entire organization?
2. Identify the primary actors that have goals fulfilled through using services of the system.
3. Identify the goals for each primary actor.
4. Define use cases that satisfy user goals; name them according to their goal. 
   
## Find Actors and Goals: Event Analysis 

- Event Based
  - to identify external events that a system must respond to.
  - What are they, where from, and why? Often, a group of events belong to the same use case.
  - Relate the events to actors and use cases.


## 识别用例准则

- Boss Test
- EBP Test (Elementary Business Process)
  > A task performed by one person in one place at one time, in response to a business event, which adds measurable business value and leaves the data in a consistent state.
- Size Test

## UML 活动图
应用场合
> 描述某一用例中执行的步骤，使复杂的多场景用例以及与Include或extend用例的关系可视化。
> 描述用户和系统之间的业务流程协作。
> 描述软件中的方法、函数或操作。（描述算法）

![UML activicy](/img/system_design_notes/uml_activity.png)


# Domain Model

领域模型表示真实世界概念类。

Provides a conceptual perspective: 
- domain objects or conceptual classes
- associations between conceptual classes
- attributes of conceptual classes

## Guideline: Find Conceptual Classes

- Identify noun phrases.
  - Identify the **nouns and noun phrases** in textual descriptions of a domain, and consider them as candidate conceptual classes or attributes
  - Some of these noun phrases may refer to conceptual classes that are ignored in this iteration (e.g., "Accounting" and "commissions"), and some may be simply attributes of conceptual classes.
  - A weakness of this approach is the imprecision of natural language; different noun phrases may represent the same conceptual class or attribute, among other ambiguities.

### A Common Mistake with Attributes vs. Classes

> If we do not think of some conceptual class X as a number or text in the real world, X is probably a conceptual class, not an attribute.

### When to Model with 'Description' Classes

何时需要描述类：
- 需要有关商品或服务的描述，独立于任何商品或服务的现有实例。
- 避免删除所有示例后，原有的商品描述信息丢失。
- 减少冗余或重复信息。


## Association

- A is a part of B
- A contains B
- A descibes B
- A is logged/recorded/reported/captured in B
- A is a member of B
- A is a subunit of B
- A uses or manages or owns B
  
## Attribute

### 导出属性

在属性名称前加'/'

### 准则：任何属性都不表示外键

领域模型中，不能将外键作为属性，应当使用关联。

### 准则：对数量和单位建模

大部分用数字表示的数量不应该表示为纯数字，应该加上单位。

## 状态模型

> 状态图描述一个 事物或对象 受 事件或消息 刺激产生 可见的状态（属性/属性组合） 的数据变化。

- 基础符号
  - 起始状态（Initial）
  - 终止状态（Final）
  - 取消/对象取消（Termination）
  - 状态（State）
  - 变迁（Transform），含条件（Condition）、事件（Event）和事件处理动作（Action/Handler）
- 扩展符号
  - 复合状态
  - 信号

![states notation](/img/system_design_notes/states_notation.png)

绘图注意事项：

- 必须有起始状态，通常有终止和取消状态
- 状态命名要用名词短语、动词过去时或正在进行时等具有延续性的词汇
- 在需求分析过程中，尽可能不涉及动作

# 期末复习

## 活动图

活动图：一种有助于使工作流和业务过程可视化的图。

 
注意事项：

1. 一定要有起点终点，起点只有一个，终点可以有多个（活动终点、流程终点）

2. 有箭头的线，如果有循环一定有归并节点，如果有条件的话，一定要写guard（写在guard里面会自动加上左右中括号[ ]的，guard在constraint选项卡里面）。

3. 作图时不要追求画的太详细，否则来不及画完！

4. 主要注意图中存在开始、结束、循环！特别注意循环的表达，分支循环

5. 活动图中的基本动作一般都会——对应该用例内的子用例（用例图中的子用例），不过每个人画的差异会很大

6. 可能会考什么是活动状态Activity：

　 活动状态用于表达状态机中的非原子的运行，特点如下：

　　a) 活动状态可以分解成其他子活动或者动作状态

　　b) 活动状态的内部活动可以用另一个活动图来表示

　　c) 和动作状态不同，活动状态可以有入口动作和出口动作，也可以有内部转移

　　d) 动作状态(Action)是活动状态的一个特例，如果某个活动状态只包括一个动作，那么它就是一个动作状态。

![活动图](/img/system_design_notes/uml_act.png)

## 状态图

状态图主要用于描述一个对象在其生存期间的动态行为，表现为一个对象所经历的状态序列，引起状态转移的事件（Event），以及因状态转移而伴随的动作（Action）。一般可以用状态机对一个对象的生命周期建模，状态图用于显示状态机，重点在与描述状态图的控制流。


### 重要概念

#### 定义：事件、状态和转换

> 事件：指一件值得注意的事情的发生。

> 状态：指对象在事件发生之间某时刻所处的情形。

> 转换：两个状态之间的关系。它表明当某事件发生时，对象从先前的状态转换到后来的状态。

#### 状态无关和状态依赖对象

　　如果一个对象对某事件的响应总相同，则认为此对象对该事件状态无关（或非模态）。例如，如果对象接收某个消息，响应该消息的方法总做相同的事情，则该对象对于该消息状态无关。如果，对所有事件，对象的响应总是相同的，则该对象是一个状态无关对象。

　　相反，状态依赖对象对事件的响应根据对象的状态或模式而不同。

#### 有关准则：

　　a.考虑为具有复杂行为的状态依赖对象而不是状态无关对象建立状态机图

　　b.一般来讲，业务信息系统通常只有少数几个复杂的状态依赖类，对此，状态机建模通常作用不大。与此相反，在过程控制、设备控制、协议处理和通信等领域通常有许多的状态依赖对象。如果你在这些领域工作，应该熟悉和考虑使用状态机建模。

### 注意事项



A.注意状态图的对象是什么。它可能是一个system，也可能是一个用例，也可能是一个对象，一定要看清楚题目要求画什么东西的状态图。

B.画系统和用例的状态图一般是画它的过程，而画对象的状态图是画它的生命周期。

C.然后就是找状态了，一定要知道状态变量是什么，一定要能枚举状态有哪些情况。

D.接下来是操作。

1、 识别状态图的对象

2、 识别状态：考试是有组合状态的。

3、 转换边：

　　格式：触发事件 [监护条件] / 动作

 

　　触发事件：触发转换的事件，包括调用、触发信号、时间等（对象或系统里面创建什么计划，发生什么变化）

 

　　监护条件（guard）：决定是否能转换的条件，监护条件为true才能转换

 

　　动作：转换被激活时会发生的操作（外部发生的事件）


　　√ 事件一般用被动语态写出来（用被动语态写出来的叫做触发，如onKeyPressed，一般是与代码对应的函数名相同的）。
 

　　√ 如果程序显式告诉你，“如果什么，怎么样”，则一定要写guard，否则要扣分
 
　　√ 动作可以不写，写错了要扣分

　　√ 一定要在连线上写出各种条件！

　　√ 文档详细告诉有何状态，一定要完整写出


E.状态图不一定有终点，一定有起点！


5.绘制状态机图步骤

 
绘制状态机图的理想步骤是：寻找主要的状态，确定状态之间的转换 ，细化状态内的活动与转换，用复合状态来展开细节

 

　　a.寻找主要状态：对于航班机票预订系统而言，显然包括的状态主要有 -- 在刚确定飞机计划时，显然是没有任何预订的，并且在有人预订机票之前都将处于这种“无预订”状态 -- 对订座而言显然有“部分预订”和“预订完”两种状态 -- 而当航班快要起飞时，显然要“预订关闭” 总结一下，主要有四种状态：无预订、部分预订、预订完以及预订关闭

 

　　b.确定状态间转换。表格进行表示：表格横向是转出，表格纵向是转入

 

　　c.细化状态内的活动与转换

 

　　d.使用复合状态

6.转换的5要素：


　　•源状态：即受转换影响的状态

　　•目标状态：当转换完成后对象的状态

　　•触发事件：用来为转换定义一个事件，包括调用、改变、信号、时间四类事件

　　•监护条件：布尔表达式，决定是否激活转换、

　　•动作：转换激活时的操作

![状态图](/img/system_design_notes/uml_state.png)

## 领域模型

### 重要概念

1. 领域模型：是对领域内的概念类或现实世界中对象的可视化表示。领域模型也称为概念模型，领域对象模型和分析对象模型。

2. 应用UML表示法，领域模型被描述为一组没有定义操作的类图。它提供了概念透视图。它可以展示：

   1）领域类之间的关联
   2）概念类之间的关联
   3）概念类的属性
　领域模型是可视化字典，表示领域的重要抽象、领域词汇和领域的内容信息。
3. 如何找到概念类

   1）重用和修改现有的模型：这是首要、最佳且最简单的方法。
   2）使用分类列表
   3）通过识别名词短语寻找概念类
4. 准则：属性和类的常见错误

　在创建领域模型时最常见的错误是，把应该是概念类的事物表示为属性。
　如果我们认为某概念类X不是现实世界中的数字或文本，那么X可能是概念类而不是属性
5. 准则：何时需要描述类？

　在以下情况下需要增加描述类（例如，ProductDescription）：
   1）需要有关商品或服务的描述，独立于任何商品或服务的现有实例
   2）删除其所有描述事物的实例后，导致信息丢失，而这些信息是需要维护的，但是被错误地与所删除的事物关联起来
   3）减少冗余或重复信息 
6.  关联：

　关联是类之间的关系，表示有意义和值得关注的连接
　在UML中，关联被定义为“两个式多个类元之间的主义联系，涉及这些元实例之间的连接”
7. 准则：为什么应该避免加入大量关联？

　我们要避免在领域模型中加入太多的关联。回顾离散数学的相关知识，可以知道，在具有N个节点的图中，节点间有（n*(n-1)）/2个关联，这可能是个非常大的数值。连线太多会产生“视觉干扰”，使图变得混乱。所在要谨慎地增加关联线。
8. 准则：在UML中如何对关联命名

　以“类名—动词短语—类名”的格式为关联命名，其中的动词短语构成了可读的和有意义的顺序。
　例如，Sale Paid—by CashPayment 反面示例，应改为Sale Uses CashPayment
           Player Is—on Square 反面示例，应以为 Player Has Square
  关联名称应该使用首字母大写的形式。在UML中，类元应该首字母大写。以下是复合性关联名称的两种常见并且等价的合法格式：
                                 Records—current
                                 RecordsCurrent
9. 应用UML：角色

  关联的每一端称为角色。角色具有如下可选项：
  1）多重性表达式
  2）名称
  3）导航
10. 应用UML：多重性

  多重性定义了类A有多少个实例可以和类B的一个实例关联
11. 应用UML：两个类之间的多重关联

  在UML类图中，两个类之间可能会有多重关联，这并不罕见。
 
12. 属性：是对象的逻辑数据值

  准则：何时展示属性
  当需求建议或暗示需要记住信息时，引入属性。
例如，在处理销售用例中的票据通常含有工期和时间、店名和地址以及收银员ID等
因此，
      1）Sale需要dataTime属性
      2）Store需要name和address属性
      3）Cashier需要ID属性
在UML中，属性的完整语法是：
      visibility name：type multiplicity=default{property—string}

  准则：什么样的属性类型是适当的
  十分常见的数据类型包括：Boolean、Date(or DataTime)、Number、Character、String(Text)和Time等
  准则：何时定义新的数据类型类
  下述情况下，在领域模型里，把最初被认为是数字或字符串的数据类型表示为新的数据类型类：
    1）由不同的小节组成
    2）具有与之相关的操作，例如解析或校验
    3）具有其他属性
    4）单位的数量
    5）具有以上性质的一个或多个类型的抽象
  
### 注意事项
　　1.名词法：找一堆名词，然后把这堆名词之间的关系给建立起来

　　2.名词里面有属性。要判断名词是不是概念类，是不是属性。

　　3.考试的时候是针对一个用例来画领域模型，一定要看清楚是要对哪个用例建模，没有那么多时间对整个系统建模。

　　（1）先找到所有名词，判断它是类还是属性

　　　　找名词的原则（下面不要的名词标红）：

　　　　1) 跟UI相关的名词不要

　　　　2) 跟database相关的名词不要

　　　　3) 跟业务流程没有关系的名词不要，如技术相关的术语，如下面的workflow，list

　　　　4) 任何计算出来的结果，不参与业务运算，不要，如果留下了这个会扣分

　　　　5) 模糊的术语一定要过滤掉

　　（2）如果出现动词，扣分

　　（3）没有名词，扣分

　　（4）多重性（关联的一对多，一对一等）没有，扣分

　　（5）漏掉一两个类，不扣分

　　6、 属性，假如每一个类有七八个属性，只写一两个典型的代表即可，考试没有那么多时间

　　7、 领域模型的类不能有操作（也就是类的函数），如果写出来要扣分。

　　8、 如果有描述类，一定要画出来。

　　描述类是包含其他事物的信息的类。命名方式：被描述类名Description

　　被描述的事物存在，并且描述独立于事物的实例

　　比如酒店的每一个同类型的房间价格都是一样的，它并不随着房间号的变化而变化，所以把房间描述独立出来会比较好

     9、 没有描述类一定会扣分！

     整个画图的最重要步骤就是找出名词！


![领域模型](/img/system_design_notes/domain.png)

## 系统顺序图

### 重要概念

1.对象：

　　对象是特定行为与属性的集合。

　　对象的表示方式有三种：

　　a.包括对象名和类名

     

　　b.只有类名。

       

　　c.只有对象名

　

 

2.消息表示形式：

　　消息用于描述对象间交互的方式及内容。

　　消息分为四种：同步消息、异步消息、返回消息、自关联消息

　　a.同步消息：一个对象向另一个对象发出同步消息后，将处于阻塞状态，一直等到另一个对象的回应。

　　表示方式：

        

　　b.异步消息：一个对象向另一个对象发出异步消息后，这个对象可以进行其他的操作，不需要等到另一个对象的响应。

　　表示方式：

       

　　c.返回消息：同步消息的返回消息

　　表示方式：

 

　　注意：创建对象的表示法也是用虚线箭头表示！

　　d.自关联消息：用来描述对象内部函数的互相调用。

　　表示方式：

        

3.复合片段

　　为了支持有条件和循环的构造（以区别于其他事物），UML使用了图框。图框是图的区域或片段，在图框中具有操作符或标签（例如loop）和保护信息（条件子句）。

　　复合片段有多种，在此主要介绍一下几种：条件判断、可选、循环、同步

　　a.条件判断：用于描述代码中if…else…这种结构

　　标记为“alt”


　　

　　b.可选：是一种特殊的“条件判断”，它只是一个if，没有else if或else

　　可选的标记为：opt



 　　

　　c.循环：是指代码中的for、while之类的语句块。

　　循环的标记为：loop

　　例如：下图中[m,n]是指至少执行m次，最多执行n次

　　 　　

　　d.同步：用于描述多线程的情况。

　　同步的标记是：par


　　 除此之外，顺序图中还包含一种特殊的形式，引用：

　　在一个顺序图中，可以引用另一个顺序图，其引用方式类似于复合片段，


　　

4.系统顺序图：

　　UML没有定义所谓的“系统顺序图”，而只是定义了“顺序图”。这一限定强调将系统的应用视为黑盒。

　　系统顺序图是为了阐述与讨论系统相关的输入和输出事件而快速、简单的创建新的制品。

　　通常，软件系统主要对以下三种时间进行响应：

　　1）来自参与者（人或计算机）的外部事件

　　2）时间事件

　　3）错误或异常（通常源于外部）

 

注意事项：

　　通常用系统顺序图来画一个用例场景（例如主场景或复杂的常用的场景）。

　　1、首先要画一个system，前面要加个冒号，不写system，扣全部分，不写冒号扣1分，位置放错扣1分。

　　　　因为要画的是系统事件，没有系统还画什么

　　2、顺序：最左边是actor（前面也要加冒号），然后是system，然后就是用例的外部实体

　　3、通常只要求描述一个场景（主场景）。主场景是按照最理想的情况把事情做完就可以了，不需要考虑细节

 

　　4、系统顺序图通常只有3-5个事件，消息不应该超过5个！一定要仔细审题，如果某个事件操作很多，直接忽略后面那些细节，否则后面的很难做，越少越好

　　5、后置条件：直接用注释写在后面

　　6、后置条件只能写这3句话中的一句或几句：

　　　  创建什么对象或删除什么对象，修改什么属性，生成什么关联

　　　  这是整个画图考试唯一需要文字的地方

　　7、操作契约：

　　　  操作、交叉引用（用例）、前置条件、后置条件　　

 ![系统顺序图](/img/system_design_notes/uml_state.png)

 ## 包图


1. 包图的M里面的元素全都来自领域模型里面。

2. 临时变量都属于控制层，动作的命名规则是在动作后面加个Action或者Controller，一个用例就一个控制器。

3. 用例中出现的界面都属于V

　　对一个用例画包图就是把这个用例的领域模型里面的元素填到M（domain layer）和C（controller）包里面，然后给界面起个名字，然后写在V（UI）包里面即可。（老师没说要画包之间的关系，他自己画的图也没有画关系，所以干脆还是不要画了吧）

　　如果有外部支持资源，写在foundation包里面就可以了。

![包图](/img/system_design_notes/pack.png)

## 顺序图

2. 实例的创建

　　UML中要求在创建实例是使用虚线表示。实心箭头表示常规的同步信息，开放箭头表示异步调用。

　

3. 对象生命线和对象的销毁

　　在某些情况下，需显式表示对象的销毁。例如当使用没有自动垃圾回收机制的C++时，或者当需要特别指明对象不再使用时（例如关闭数据库连接），都需要如此表示。

　　UML生命线表示法提供了表示销毁的方式。

      

4、引用：

　　在一个顺序图中，可以引用另一个顺序图，其引用方式类似于复合片段，

　　标签为：ref

注意事项：

　　1.重点表示主场景是怎么实现的，不关注不成功的情况。

　　2.遵循使用BCE方法：boundary、control、entity

　　3.注意名称都是以冒号开头，冒号不写要扣分，画下划线的是静态对象

　　4.最左边的方法是把SSD里面的方法copy过来，方法不能多也不能少，顺序图是研究系统事件是如何实现的，所以必须和SSD一样的事件。只是把系统的职责转移到控制器中来实现它。

　　5.记住一定要简洁，遇到并行的就不管了，把意思表达出来就可以了

　　6.图中的控制器一定要来源于包图中的控制器

　　7.控制器左边的对象一定是UI的对象

　　8.控制器中的方法应是在顺序图和交互图中保持一致

　　9.本题重点是围绕单词，故方法都直接连到单词


![顺序图](/img/system_design_notes/sequence.png)


## 设计类图

重要概念：

1. 类图（Class Diagram）: 类图是面向对象系统建模中最常用和最重要的图，是定义其它图的基础。类图主要是用来显示系统中的类、接口以及它们之间的静态结构和关系的一种静态模型。UML用类图表示类、接口及其关联。

2. 表示类元属性的方法：

　　a.属性文本：如currentSale：Sale

　　b.关联线表示法

　　c.两者兼有

　　属性文本表示法的完整格式：visibility name : type multiplicity = default {property-string}

　　关联线表示的属性：导航性箭头+多重性（放在目标一端，而不是源的一端）+角色名（只放在目标一段，用以表示属性名称）+不需要关联名称

　　准则：通常对数据类型对象使用属性文本表示法，对其他对象使用关联线。

3.关联端点的描述法

　　关联的端点可以附加导航性箭头，也可包含可选的角色名（关联端点名）来表示属性名称。

　　关联端点还可以附加多重性值。

　　关联端点还可以使用{ordered}、{ordered、list}这样的特殊字符串。

4.对象之间的关系

 

　　a. 依赖（Dependency）
 

　　实体之间一个“使用”关系暗示一个实体的规范发生变化后，可能影响依赖于它的其他实例。更具体地说，它可转换为对不在实例作用域内的一个类或对象的任何类型的引用。其中包括一个局部变量，对通过方法调用而获得的一个对象的引用（如下例所示），或者对一个类的静态方法的引用（同时不存在那个类的一个实例）。也可利用“依赖”来表示包和包之间的关系。由于包中含有类，所以你可根据那些包中的各个类之间的关系，表示出包和包的关系。
 



 

　　b. 关联（Association）——单向关联、双向关联、多维关联、自身关联
 

　　实体之间的一个结构化关系表明对象是相互连接的。箭头是可选的，它用于指定导航能力。如果没有箭头，暗示是一种双向的导航能力。在Java中，关联转换为一个实例作用域的变量，就像图E的“Java”区域所展示的代码那样。可为一个关联附加其他修饰符。多重性（Multiplicity）修饰符暗示着实例之间的关系。在示范代码中，Employee可以有0个或更多的TimeCard对象。但是，每个TimeCard只从属于单独一个Employee。
　　单向关联：实线箭头表示
　　双向关联：实线无箭头表示
　　多维关联：关联中心有一个◇（菱形）、实线无箭头进行连接表示
　　自身关联：由自身出发，回到本身　
 



 

　　c. 聚合（Aggregation）——较强的关联关系，局部离开整体之后不会失效
 

　　聚合是关联的一种形式，代表两个类之间的整体/局部关系。聚合暗示着整体在概念上处于比局部更高的一个级别，而关联暗示两个类在概念上位于相同的级别。聚合也转换成Java中的一个实例作用域变量。
 

　　关联和聚合的区别纯粹是概念上的，而且严格反映在语义上。聚合还暗示着实例图中不存在回路。换言之，只能是一种单向关系。
 

 
 



　　d. 合成（Composition）——更强的关联关系，局部离开整体之后失效 
 

　　合成是聚合的一种特殊形式，暗示“局部”在“整体”内部的生存期职责。合成也是非共享的。所以，虽然局部不一定要随整体的销毁而被销毁，但整体要么负责保持局部的存活状态，要么负责将其销毁。
 

　　局部不可与其他整体共享。但是，整体可将所有权转交给另一个对象，后者随即将承担生存期职责。Employee和TimeCard的关系或许更适合表示成“合成”，而不是表示成“关联”。
 



　　e. 泛化（Generalization）——继承关系
 

　　泛化表示一个更泛化的元素和一个更具体的元素之间的关系。泛化是用于对继承进行建模的UML元素。在Java中，用extends关键字来直接表示这种关系。
 



 

　　f. 实现（Realization）——接口的实现
 

　　实例关系指定两个实体之间的一个合同。换言之，一个实体定义一个合同，而另一个实体保证履行该合同。对Java应用程序进行建模时，实现关系可直接用implements关键字来表示。
 



　　接口是一种特殊的类，具有类的结构但不可被实例化，只可以被实现（继承）。UML类图中接口有两种表示方法：矩形表示法（如图-2中的飞翔的接口）和棒棒糖表示法(如图-2中唐老鸭类中实现讲人话的接口)。矩形表示法，顶端有<<接口>>或者<<interface>>，第一行：接口名称，第二行：接口方法。棒棒糖表示法，圆圈旁为接口名称，接口方法在实现类中出现，如唐老鸭类中的讲话。

                                                                             

 

5.约束

　　UML约束是对UML元素的限制条件。约束以花括号之间的文本表示，如{size>10}

　　对属性的约束条件，写在属性后面。对操作的约束，以注释或后置条件的形式写出。但是都要有{}。

　

绘制要点：

　　具体方法：http://www.cnblogs.com/riky/archive/2007/04/07/704298.html

　　　　　　　http://www.uml.org.cn/oobject/201104212.asp

　　　　　　　http://www.cnblogs.com/silent2012/archive/2011/09/07/2169946.html

　　　　　　　http://developer.51cto.com/art/201007/209503.htm

　　实现实例：http://blog.csdn.net/flanet/article/details/7746004

　　根据实例来看，可以很好的理解掌握类图中需要可以熟练运用的知识点。

　

注意事项：

　　1.画图步骤

　　首先把顺序图里面的类抄过来。

　　然后查看领域模型，把领域模型里面对应这里的类的属性copy过来，然后把领域模型里面的关联到这里变成实现。

　　补充类的方法。

　　控制器类方法的数量只能与前面的系统事件数量一样，系统事件有多少个就只能写多少个方法。

　　1) 前面的has，own这种关联不能留下来，如果留下来，扣分，应把概念类图中的关联改为有箭头没文字（或有在关联端点有文字）的表示

　　2) 多重性要保持

　　3) 不要写什么set方法、get方法，这是编程的问题，不是这里的问题

　　2.一定要与交互图一一对应

　　方法和对象都要一一对应！！

　　3.Domain中先把除了UI和Controller之外的都放进去

　　分别确定每个对象的属性，依赖，实现

　　画出domain中的各个对象之间的关系（存在的方法）

　　4.领域模型==》概念透视图

　　　设计模型（DCD）==》软件透视图

 [设计类图](https://www.cnblogs.com/xiaolongbao-lzh/p/4611490.html)

 ![设计类图](/img/system_design_notes/dcd.png)

 ## 部署图

 重要概念：

　　1. 部署图

　　　部署图表示的是，如何将具体的软件制品（例如可执行文件）分配到计算节点（具有处理服务的某种事物）上。部署图表示了软件元素在物理架构上的部署，以及物理元素之间的通信（通常通过网络进行）。

　　2. 部署图中最基本的元素是节点。有两种节点：

　　　a.设备节点——具有处理和存储能力，可执行软件的物理（电子数字式）计算资源，例如典型的计算机或移动电源。

　　　　•设备（《device》）：没有处理能力的节点，至少是不关心其处理能力的节点。例如打印机、IC卡读写器，如果我们的系统不考虑它们内部的芯片，就可建模为设备

　　　b.执行环境节点——在外部节点（如计算机）中运行的软件计算资源，其自身可以容纳和执行其他可执行软件元素。

　　　　•处理器（《process》）：具有处理能力的节点，即可以执行构件

　　　　　　如，操作系统（OS）是容纳和执行操作程序的软件。

　　　　　　虚拟机（VM）容纳和执行程序。

　　　　　　数据库引擎（如PostgreSQL）接受SQL语句并执行之，并且容纳和执行内部存储过程（用Java或其它专有语言编写）

　　　　　　web浏览器容纳和执行JavaScript、Java Applets、Flash和其他可执行的元素。

　　　　　　工作流引擎。

　　　　　　Servlet容器或EJB容器。

　　　节点属性和操作：可以为一个节点提供处理器速度、内存容量、网卡数量等属性，可以为其提供启动、关机等操作

　　3. 通信路径：

　　　节点之间的一般连接表示一种通信路径，上面可以标记协议。他们通常表示网络连接。为了更好地表示两个节点之间的关系，我们可以通过“约束”来对连接进行描述。约束表示为{}。

　　4. 节点命名

　　　实例名称格式：Node Instance : node

   　　与结点的区别在于名称有下划线和结点类型前面有冒号，冒号前面可以有示例名称也可以没有示例名称

　　　通常在UML中，具体实例的名称带有下划线，如果没有下划线则代表类，而不是实例。注意，该规则对于交互图中的实例具有例外，以生命线框图表示实例，其名称没有下划线。通常，在任何情况下，我们可以看到部署图中对象实例名称带下划线。但是UML规范中规定，部署图中的下划线可以忽略。

　　5. 物件（Artifact）

    　　物件是软件开发过程中的产物，包括过程模型（比如用例图、设计图等等）、源代码、可执行程序、设计文档、测试报告、需求原型、用户手册等等。物件表示如下，带有关键字«artifact»和文档图标，或者表示为《artifact》+name。

                       

　　6. 节点和构件的联系与区别：

　　　节点的概念和构件有许多相同之处，例如二者有多名称，都可以参与依赖、泛化和关联关系，都可以被嵌套，都可以有实例，都可以参与交互。

　　　但它们之间也存在明显的区别：构件是参与系统执行的事物，而节点是执行构件的事物；构件表示逻辑元素的物理打包，而节点表示构件的物理部署

 


注意事项：

　　1.节点：分类题目中已告知

　　　根据图中所给的信息，将部署图对应画出来就可以了。

　　2.操作系统等信息在节点中表示为{OS=XXXX}

　　3.数据库与其他的通信协议为JDBC。 

![部署图](/img/system_design_notes/deploy.png)