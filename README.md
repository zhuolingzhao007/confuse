# confuse(iOS马甲包混淆，上架神器)

<a name="X50Qx"></a>
#                             ![image.png](https://cdn.nlark.com/yuque/0/2020/png/213807/1593768128247-016fe60b-8853-48fb-8b76-f9f702b83db5.png#align=left&display=inline&height=177&margin=%5Bobject%20Object%5D&name=image.png&originHeight=512&originWidth=512&size=119707&status=done&style=none&width=177)
<a name="KQtMH"></a>
# 警告⚠️

1. 建议暂时先不用（[插入文件]、[插入文本]），该功能优化中，目标逼近正常开发，让插入的垃圾不在是垃圾，告别所谓的垃圾
1. [重命名多语言]存在bug，某些情况下会导致项目编译出错，建议先不使用<br />
<a name="GZrkm"></a>
# 前言
因公司发展需要，本人19年中旬开始从事iOS[马甲包业务](https://www.yuque.com/docs/share/7e70244c-5dea-4035-b634-65cc082097da?#《马甲包简介》)，前期也使用过目前市面上其他得工具，实际效果不太理想。经过大量实践，开发出一款功能齐全的[马甲包工具](https://github.com/520coding/confuse)，支持OC、Lua、C++。工具的主要功能OC已封装成Mac应用，其他功能还在封装中，敬请期待。目前公测阶段: _**免费**_
<a name="qPY4i"></a>
# 提示
为了提高通用性，近期不断重构(>=v1.2.0)之前老版本的功能，为此新建测试工程[**confuse_test**](https://github.com/520coding/confuse/tree/master/confuse_test)，大家在实际使用过程中如果遇到问题，欢迎扩展测试工程，请在工程中请注明bug细节。
> 1.2.0之前的老版本说明：
> 简介：不涉及语法及编译要求，但是混淆后可能出现局部漏改或者改错，请自行添加至黑名单过滤。
> 适用项目：C++、Swift、RN等还未适配的混合项目。
> 使用条件：目前能用v1.1.3，其他过期了

<a name="ddChF"></a>
# 自述
其实识别一个工具的优劣，只需看看它能否修改方法名的所有参数名（极少）、带block的参数的方法（极少），偏移元素（较少）。更别说“还有谁...”能识别宏、区分继承链等上下文关联内容。也欢迎大家使用不同工具混淆测试工程[**confuse_test**](https://github.com/520coding/confuse/tree/master/confuse_test)，对比效果。<br />马甲包的本质：

1. 阶段一减低重复率 ，本人开发初期的版本和目前市面上的其它工具基本相似，主要是‘名称’全局替换这一个基本的功能
1. 阶段二减少相似度（相同元素的正态分布），目前该工具经过优化已经有了很大的改善，已经在慢慢往这方面靠近，详情见以下功能介绍。事物都有两面性，功能越强大混淆耗时越长，如果你的项目很大，混淆几个小时也是有可能的，请不要见怪，后续持续优化中。
<a name="ICzWQ"></a>
# 功能
confuse是一款[马甲包工具](https://github.com/520coding/confuse)，尽可能模拟人工混淆，避免机核4.3、2.1、2.3.1、账号调查等。<br />目标：**模拟人工修改一切能改的地方**，这也是为什么本工具只有黑名单没有白名单的原因<br />详细功能如下：
<a name="MQHkR"></a>
### 已完成

1. [资源替换]，混淆前指定需要替换的资源文件夹，自动进行同名文件替换，方便快捷
1. [删除注释]
1. [修改图片]，图片质量修改、大小偏移、颜色微调、透明度设置、RGB偏移、模式修改等
1. [重命名图片]
1. [重命名多语言]，支持汉字，所有字符串将被修改
1. [重命名属性]，支持@property的所有类型
   1. 基本功能，改名字、前缀类似其他工具，不做过多描述
   1. 优势：
      1. 识别语法，识别类型、继承关系，**属性名混淆和类名（包含继承链）关联**，同名属性不同类混淆后将不一致，自动识别系统属性
      1. 可设置文件名Model后缀过滤
      1. 智能名词替换
7. [重命名方法]，近似Xcode的Rename功能
   1. 基本功能：改名字、前缀类似其他工具，不做过多描述
   1. 优势：
      1. 语法相关，识别类型、继承关系，支持**多参修改，方法名混淆和类名（包含继承链）及类型关联**，同名方法不同类、同类同名方法不同类型（类方法、对象方法）混淆后将不一致
      1. 智能名词替换
      1. 智能避开系统、第三方、Pod方法，并不是‘傻瓜式’的相等判断
8. [插入方法]，插入并调用上下文关联方法，告别“垃圾代码”
   1. 优势：
      1. 根据方法的返回值类型，在分类中创建相应的方法。同时封装原方法的返回值并调用。
      1. 可多次执行，指数x2递增
9. [修改方法]，模拟人工封装调用
   1. 优势：
      1. 对原方法进行**拆分调用并根据参数类型（支持继承）局部调整**，详情见[[修改方法]参数类型汇总表](https://www.yuque.com/docs/share/315b72d9-28f9-4fa6-bf20-c40d94f2253a?#《修改方法-支持参数类型汇总表》)
      1. 可多次执行，指数x2递增
10. [重命名全局变量]，智能名词替换
10. [修改全局变量]，替换全局变量名、**全局变量转化为全局函数**、混淆字符串变量值
10. [修改局部变量]，模拟人工封装调用，变量名关联类型
   1. 优势：
      1. 局部变量值运行时保持不变，详情见[[修改局部变量]修改局部变量-支持类型汇总表](https://www.yuque.com/docs/share/90444065-4f4e-49c8-9e1a-5bd3d3b4f84d?#《修改局部变量-支持类型汇总表》)
      1. 可多次执行，指数x2递增
13. [修改字符串]，加密处理（硬编码->Byte数组）,自定义设置‘最少长度’、‘有效个数’
13. [修改xib、storyboard]，**插入垃圾视图，并修改内部结构属性**
13. [修改字体]，对项目中使用的字体随机微调，**识别宏**
13. [修改颜色]，对项目中UI控件颜色随机偏移，**识别宏**
13. [UI布局偏移]，支持Frame，**Mansonry**以及**SDAutoLayout**
13. 优化中...~~[插入文件]，插入ViewController类文件，相互调用及源文件调用，支持自动、收入导入项目~~
13. [插入属性]，类中自动初始化、调用及销毁
13. [插入图片]，类中自动初始化、调用及销毁
13. 优化中...~~[插入文本]，文件（json、txt、doc）~~
13. [重命名类]，可指定添加前缀
   1. 智能名词替换
   1. 可设置‘重命名同名文件’
   1. 可设置‘重命名相似字符串’，(忽略|相等|包含)三种设置
23. [修改文件属性]，如创建时间、访问时间、修改时间
23. [修改项目]，基本配置信息，例如：版本号、SDK的BundleID

_以上所有功能均支持黑名单过滤，对指定的内容进行屏蔽，忽略混淆。_
<a name="r8lqr"></a>
#### 名词解释

- 智能名词替换:重命名时使用关联类型已有信息+相近语义+类型+部分旧词汇等组合，~~弃用‘随机单词无脑组合’~~
<a name="OEesy"></a>
### 规划中
更新迭代将按照以下顺序依次进行

1. Objective-C，重构的目的是为了提高工具的通用性和稳定性，及强化功能
   1. 重构[插入属性]、重构[重命名图片名]
   1. 优化插入垃圾，目标逼近正常开发，让插入的垃圾不在是垃圾，告别所谓的垃圾
      - [插入文件]，提取项目原有信息，进行合理组合并创建类，然后在源文件调用，支持自动、收入导入项目
      - [插入文本]，文件（json、txt、doc），尽可能模拟正常项目资源配置
   3. 重构《多语言》
   3. 移除混淆前需要创建目录Confuse、Discard要求（这部分是老代码，需要些时间适配，请见谅）
2. C++，现有功能还不具备通用性，暂时不开放，准备重构中...
   1. 字符串加密混淆
   1. 方法
      1. 重命名，（完成50%，暂停，准备把OC部分做全）
      1. 插入
      1. 修改
   3. 属性
      1. 重命名
      1. 修改
      1. 插入
3. Cocos2d-x，现有功能不具备通用性，准备整合至C++中
3. Lua的针对性太强了，暂时不开放，暂时不打算重构有需要在说吧
3. Swift，本人实际项目使用不多，故排在最后，看用户需求再决定
<a name="vlfzY"></a>
# 图文介绍
运行APP效果图，使用前请详细阅读[工具使用教程](https://www.yuque.com/docs/share/edd2603f-d09d-4795-ae71-b42419b99446?#《confuse使用说明》)<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/213807/1594644980313-b3ee8604-9652-4bba-bb18-3d06399593e9.png#align=left&display=inline&height=540&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1080&originWidth=1920&size=537018&status=done&style=none&width=960)
<a name="WtuYs"></a>
# 更新日志
<a name="sEHpd"></a>
# v2.4.0（2020.11.16）

1. 重构[重命名类]
   1. 移除类名限制条件（1.小写开头 2.名称长度小于4），现在无限制
   1. 移除随机拼接单词，使用智能名词替换
   1. 移除前缀只能大写限制
   1. 新增‘重命名同名文件’开关
   1. 新增‘重命名相似字符串’，(忽略|相等|包含)三种设置

[查看更多历史更新记录](https://www.yuque.com/docs/share/39f2f60e-b6a8-443b-b005-b9364fb79b95?#《confuse更新说明》)
<a name="63ca6131"></a>
# 感谢反馈
[shizu2014](https://github.com/shizu2014)、[myhonior](https://github.com/myhonior)、[imbahong](https://github.com/imbahong)
<a name="BUG"></a>
# 链接导航

1. [工具使用教程](https://www.yuque.com/docs/share/edd2603f-d09d-4795-ae71-b42419b99446?#《confuse使用说明》)
1. [软件使用问答（Q&A）](https://www.yuque.com/docs/share/4a87ec96-80fe-4d25-873d-93cb428b3e15?#《软件使用问答（Q&A）》)
1. [[修改方法]参数类型汇总表](https://www.yuque.com/docs/share/315b72d9-28f9-4fa6-bf20-c40d94f2253a?#《修改方法-支持参数类型汇总表》)
1. [[修改局部变量]修改局部变量-支持类型汇总表](https://www.yuque.com/docs/share/90444065-4f4e-49c8-9e1a-5bd3d3b4f84d?#《修改局部变量-支持类型汇总表》)
