# SkyAutoPlayerScript

使用Auto.js提供的无障碍权限实现在Sky光遇中自动弹奏[SkyStudio](https://play.google.com/store/apps/details?id=com.Maple.SkyStudio)导出的曲谱

A script to play Sheets generated by SkyStudio automatically in game Sky with accessibility services using Auto.js

## 特性 / Feature

相比于其他脚本，SkyAutoPlayerScript拥有以下优势

There are many features in SkyAutoPlayerScript compared to other auto player scripts.

* 全GUI操作，无需编辑任何代码，流畅的UI动画。<br>Friendly GUI, no code edit, fluent ui animation.
* 多功能的弹奏控制面板，支持**暂停**， **进度控制**， **倍速控制**等<br>Multi-functional player control panel with **pause**, **progress control** and **speed control**.
* 通过引导自设定键位坐标，避免按压琴键的偏移问题<br>Set key coordinates by yourself with guidence, avoiding key offset when playing.
* 在线[共享乐谱](https://github.com/StageGuard/SkyAutoPlayerScript/tree/master/shared_sheets)，有许多优质乐谱。</br>~~(甚至有的乐谱复杂到根本无法手弹)~~<br>There are many excellent online [shared sheets](https://github.com/StageGuard/SkyAutoPlayerScript/tree/master/shared_sheets).
* 自动更新，及时修复BUG，无需担心版本过时问题。<br>Automatically update script.
* [多语言支持](#翻译--translation)<br>[Multi-language support](#翻译--translation).
* ...

## 使用 / Usage

①Auto.js `4.1.1 Alpha2 (461)` 版本下载: [`Ericwyn/Auto.js/releases@V4.1.1.Alpha2`](https://github.com/Ericwyn/Auto.js/releases/tag/V4.1.1.Alpha2)

Download `Auto.js`[`Ericwyn/Auto.js/releases@V4.1.1.Alpha2`](https://github.com/Ericwyn/Auto.js/releases/tag/V4.1.1.Alpha2)

手机请下载 `autoJs-V4.1.1.Alpha2-common-armeabi-v7a-debug.apk`

Please download `autoJs-V4.1.1.Alpha2-common-armeabi-v7a-debug.apk`

②为Auto.js开启**无障碍服务**和**悬浮窗权限**。

Turn on **Accessibility service** and allow **Display pop-up window** permission for Auto.js

③在Auto.js中新建一个脚本并粘贴以下代码并运行：

Create a new script file in Auto.js. Copy the code below and run!

```javascript
"ui";
"use strict";
var emitter = events.emitter(threads.currentThread());
threads.start(function() {
  emitter.emit("evaluate", (function(){
    var resp = http.get("https://gitee.com/stageguard/SkyAutoPlayerScript/raw/master/source/SkyAutoplayer.js");
    if(resp.statusCode >= 200 && resp.statusCode < 300) {
      return resp.body.string();
    } else {
      resp = http.get("https://cdn.jsdelivr.net/gh/StageGuard/SkyAutoPlayerScript@" + http.get("https://gitee.com/stageguard/SkyAutoPlayerScript/raw/master/gitVersion").body.string() + "/source/SkyAutoplayer.js");
      if(resp.statusCode >= 200 && resp.statusCode < 300) {
        return resp.body.string();
      } else {
        return "console.show();console.log(\"Failed to load script\")";
      }
	}
  }()));
});
emitter.on('evaluate', function(s){
  eval(s);
});
```

在Auto.js版本 `4.1.1 Alpha2 (461)` 中测试通过，不保证其他版本的兼容性(取决于其他版本相对于此版本的API是否有`breaking changes`)

## 清除数据 / Clear data

`SkyAutoPlayerScript` 在使用过程中会产生本地数据存储，若想全部删除，请使用Auto.js执行以下代码

`SkyAutoPlayerScript` will store data while running, if you want to delete all data, please run the code below in Auto.js

```
storages.remove("StageGuard:SkyAutoPlayer:Config");
files.removeDir("/storage/emulated/0/Documents/SkyAutoPlayer/");
```

# 上传乐谱 / Upload sheets

SkyAutoplayerScript 可以从这个仓库中的 `shared_sheets.json` 读取在线共享乐谱列表并可以方便地下载和弹奏。

SkyAutoplayerScript will load online shared sheets from the file `shared_sheets.json` in this repository and it is convenient to download and play.

你可以通过以下两种方式的任意一种方式来让你自制或经过原作者授权转载的乐谱在这个列表显示：

If you want to let sheets that transcribed by yourself or permitted to reprint shown on the list, you can do one of the following ways:

### 1. Pull Request

你可以克隆这个仓库，将你的乐谱添加到 `shared_sheets` 文件夹中，并按照以下要求在 `shared_sheets.json` 中添加新的项目：

You can clone this repository, put your sheet file into `shared_sheets` folder and add a new item in `shared_sheets.json` :

```javascript
{
  //乐谱名称 / sheet name
  "name": "Vicetone - Nevada",
  //文件名 / sheet file name in shared_sheets folder
  "file": "Nevada.txt",
  //乐谱作者 / transcriber
  "author": "StageGuard",
  //乐谱简介 / short description about this sheet
  "desc": "Nevada SkyStudio钢琴版。\n内包含<u>比较复杂的和弦</u>，不适合手弹(笑\n你可以在SkyStudio的练习模式试试[狗头]",
  //乐谱文件中的BPM / BPM in sheet file
  "bpm": 497,
  //乐谱的键位数目(它是一个 15 键位乐谱还是 8 键位乐谱)
  //key count(a 15 key sheet or 8 key sheet)
  "keyCount": 15,
  //乐谱文件中的音高 / pitch level in sheet file
  "pitchLevel": 3,
  //键的数目 / note count
  //乐谱文件中数组 songNotes 的长度 / length of array songNotes in sheet file
  "noteCount": 1308,
  //你的社交链接 / your social link
  //你可以添加多个 / you can add many links if you want
  "social": [
    {
      //社交平台名称代号，现在支持 github, twitter, douyin(tiktok) and coolapk
      //platform code name, currently support github, twitter, douyin(tiktok) and coolapk
      "platform": "coolapk",
      //社交平台名称 / social platform name
      "name": "酷安",
      //社交连接 / social link
      "link": "http://www.coolapk.com/u/808787"
    }
  ]
},
```

修改完成后，申请 `Pull Request` ，等待 merge 即可。

After finishing, you need to create a new `pull request` and wait it to be merged.

> 请注意：在申请`Pull Request`之前请确保你本地的 仓库已同步至最新，以免出现意外问题！
>
> Attention: Before creating a new `pull request` , make sure your local repository is up-to-date!

### 2. 如果你不是很懂 Github...<br>If you are not familiar with Github...

只需要把乐谱发送到邮箱 [beamiscool@qq.com](mailto:beamiscool@qq.com) 来交给我就行啦！别忘了附带乐谱简介！

Just mail your sheet file to [beamiscool@qq.com](mailto:beamiscool@qq.com) and don't forget the sheet description !

# 注意! / Attention!

### 请仔细阅读以下使用须知！<br>Before using this script, you must read the following notes!

1. 未充分测试，若遇到BUG，请酷安私信@StageGuard或新建Issue来反馈BUG！<br>

   SkyAutoplayerScript is not fully tested, if you meet a bug, please PM CoolApk@StageGuard or create a new issue.

2. **SkyAutoplayerScript是完全免费且开源的软件/脚本([https://github.com/StageGuard/SkyAutoPlayerScript](https://github.com/StageGuard/SkyAutoPlayerScript))，使用 SkyAutoplayerScript 盈利的同时请标注源项目链接。**<br>

   **SkyAutoplayerScript is a free and open source project. If you want to use it for commercial purposes, please add the link refers to this repository.**

3. **共享乐谱不遵守LGPL-2.1协议，如您想在SkyAutoPlayer以外使用这些乐谱，请自行找乐谱作者授权！**<br>

   **Shared sheets is not under the protection of LGPL-2.1, if you want to reprint shared sheets to other platforms, please contact sheet transcriber by yourself!**

4. 本脚本仅可用作娱乐用途，请不要在正规场合使用本脚本(请自行体会\"正规场合\"是什么意思)，若因使用本脚本所出现了一些不友好的问题，与脚本作者StageGuard无关。<br>

   The script is just for entertainment and it is not suitable to use it in formal occasion.

5. 脚本只能给你一时满足感而不能使你进步，请适当使用，只有真正的技术才是王道，才能使你感到快乐。<br>

   Script just gives you a short-time sense of satisfaction and do not make you progress.

6. 本脚本只是一个"弹奏机"，并不内置曲谱，请在 GooglePlay 下载 [SkyStudio](https://play.google.com/store/apps/details?id=com.Maple.SkyStudio) 编谱。<br>

   This script is just a "player" and no built-in sheets, you must download [SkyStudio](https://play.google.com/store/apps/details?id=com.Maple.SkyStudio) to make sheets.

7. 本脚本不会增加解密乐谱功能，包括但不限于**加密的SkyStudio乐谱**，**加密的JS**等，也不接受加密乐谱的共享。<br>

   The script will not support decrypt sheet, including **SkyStudio encryped sheet** or **encrypted js** etc...<br>And the repository will also not accept encrypted sheet.


<details> <summary>针对上述第2, 3条出现的问题：</summary>

# 关于脚本倒卖的问题: [#1](https://github.com/StageGuard/SkyAutoPlayerScript/issues/1)

# 耻辱柱

Gitee 用户[嗨游圈(@vipssp)](https://gitee.com/vipssp)在**未经乐谱上传者的同意下私自盗用** SkyAutoplayerScript 于 Gitee 的[同步镜像](https://gitee.com/stageguard/SkyAutoPlayerScript)到它的 [Gitee 仓库](https://gitee.com/vipssp/SkyAutoPlayerScript/)。

在经过[通知](https://gitee.com/vipssp/SkyAutoPlayerScript/commit/197925a71ff9cc6248be682a55406fc5814b12d7#note_3637784)后仍未及时删除盗用的乐谱，在于一些乐谱原作者沟通后，决定将其挂在此 README.md 首部，以告示。

<table>
<tr>
    <td align="center" height="200">
        <img src="https://gitee.com/stageguard/SkyAutoPlayerScript/raw/master/resources/static/2020-12-19_0-8-40.PNG" />
    </td>
    <td align="center" height="200">
        <img src="https://gitee.com/stageguard/SkyAutoPlayerScript/raw/master/resources/static/2020-12-19_0-9-57.PNG" />
    </td>
    <td align="center" height="200">
        <img src="https://gitee.com/stageguard/SkyAutoPlayerScript/raw/master/resources/static/2020-12-19_0-44-4.PNG" />
    </td>
    <td align="center" height="200">
        <img src="https://gitee.com/stageguard/SkyAutoPlayerScript/raw/master/resources/static/Screenshot_2020-12-19-00-10-12-499_com.coolapk.market.jpg" />
    </td>
    <td align="center" height="200">
        <img src="https://gitee.com/stageguard/SkyAutoPlayerScript/raw/master/resources/static/Screenshot_2020-12-19-00-11-23-671_com.coolapk.market.jpg" />
    </td>
</tr>
</table>
</details>

# 贡献 / Contribution

欢迎任何人贡献本项目，包括但不限于 Pull Request，Issue，New feature request 或者 贡献翻译。

Welcome everyone to contribute this project, including pull request, issue, new feature request or translation.

## 贡献者 / Contributor

### SkyAutoPlayerScript

[@tiaod](https://github.com/tiaod)

### 共享乐谱 / Shared sheets

酷安[@Aex技术总监](http://www.coolapk.com/u/1286879)<br>
酷安[@夏卡卡卡](http://www.coolapk.com/u/2313452)<br>
酷安[@深空失忆か](http://www.coolapk.com/u/3005974)<br>
抖音[@子哲啊🌈(zizhe1880689503)](https://v.douyin.com/J9gUaVE/)<br>
酷安[@你们很有趣呢](http://www.coolapk.com/u/2416229)<br>
酷安[@情如风雪无常](http://www.coolapk.com/u/643670)<br>
酷安[@慕疵](http://www.coolapk.com/u/3286967)<br>
酷安[@社区最弱萌新](http://www.coolapk.com/u/3291313)<br>
酷安[@九方辰](http://www.coolapk.com/u/634078)<br>
酷安[@北极马可罗尼](http://www.coolapk.com/u/463478)<br>
哔哩哔哩[@UTF16](https://space.bilibili.com/623364258)<br>
酷安[@Syngenex](http://www.coolapk.com/u/1093421)<br>
Twitter[Phoebe@huunhut1217](https://mobile.twitter.com/huunhut1217)<br>
酷安[@终究是错付了](http://www.coolapk.com/u/2293899)<br>
酷安[@DesperatU](http://www.coolapk.com/u/1075889)<br>
酷安[@明明酱](http://www.coolapk.com/u/1706128)<br>
酷安[@cxk的篮球](http://www.coolapk.com/u/1090769)<br>
酷安[@头条乀](http://www.coolapk.com/u/1192320)<br>
酷安[@Alusias](http://www.coolapk.com/u/808787)<br>
[chikin](mailto:2869826936@qq.com)<br>
酷安[@温茶予君](http://www.coolapk.com/u/1212499)<br>
酷安[@落红难相聚](http://www.coolapk.com/u/2082465)<br>

## 翻译 / Translation

SkyAutoplayerScript 在版本 21 已支持多语言并可以在线获取语言列表，你可以查看 [contribute-translation.md](contribute-translation.md) (English) 来了解如何贡献翻译。

SkyAutoplayerScript version 21 has supported multi-language and can also fetch online language list, follow [contribute-translation.md](contribute-translation.md) (English) guide to contribute translation.

### 贡献者 / Contributor

无

# 图标来源 / Icon from:

[Iconfont-阿里巴巴矢量图标库](https://www.iconfont.cn/)

# 鸣谢 / Thanks to:

[projectXero](https://gitee.com/projectXero) (提供适用于Rhino的`ListAdapter`)

# 许可证协议 / LICENSE

```
    SkyAutoPlayer (Auto.js script)
	  Copyright © 2020-2021 StageGuard

  This library is free software; you can redistribute it and/or
  modify it under the terms of the GNU Lesser General Public
  License as published by the Free Software Foundation; either
  version 2.1 of the License, or (at your option) any later version.

  This library is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
  Lesser General Public License for more details.

  You should have received a copy of the GNU Lesser General Public
  License along with this library; if not, write to the Free Software
  Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301
  USA
```
