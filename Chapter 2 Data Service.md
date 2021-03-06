# 第二章 运营数据服务 
### 本章起草人：刘栖桐（腾讯互娱运维负责人，智能运维提出方）

## 1. 整体框架
### 1.1 概述
运营数据服务是应用运维里面的核心服务之一，它是运维自动化和智能化的核心能力，也是使运维能扩展价值的重要能力。为实现这一能力，需要全面掌握应用运维的相关数据项，并通过对这些数据项的处理（采集-存储-统计/分析），使其可以用于监控和发现业务问题，进而帮助业务优化体验和性能等关键指标。

运营数据服务的服务对象包括运营、运维、研发、测试等几乎所有同运营相关的角色。

### 1.2 框架图

![](http://7xl5e0.com1.z0.glb.clouddn.com/c2%20001.jpg)

图片地址：http://7xl5e0.com1.z0.glb.clouddn.com/c2%20001.jpg

## 2. 最佳实践
### 2.1 数据项管理
  数据服务的前提是需要清楚的知道哪些数据是有价值的，对这些数据项持续管理，同时还要建立数据项之间的关联关系，明确各数据项之间的相关性。数据项可以分为宏观和微观两类，例如单位时间内的“登录成功率”是宏观数据，而“单用户登录失败”则是微观数据，宏观数据可以用于发现问题和预测趋势，微观数据则可以帮助定位具体的问题点； 

#### 2.1.1 可管理级最佳实践
##### 2.1.1.1 概述
保证运维质量是运维的核心价值之一，运维质量相关数据项首先需要被识别和管理起来，例如“登录成功率”、“点击成功率”、“支付成功率”等等；

##### 2.1.1.2 最佳实践

- 首先梳理出业务的逻辑，建议可以以用户在业务中的行为为主线进行梳理，最后形成用户行为的多条逻辑线，例如登录、购买等等
- 按照上述的用户行为逻辑线，识别相关数据，根据重要程度不同可以分为关键逻辑和次要逻辑，例如登录逻辑就是关键数据项之一。
- 建立数据项管理的流程来保证数据项的完整性和正确性
- 建立数据项管理库对数据项进行记录和维护，要求要方便维护管理
- 根据自身条件的不同，可以优先将宏观和关键的数据项管理起来（目的是先保证能发现业务的关键问题），再逐渐扩展；

#### 2.1.2 已定义级最佳实践
##### 2.1.2.1 概述
运维除了关注基础运维质量之外，也有责任帮助业务进一步掌控用户体验的状态，因此我们需要具备对用户体验数据项的识别和管理能力

##### 2.1.2.2 最佳实践
- 可以从用户行为逻辑线出发进一步梳理其中用户体验相关的数据项，例如登录逻辑中的“用户登录等待时长”等
- 数据项管理的流程覆盖到用户体验相关的数据项

#### 2.1.3 量化管理级最佳实践
##### 2.1.3.1 概述
  在一个业务以及不同的业务之间，数据项之间存在着关联性，例如在游戏业务中，“登录成功率”和“同时在线用户数”之间存在明显的关联。而在电商业务中，“支付成功率”和“交易笔数”、“单位时间交易金额”之间也存在关联。将这些数据项之间的关联性识别并管理起来，能让我们更容易的进行问题分析、故障定位和趋势预测等进一步的工作

##### 2.1.3.2 最佳实践
- 建立数据项相关性的视图，识别哪些数据项具有相关性，并明确其相关性系数
- 建立数据项相关性管理流程，因为随着业务逻辑的变化，数据项的相关性可能会发生变化
- 数据项管理库应该能够储存和维护数据项相关性的信息，并具备较好的可维护性

### 2.1.4 优化管理级最佳实践
#### 2.1.4.1 概述
数据项以及数据项相关性的维护可以不依赖人工，可自动识别和自动维护

#### 2.1.4.2 最佳实践
（暂无）

===

### 2.2 数据管理

识别出数据项之后，还需要具备将这些数据采集并管理起来的能力，以供进一步使用。数据管理包含了几个维度：数据源管理、数据采集和清洗、数据传输、数据存储、数据生命周期管理。保证数据的完整性和采传效率是数据管理的关键能力。

#### 2.2.1 可管理级最佳实践
##### 2.2.1.1 概述
业务运行质量相关的数据能够找到明确的数据源，并能被有效的采集、传输和存储，使这部分数据具备用于后续数据分析和利用的条件。但数据源未实现标准化，可能存在采集效率不高和后续维护成本高的问题；采集到数据可能没有集中存储，因此对数据进行关联分析的难度较大。在本阶段，数据管理的职责已经被明确定义出来，但负责数据管理的人可能是由运维或其他角色兼任的，没有分化出专职的人员。

##### 2.2.1.2 最佳实践
- 根据2.1.1中所确定的数据项明确相应的数据源
- 理解数据源的数据结构，根据数据源的格式确定相应的采集方案，包含要素为：采集方式（例如脚本采集、接口上报等），采集频率（根据数据项需求确定，同时需要考虑对系统的性能影响）
- 采集的数据可能进行初步清洗，也可能没有，本阶段的清洗一般是很简单和初级的，只对明显的脏数据进行过滤，清洗规则没有标准化。
- 数据采集过程中会涉及数据传输的问题，可以根据情况选择传输方式，传输时要注意有限速措施，避免占用资源过高影响服务器性能或网络性能。
- 采集到的数据可能是分散存储或集中存储的，在本阶段，数据量可能不会太大，对存储结构和存储组件的要求不高，没有标准化，有较多的选择，但需要满足数据完整性和存储性能的要求。
- 数据的安全性有保证，有严格的权限管理机制来保护数据不被非法访问或获取。

#### 2.2.2 已定义级最佳实践
##### 2.2.2.1 概述
对运维质量和用户体验相关的数据都能够明确数据源，并能被有效的采集、传输和存储，使这部分数据具备用于后续数据分析和利用的条件。在本级的阶段，随着数据项的增加（纳入了用户体验数据的采集）和数据量的增大，对数据源的管理和采集、清洗、传输和存储方案都提出了更高的要求，因此在这些领域，方案都实现了标准化，但对上述方案的管理可能还需要人工介入。在本阶段，数据管理工作量加大，可能需要专职人员负责。

##### 2.2.2.2 最佳实践
- 根据2.1.2中所确定的数据项明确相应的数据源
- 对数据源进行标准化，同数据源的owner（例如开发方）明确数据输出的标准，为后续环节的标准化打下基础。
- 建立数据源的采集、清洗、传输、存储的标准并落地。例如，同类数据的采集方式和采集频率保持统一标准等。
- 数据实现集中存储，以便于交叉汇总和关联分析。
- 因为数据源较多且数据量较大，所以需要有高性能的采集、传输、存储的方案来保障数据的完整性、一致性和时效性。此点对于进一步利用数据意义重大。

#### 2.2.3 量化管理级最佳实践
##### 2.2.3.1 概述

形成了数据管理的集中管控体系，该体系能够动态的管理和控制数据源、采集渠道和集中存储，且具备图形化的管理界面，易于数据管理人员遂行管理职责。随着数据源和数据量的进一步增大，要求数据管理方案具备对海量数据的管理能力

##### 2.2.3.2 最佳实践
- 动态管理的核心思想是自动感知、自动修复，目的是最大限度的保证数据的完整性、一致性和时效性，同时降低维护成本。数据管控体系的动态管理能力能力包含了监控、故障自愈、配置修正、自动扩缩容等要素。例如要能自动感知数据源的变化并做出相应的反应，当数据源的数据量发生变化时能及时自动扩容传输通道和存储。 
- 图形化管理界面能够实时展示数据管理各节点的状态，并能帮助数据管理人员容易的修改各配置项。当出现问题无法自动修复时，也能帮助数据管理人员及时定位到问题点并给出修复建议。
- 要具备海量的数据采集、传输、清洗和存储能力，能够保证数据的完整性、一致性和时效性。

#### 2.2.4 优化管理级最佳实践
（暂无）

===
### 2.3 数据呈现
#### 2.3.1 概述
数据呈现是数据服务的交付界面，统计的数据结果需要以某种方式呈现出来，供服务对象（运营、开发、测试等）查看；数据呈现方式很重要，好的呈现可以帮助服务对象一目了然的了解运营状况。

#### 2.3.2 可管理级最佳实践
#### 2.3.2.1 概述
通过定期报告的方式让服务对象可以大致了解运维质量的主要数据，呈现方式要便于阅读并固化下来，但该报告可能需要人工或部分人工统计生成且时效性不强。

#### 2.3.2.2 最佳实践
- 根据客户需求制定报告内容，应包含主要的运维质量数据，例如可用性、容量状况、故障状况、风险问题等。根据对象的不同，报告内容应该有针对性，所突出的重点有所不同。
- 确定报告格式，原则是便于阅读、重点突出。如果要采用图表展示，则应避免图表数据太繁杂，切忌大量数据的堆砌和罗列。阅读性太差的报告无法传递需传递的信息，长期下去容易流于形式。
- 报告发送周期根据服务对象的不同可以有日报、周报、月报等，因为需要人工整理，所以确定报告周期时需要考虑时效性和人力投入成本的平衡。

#### 2.3.3 已定义级最佳实践
##### 2.3.3.1 概述
除运维质量类数据之外，用户体验类数据也可被呈现（在部分公司，还会包含营销类数据，这取决于公司内部的分工要求是否由应用运维来负责提供此类数据），而数据呈现方式也进一步改进，有固定的页面展示，数据的时效性增强，不受报告周期的限制；同时，为了满足不同服务对象的需求，周期报告仍可保留，改为订阅及自动发送的方式。

##### 2.3.3.2 最佳实践

- 明确页面上需展示的数据以及呈现方式，建议可以根据服务对象的不同呈现不同的数据（这要求页面能识别当前阅读者的对象属性）；
- 为保障数据的安全性，页面必须具备用户权限管理的能力，数据按重要程度有分级机制，用户权限和数据分级有匹配关系
- 页面要具备自动刷新的功能
- 要有数据报告订阅功能
- 页面有监控机制，如果页面有异常可及时发现

#### 2.3.4 量化管理级最佳实践
##### 2.3.4.1 概述
本级要求数据的呈现不再按传统的分类（如可用性、故障等），而是将各类数据关联起来，以场景化的方式呈现，例如基于“一次新版本发布”的场景关联呈现各类数据，这要求各数据源之间必须明确关联关系（2.1.3中要求的能力），并形成关联模型。此外，数据系统可给服务对象建议的报告模版，也允许服务对象自定义数据展示方式。

#### 2.3.4.2 最佳实践
- 明确业务中的主要运营场景，并按照重要程度分级；
- 根据运营场景明确各场景的数据项，引入2.1.3中的数据项关联关系，并建立数据关联模型，关联模型应该包含多种策略，以保证其准确性； 
- 根据场景和数据关联模型来设计数据呈现方式；
- 整套数据呈现的框架是可配置的，新的数据项和关联关系很容易被配置进去；

#### 2.3.5 优化管理级最佳实践
##### 2.3.5.1 概述
数据中的异常点能在页面上被自动识别出来并自动关联到相应的事件。新数据甚至可以被自动识别并自动给出其建议的呈现方式（如果有相应策略的话）。本级对自动化的要求较高，甚至要求数据系统对业务有一定的理解（可以认为具备了一定的智能）
最佳实践

##### 2.3.5.2 最佳实践
（暂无）

===

### 2.4 数据监控/预测
#### 2.4.1 概述
运维收集了大量的数据，无异于一座宝库。仅在运维领域，这些数据就可以用于监控和故障自愈、业务趋势分析、用户体验优化等；但这些数据不会主动说话，要将这些数据利用起来，需要有相应的能力。

![](http://7xl5e0.com1.z0.glb.clouddn.com/c2%20002.png)

图片地址：http://7xl5e0.com1.z0.glb.clouddn.com/c2%20002.png

#### 2.4.2 可管理级最佳实践
##### 2.4.2.1 概述
运维质量（即可用性）相关的数据能被监控，本级的主要要求是将数据用于业务可用性监控，达到本级的能力要求基本可以发现某个单项可用性指标的异常，但没有关联分析，误报率可能较高。例如，通过游戏PCU监控可以发现游戏PCU陡降的问题，但无法自动发现引起PCU陡降的故障点。再例如，通过对ping探测可以发现某服务器宕机的故障，但不能自动发现该宕机故障对于业务其他指标有何影响。具体监控指标可参看“应用运维规范之监控能力模型”的建议。

##### 2.4.2.2 最佳实践

- 首先需明确监控点并关联相应的数据；
- 针对各数据设定监控方式，可以是阈值监控，也可以是趋势监控。以上监控方式应该是可配置的，便于维护，可以随时修改、打开、关闭；
- 监控之后要进一步跟告警对接起来，告警渠道要保证通畅和及时；

#### 2.4.3 已定义级最佳实践
##### 2.4.3.1 概述
除监控可用性相关的异常点之外，也能监控用户体验的异常。这要求将监控策略覆盖到用户体验数据上去，因为数据维度的增加，对于监控策略和监控模型也提出了更高的要求；

##### 2.4.3.2 最佳实践
- 明确用户体验监控点并关联相应的数据
- 针对各数据设定监控方式，因为数据复杂度增加，所以需要完善监控模型，使监控更精准

#### 2.4.4 量化管理级最佳实践
##### 2.4.4.1 概述
本级要求除了能及时发现故障异常之外，还要能通过数据间的关联关系、关联模型和智能分析决策自动定位问题点，定位成功率超过60%；并具备通过数据趋势预测风险的能力；

##### 2.4.4.2 最佳实践

- 通过数据项的关联关系（见2.1.3章节要求的能力）和关联模型（见2.3.4章节要求的能力）设置分析策略，最初策略可能比较简单，随着策略的增加，使其决策更加智能；策略中间可能有较多的逻辑树判断，例如A组件的某个指标下降，可能关联到B组件，而B组件又跟C组件有关联，可以根据此逻辑链找到问题点；
- 定位到问题点之后，系统应该能根据策略判断下一步应该如何动作，完成动作之后应该再次探测问题是否消除，通过这样的方式反复多次直到问题解决。在这个过程中，当策略不成熟时，可以设置每一步决策均需人工审核后才能执行，以保证安全。但最终目的是分析和决策可以自动执行，不需人工干预
- 趋势预测的原理同上一点类似，也需要不断验证和修正分析模型
- 以上各项功能应该统一在一套平台内，所有配置项可以灵活设置，方便各种策略的调整；
- 因为系统具备执行自主权限，为了保证安全，除了系统层面的安全监控和严格的权限控制之外，还需要设置安全阈值，即当系统执行破坏后果严重的操作时必须能及时发现和具备人工干预能力；


#### 2.4.5 优化管理级最佳实践
##### 2.4.5.1 概述
数据智能分析决策的能力具备自我学习能力，能够通过实际环境的变化自我修正其分析和决策策略，具备学习能力。

===
### 2.5 业务优化
#### 2.5.1 概述
在2.4章结中，通过对数据的分析和预测，我们可以及时发现业务的异常甚至自动恢复，本节要求我们利用此项能力持续帮助业务优化，例如，我们不光可以发现异常并恢复，我们还要深挖异常背后的原因并持续推动业务进行优化，直到问题被根本性的解决。

#### 2.5.2 可管理级最佳实践
##### 2.5.2.1 概述
运维质量相关的数据异常有被标识出来并持续关注而不止是当时解决就结束。有流程来跟进这一过程，例如问题单流程。

##### 2.5.2.2最佳实践

- 有问题单流程并形成闭环
- 有相应的管理和考核机制来保证流程的持续执行
- 流程可能需要人工跟进

#### 2.5.3 已定义级最佳实践
##### 2.5.3.1 概述
除运维质量类数据外，用户体验的异常问题也被流程覆盖进来持续跟进

##### 2.5.3.2 最佳实践
- 增加问题单的问题类别和覆盖范围
- 部分问题单可以实现自动转入，而不需要人工录入

#### 2.5.4 量化管理级最佳实践
##### 2.5.4.1 概述
数据成为业务优化的主要驱动力，这里的数据不仅仅是异常数据，例如，游戏业务中，玩家接入延时200ms也可以正常游戏，但我们认为如果提升到150ms可以使玩家获得更好的体验，从而增加玩家的留存率。受此驱动也可以发起一个优化流程来闭环此次优化；

##### 2.5.4.2 最佳实践

- 要实现此目标必须依赖2.3.3的数据关联模型，明确的关联模型能让我们清楚各种数据之前的影响关系，从而评估某个数据是不是需要优化
- 业务优化的流程有系统支持，基本实现自动录入且闭环
- 优化的过程能被持续跟踪，每一步优化之后的数据可以明确呈现出来

#### 2.5.5 优化管理级最佳实践
##### 2.5.5.1 概述
相对上一级的进步在于本级的系统可以主动发现系统中潜在的优化点并智能决策是否优化，也会自动跟进优化的结果，具备一定的智能。

##### 2.5.5.2 最佳实践
（暂无）















