<div align="center">
<h1 align="center">UniTask</h1>
</div>

# 简介



联通手机营业厅自动完成每日任务



# 目录

- [简介](#简介)
- [目录](#目录)
- [功能](#功能)
- [使用方式](#使用方式)
  - [腾讯云函数（推荐）](#腾讯云函数推荐)
    - [1.fork本项目](#1fork本项目-1)
    - [2.准备需要的参数](#2准备需要的参数-1)
    - [3.将参数填到Secrets](#3将参数填到secrets-1)
    - [4.部署](#4部署)
- [通知推送方式](#通知推送方式)
- [同步上游代码](#同步上游代码)
- [申明](#申明)
- [参考项目](#参考项目)

# 功能

* [x] 沃之树领流量、浇水(12M日流量)
* [x] 每日签到(1积分+翻倍4积分+第七天1G流量日包)
* [x] 天天抽奖，每天三次免费机会(随机奖励)
* [x] 游戏中心每日打卡(连续打卡，积分递增至最高7，第七天1G流量日包)
* [x] 游戏中心宝箱100M任务(100M日流量+随机奖励并翻倍)
* [x] 4G流量包看视频、下软件任务(90M+150M七日流量)
* [x] 每日领取100定向积分 
* [x] 积分抽奖，每天最多抽30次(中奖几率渺茫)
* [x] 冬奥积分活动(第1和7天，可领取600定向积分，其余领取300定向积分,有效期至下月底)
* [x] 获取每日1G流量日包(截止日期暂时不知道)
* [x] 邮件、钉钉、Tg、企业微信等推送运行结果
* [x] 自动激活即将过期流量包（到期时间1天内）

# 使用方式


## 腾讯云函数（推荐）

### 1.fork本项目


### 2.准备需要的参数

* 开通云函数 `SCF` 的腾讯云账号，在[访问秘钥页面](https://console.cloud.tencent.com/cam/capi)获取账号的 `TENCENT_SECRET_ID`，`TENCENT_SECRET_KEY`

> 注意！为了确保权限足够，获取这两个参数时不要使用子账户！此外，腾讯云账户需要[实名认证](https://console.cloud.tencent.com/developer/auth)。

* 依次登录 [SCF 云函数控制台](https://console.cloud.tencent.com/scf) 和 [SLS 控制台](https://console.cloud.tencent.com/sls) 开通相关服务，确保您已开通服务并创建相应[服务角色](https://console.cloud.tencent.com/cam/role) **SCF_QcsRole、SLS_QcsRole**

* 手机号，服务密码，appId等等（参考[2.准备需要的参数](#2准备需要的参数)）

### 3.将参数填到Secrets

`Name`和`Value`格式如下：
  
Name | Value
-|-
TENCENT_SECRET_ID | 腾讯云用户SecretID(需要主账户，子账户可能没权限)
TENCENT_SECRET_KEY | 腾讯云账户SecretKey
USERS_COVER | config.json中内容

如对于`Secrets`不知如何添加，参考[3.将参数填到Secrets](#3将参数填到secrets)

![](https://draw-static.vercel.app/UnicomTask/云函数添加Secrets.png)

### 4.部署

* 添加完上面`3`个`Secrets`后，进入`Actions`(上面那个不是`Secrets`下面那个) --> `deploy for serverless`，点击右边的`Run workflow`即可部署至腾讯云函数(如果出错请在红叉右边点击`deploy for serverless`查看部署任务的输出信息找出错误原因)。

* 首次`fork`可能要去`Actions`里面同意使用`Actions`条款，如果`Actions`里面没有`deploy for serverless`，点一下右上角的`star`，`deploy for serverless`就会出现在`Actions`里面。（参考[4.开启Actions](#4开启actions)）

还可本地部署到腾讯云，详情见 [云函数本地部署](https://github.com/srcrs/UnicomTask/discussions/140)。

# 通知推送方式

## 1.邮箱

本方式较简单，只需要填写邮箱即可，由于使用的是公共`API`接口，稳定性不高

## 2.钉钉机器人

钉钉群组自定义机器人，配置稍微复杂一些，但是稳定性高，使用方式参考如下：

[钉钉自定义机器人使用方式](https://developers.dingtalk.com/document/app/custom-robot-access)，注意安全设置部分，选择自定义关键词，填写`UnicomTask`。

## 3.TgBot机器人

类似于钉钉机器人，只需要一个`token`和`userId`，自行搜索这两个参数的获取方式。

## 4.pushplus机器人

类似于钉钉机器人，只需要一个`token`，参考[获取pushplus的token](http://www.pushplus.plus/login?redirectUrl=/message)。注意，升级到了新接口，要重新申请`token`。详情见：[更新推送加推送接口](https://github.com/srcrs/UnicomTask/issues/134)

## 5.企业微信应用通知

企业微信自建应用，可发送消息，并且可以不借助第三方，将消息转发到普通微信。用电脑，进行[企业微信登录](https://work.weixin.qq.com/wework_admin/loginpage_wx)，普通微信扫码也可登录，，按照[此教程](https://note.youdao.com/ynoteshare1/index.html?id=351e08a72378206f9dd64d2281e9b83b&type=note#/)获取需要的三个值。

# 同步上游代码

## 将参数填到Secrets

> 注意！为了确保 Push 权限足够，需要 Github Personal access tokens

在`Secrets`中的`Name`和`Value`格式如下：

Name | Value
-|-
TOKEN | Github Personal access tokens

在最新的代码中，已经加上自动同步上游代码的`Action`，将会定时在每周五`16`点执行，文件地址在`.github/workflows/auto_merge.yml`。

同时您也可以安装[pull](https://github.com/apps/pull)应用，也可实现自动同步上游代码。

# 申明

本项目仅用于学习。

# 参考项目

[mixool/HiCnUnicom](https://github.com/mixool/HiCnUnicom)，感谢该项目对于登录部分的思路

[happy888888/BiliExp](https://github.com/happy888888/BiliExp)，参考了该项目的云函数实现
