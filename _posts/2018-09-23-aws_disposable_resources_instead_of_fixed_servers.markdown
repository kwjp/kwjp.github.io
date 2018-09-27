---
layout: post
title:  "AWS Design Principles - Disposable Resources Instead of Fixed Servers"
date:   2018-09-23 00:00:00 +0000
categories: aws
---

在传统的IT环境里，我们得不断地修复资源和导入新资源，这需要我们不停地登陆到服务器去配置软件或修复bug，写死各种IP地址，跑测试用例，或者执行其他一系列操作。

然而当你面向aws设计系统时，就得抛弃上面这些想法，这样才能利用好云计算的动态资源特性。你可以把服务器和其它组件都当作临时资源，在你需要时可以启用任意数量的资源，然后在不需要时把他们随时关闭掉。

云计算的另一个优势，就是可以解决长时间运行的服务器的“配置漂移”问题。随着时间的推移，需求的变更和软件的补丁版本会导致系统难以测试，并且在不同环境中配置不一致。这个问题可以通过“持久架构模式”来解决，在这种模式下，一台服务器在从启动到废弃的整个生命周期当中，它的配置都不会在改变，当遇到bug或者需要更新时，就用一台已经有最新配置的符合要求的资源来替代它。因此，该模式可以保证计算机资源始终保持一致性、可测性，也更便于回滚


当你想要部署一台测试服务器，或者是因为既存系统工作量过载而需要扩容增加服务器时，你肯定不想手动地去配置这些新资源，让这些重复性工作自动完成可以节约时间并有效避免人为错误

有以下途径可以实现自动部署

### Bootstrapping
使用默认配置启动EC2或者RDS,然后再执行一些自动化配置脚本来安装软件或者拷贝数据，
从而使这台新的资源达到一个特定的状态，通过在脚本里设置参数来实现不同环境的初始化(比如开发环境和测试环境)，从而实现脚本的复用

你可以使用数据脚本或者cloud-init和AWS OpsWorks lifecycle events来实现EC2的自动初始化，还可以使用Chef or Puppet。AWS OpsWorks原生支持Chef recipes or Bash/PowerShell scripts。通过使用这些脚本，并配合上AWS APIs或者AWS CloudFormation，可以实现几乎所有aws资源的开通和初始化操作

### Golden Images
EC2,RDS,EBS都可以从golden image上启动，golden image指的是一个特定状态的快照(snapshot),比起Bootstrapping方式来，Golden Image方式起始更快，而且也比避免了对配置服务或第三方脚本工具的依赖，在自动弹性环境中，当想要快速而又稳定地启动更多资源来响应需求变动时，Golden Images策略就显得很重要。

你可以个性化设置EC2，然后通过创建AMI来保存这些配置，你可以从这个AMI上启动任意台EC2, 而且他们都已经应用了你保存的个性化配置。每当你想改变你的配置时，就需要创建一个新的Golden Image，这要求你得有针对AMI的版本管理约定。我们建议你使用Bootstrapping策略的脚本来创建新的EC2，然后将这台EC2保存一个Golden Image，这样更便于日后的测试和调整。


另外，如果你已经有了一个虚拟环境的镜像，那也可以通过VM的导入导出功能，将其转化成一个AMI。当然你也可以使用别的aws分享的AMI，或者通过第三方团体提供的AWS Community AMI catalog以及AWS Marketplace来获取AMI。

虽然Golden Image策略大部分情况是用于EC2，但是它也是可以用在RDS和EBS上。比如当你想要创建一个测试环境时，数据库就可以通过RDS的Gloden Image来创建，这样就不用通过SQL脚本来导入导出数据了。

另一个流行的做法是Docker，


