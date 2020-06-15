# Java基础开发规范

### 规范检查 <a id="h2-u89C4u8303u68C0u67E5"></a>

本地开发开启两种规范检查，一种为sonarQube的规范检查，一种为基于sun和google的checkStyle规范检查。

同时在CI的SonarQube代码检查中也生效了这两种规范检查，控制代码质量。

* 本地开启sonarQube规范检查：

在Idea中安装sonarLint插件：

![](https://static.bookstack.cn/projects/choerodon-v1.6/50a1324449671267bb9464ca9a385d7a.png)

![](https://static.bookstack.cn/projects/choerodon-v1.6/39279b14c5379430bed5aac420f6db0c.png)

指定本地修改的文件或者全项目检查：

![](https://static.bookstack.cn/projects/choerodon-v1.6/a068406c79959c53032bf25798baa633.png)

查看不符合检查的地方：

![](https://static.bookstack.cn/projects/choerodon-v1.6/63fb1b5f11a026f178013b010201195f.png)

* 本地开启checkStyle规范检查,注意更新插件，否则部分规则不适用于旧版本会报错：

在Idea中安装CheckStyle-IDEA插件，步骤与上面安装插件一样

生效checkStyle中checkStyle的规范config：[choerodon\_checks.xml](https://rdc.hand-china.com/gitlab/io.choerodon/coding_guidelines/raw/master/checkConfigure/choerodon_checks.xml)![Java&#x57FA;&#x7840;&#x5F00;&#x53D1;&#x89C4;&#x8303;  - &#x56FE;5](https://static.bookstack.cn/projects/choerodon-v1.6/63fb1b5f11a026f178013b010201195f.png)

附:

sonarQube规范检查规则链接：[SonarRules](https://rules.sonarsource.com/java/RSPEC-3281)

googleCheck规范检查规则链接：[googleCheck](http://checkstyle.sourceforge.net/google_style.html)

alibaba p3c代码规范：[aliP3c](https://github.com/alibaba/p3c)

### idea设置 <a id="h2-idea-"></a>

* 修改import分组、排序规则

![](https://static.bookstack.cn/projects/choerodon-v1.6/06074b009acd43b649f688dfc0a1864a.png)

* 换行设置

![](https://static.bookstack.cn/projects/choerodon-v1.6/41ace657504e7e96a60a673b71cc6ff6.png)

* tab设置

![](https://static.bookstack.cn/projects/choerodon-v1.6/32772d67a680f977bd862ee505fd5aa4.png)

