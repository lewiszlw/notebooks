后端技术方案模板

什么工作需要技术方案：
- 调研、方案制定和开发（不包含测试上线）耗时在 x 天及以上时
- 特殊情况，一旦修改出错影响面很广时

备注：
- 设计方案前先调研业界优秀解决案例
- 方案设计一定是长期目光，避免为了短期效益给系统造成长期损害
- 方案设计避免重复造轮子，多看下开源或者公司提供的好用的轮子
- 方案也会对外公布，会反应出我们组以及方案制定者的一个技术形象和技术能力
- 模板是一个比较完整的方案，可根据自己需求和方案特点，按需选择模板内容。

以下为技术方案模板内容：

------------------------

# 背景
（描述业务问题，同时提供需求文档链接）

# 需求
（从两方面描述需求）
## 功能性需求
（指那些需要和PM对齐的需求）
## 非功能性需求
（如系统响应时间、可用性几个点、安全等）

# 名词解释
（通俗解释方案中的一些专有名词，或对某些词加下备注或限定范围）

# 系统现状
（当前系统架构、核心流程，让读者快速熟悉该系统，尤其是需求涉及的那部分）

# 技术方案
（你调研后认为的最合适的正式方案）
## 前提假设
（该方案有哪些前提，如必须上游系统保证足够高可用性）
## 方案细节
（流程图、数据模型、API、如何解决需求的）
## 成功指标
（哪些指标能够反馈该方案是否成功）

# 替代方案
（调研过程中一些被抛弃的方案）
## 方案XXX
（描述弃用方案）
## 方案XXX
（描述弃用方案）
## 优缺点比较
（比较所有弃用方案和正式方案各类优缺点）

|          |正式方案 | 方案xxx | 方案xxx |
|----------|--------|--------|--------|
|可用性     |        |        |        | 
|可扩展性   |        |        |        | 
|复杂性     |        |        |        | 
|耗费人力   |        |        |        | 
|xxx       |        |        |        | 


# 运维监控
（监控告警、机器型号和申请、服务运维）

# 方案缺陷
（正式方案会给系统带来哪些缺陷，如何规避）

# 任务拆分和排期
（将方案拆成独立可执行小任务，并估算人力）

|模块       |子任务 | 工作量（天） | 排期 | 状态 |
|----------|-------|-----------|------|-----|
|模块xxx   |       |           |      |     |


# 测试方案
（单测、集成测试、手动测试case等等）

# 上线方案
（上线需要哪些步骤，如数据库修改sql、等待上游系统xxx先上线；以及上线出现问题如何回滚；以及上线后如何验证，如手动测试、或观察xxx指标）

# 常见问题
（提前作为读者想下大家会问哪些问题，以及上面哪些点没讲清楚适合放在这部分）

# 附件
（贴上调研、引用文档的链接）

# 评审记录
2022/1/10 
- 评审结论：该方案评审通过 / 该方案需要确认xxx后再次评审
- 会议纪要：xxx
- 跟进项：xxx

