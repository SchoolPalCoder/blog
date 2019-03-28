title: JMeter在消息中心中的应用
author: 校宝在线
date: 2019-03-28 10:31:45
tags: 
- JMeter
categories: 
- 测试
---
# JMeter在消息中心中的应用

<a name="ab25aa93"></a>
## 1 目的
自动化发送线索下次沟通时间提醒消息和新线索分配提醒消息


<a name="aea77d68"></a>
## 2 JMeter测试计划总架构
![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553065411407-8d533d1b-571f-4fd4-be74-31f9777b48a5.png#align=left&display=inline&height=295&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=295&originWidth=351&size=16906&status=done&width=351)

<a name="72922ca6"></a>
<!--more-->
## 3 组件讲解
<a name="04871bc5"></a>
### 3.1 用户自定义参数-02 与 用户自定义参数-01

**组件名：**User Defined Variables<br /><br /><br />**定义：**用户自定义参数-02是对测试02环境的参数配置，用户自定义参数-01是对测试01环境的参数配置<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553066378701-46b9a6c9-620e-414f-9e3e-b15ffa6af073.png#align=left&display=inline&height=173&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=341&originWidth=561&size=17414&status=done&width=285)            ![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553066406880-a81f1dda-21f2-4df8-98f5-73289765a5f3.png#align=left&display=inline&height=173&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=341&originWidth=562&size=17441&status=done&width=285)

**目的：**方便后期统一更改参数

**使用方法：**针对测试02环境时，可以将用户自定义参数-02置为enable，将用户自定义参数-01置为disable，反之亦然<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553066775638-bda27502-fc5d-4392-82f3-2cbc696f976f.png#align=left&display=inline&height=271&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=423&originWidth=360&size=14539&status=done&width=231)

<a name="ffa14158"></a>
### 3.2 登录（Login） 与 HTTP用户信息缓存管理

**组件名：**<br />登录（Login）：HTTP Request<br />HTTP用户信息缓存管理：HTTP Cookie Manager

**定义：**<br />Method：Post<br />Path：api/MemberShip/Login<br />Content encoding：UTF-8<br />参数说明：<br />name：用户名，使用用户自定义参数中的UserName<br />password：密码，使用用户自定义参数中的time和UserPassWord，并使用md5加密<br />timestamp：时间戳，使用用户自定义参数中的time<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553067040306-d69ef217-4124-4479-9563-b6a04de8401b.png#align=left&display=inline&height=389&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=400&originWidth=768&size=19719&status=done&width=746)

**功能：**<br />登录（Login）即实际操作中的登录，HTTP用户信息缓存管理可以将用户session保存至过期，使用户在脚本运行期间保持登录状态

<a name="b0078c68"></a>
### 3.3 HTTP请求默认值（IP）

**组件名：**HTTP Request Defaults

**定义：**<br />Protocol【http】：https<br />Server Name or IP：请求IP，使用用户自定义参数中的IP**![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553068092504-5b958823-f820-4b46-a50b-a7a53831667e.png#align=left&display=inline&height=235&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=341&originWidth=767&size=11191&status=done&width=528)**

**目的：**方便后期更改IP<br /><br />
<a name="59d7a114"></a>
### 3.4 HTTP头管理

**组件名：**HTTP Header Manager

**定义：**<br />参数：name（content-type），value（application/json）<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553068335496-b67e34df-bcb6-4903-9b85-0b7740205967.png#align=left&display=inline&height=118&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=202&originWidth=768&size=5010&status=done&width=448)

**目的：**统一设置HTTP请求中的请求头

<a name="79c2637b"></a>
### 3.5 用户组

**组件名：**Thread Group<br /><br /><br />**定义：**<br />Number of Threads（users）：用户数，设定为1个用户<br />Ramp-Up Period（in seconds）：所有用户在该时间内创建完毕，设定为1秒<br />Loop Count：循环次数，设定为1次，即不循环<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553068444296-c7cb880e-f73d-4933-8ee5-13ebfdbdaa1b.png#align=left&display=inline&height=313&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=403&originWidth=738&size=13372&status=done&width=574)

<a name="23106909"></a>
### 3.6 获取当前账户机构用户（GetCurrentUser）

**组件名：**HTTP Request

**定义：**<br />Method：Get<br />Path：api/MemberShip/GetCurrentUser<br />Content encoding：UTF-8<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553069288550-c25cbb66-9cd8-4046-b0ed-82d1ee1d73f9.png#align=left&display=inline&height=257&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=328&originWidth=727&size=15199&status=done&width=570)

**后续提取数据组件名：**JSON Extractor

**后续提取数据组件定义：**<br />Name of created variables：创建的变量名，用于存储提取出的数据<br />JSON Path expressions：用于提取数据的JSON路径<br />Default Values：若提取出来无数据，则使用该默认值<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553069480401-1567c0ea-20cc-4f09-96dc-3380dcbbd405.png#align=left&display=inline&height=203&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=261&originWidth=730&size=11989&status=done&width=568)

<a name="dd655416"></a>
### 3.7 Loop Controller

**组件名：**Loop Controller

**定义：**<br />Loop Count：循环次数，设置为8次，即每个用户运行该controller下的请求8次<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553069967651-3bbb6d7c-49b0-4a6a-a40c-eced0e6dda5f.png#align=left&display=inline&height=115&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=160&originWidth=733&size=3587&status=done&width=529)<br />

<a name="17300b60"></a>
### 3.8 CSV Data Set Config -02 与 CSV Data Set Config -01

**组件名：**CSV Data Set Config

**定义：**CSV Data Set Config -02用于提取测试02环境的线索ID，CSV Data Set Config -01用于提取测试01环境的线索ID<br />Filename：要读取的csv文档路径<br />File encoding：UTF-8<br />Variable Names：创建的变量名，用于存储从csv读取的数据<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553070502582-93b59159-c672-4080-a364-43a45607a25e.png#align=left&display=inline&height=218&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=328&originWidth=736&size=15780&status=done&width=490)<br />
![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553070476347-a28a0ab0-cbc0-4d44-94ea-e5edf567da39.png#align=left&display=inline&height=207&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=312&originWidth=744&size=15752&status=done&width=494)

![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553070734312-75daebae-4dbf-47e0-893f-a9f5426bdf88.png#align=left&display=inline&height=195&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=222&originWidth=239&size=23244&status=done&width=210) ![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553070759135-3ade771b-e72d-4d3a-87b9-5bf72bb4f471.png#align=left&display=inline&height=196&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=198&originWidth=193&size=18345&status=done&width=191)

<a name="c4eaa5e0"></a>
### 3.9 添加沟通记录（ClueCommunication/Save）

**组件名：**HTTP Request

**定义：**<br />Method：Post<br />Path：api/ClueCommunication/Save<br />Content encoding：UTF-8<br />参数说明：<br />clueId：线索ID，使用CSV Data Set Config中提取的数据<br />clueCommunicationTypeId：沟通方式ID，使用用户自定义参数中的clueCommunicationTypeId<br />clueIntentionalId：意向度ID，使用用户自定义参数中的clueIntentionalId<br />clueStatusId：线索状态ID，使用用户自定义参数中的clueStatusId<br />clueClassifyIds：线索分类ID数组，使用用户自定义参数中的clueClassifyIds<br />effectiveStatus：有效状态，使用用户自定义参数中的effectiveStatus<br />communicationTime：沟通时间，使用用户自定义参数中的communicationTime<br />communicationContent：沟通内容，使用用户自定义参数中的communicationContent<br />![微信图片_20190320145844.png](https://cdn.nlark.com/yuque/0/2019/png/279481/1553070921880-1ccca5a0-6ca0-4bba-a54e-45895a8d4a96.png#align=left&display=inline&height=412&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190320145844.png&originHeight=535&originWidth=742&size=28875&status=done&width=571)<br />

<a name="aa8ed694"></a>
### 3.10 批量分配（BatchDistribute）

**组件名：**HTTP Request

**定义：**<br />Method：Post<br />Path：api/Clue/BatchDistribute<br />Content encoding：UTF-8<br />参数说明：<br />clueIds：线索ID数组，使用用户自定义参数中的clueIds<br />recruiterId：招生顾问ID，使用获取当前账户机构用户（GetCurrentUser）中提取出的Public_Operability_OrgUserId

<a name="ca8f529d"></a>
### 3.11 Debug Sampler

**组件名：**Debug Sampler

**目的：**用于统一查看测试计划中的参数

<a name="735f0983"></a>
### 3.12 View Results Tree

**组件名：**View Results Tree

**目的：**用于统一查看测试计划中请求运行结果















