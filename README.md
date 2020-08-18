# 关于脚本倒卖的问题: [#1](https://github.com/StageGuard/SkyAutoPlayerScript/issues/1)

# SkyAutoPlayerScript
A script to play Sheets generated by SkyStudio automatically in game Sky with accessibility services using Auto.js
</br>使用Auto.js提供的无障碍权限实现在Sky光遇中自动弹奏[SkyStudio](https://play.google.com/store/apps/details?id=com.Maple.SkyStudio)导出的曲谱

## 特性
相比于其他脚本，SkyAutoPlayerScript拥有以下优势

* 全GUI操作，无需编辑任何代码，流畅的UI动画。
* 完整的弹奏控制面板，支持**暂停**， **进度控制**， **倍速控制**等
* 自设定键位坐标，避免按压琴键的偏移问题
* 在线[共享乐谱](https://github.com/StageGuard/SkyAutoPlayerScript/tree/master/shared_sheets)，有许多优质乐谱。</br>~~(甚至有的乐谱复杂到根本无法手弹)~~
* 自动更新，及时修复BUG，无需担心版本过时问题。
* ...

## 使用
①为Auto.js开启**无障碍服务**和**悬浮窗权限**。
</br>②在Auto.js中新建一个脚本并粘贴以下代码并运行：
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

在Auto.js版本`4.1.1 Alpha2 (461)`中测试通过，不保证其他版本的兼容性(取决于其他版本相对于此版本的API是否有`breaking changes`)
</br>Auto.js`4.1.1 Alpha2 (461)`版本下载: [`Ericwyn/Auto.js/releases@V4.1.1.Alpha2`](https://github.com/Ericwyn/Auto.js/releases/tag/V4.1.1.Alpha2)
</br>手机请下载`autoJs-V4.1.1.Alpha2-common-armeabi-v7a-debug.apk`

### 清除数据
SkyAutoPlayer在使用过程中会产生本地数据存储，若想全部删除，请使用Auto.js执行以下代码
```
storages.remove("StageGuard:SkyAutoPlayer:Config");
files.removeDir("/storage/emulated/0/Documents/SkyAutoPlayer/");
```

## 上传乐谱

你可以fork本仓库，将你要上传的乐谱添加至`shared_sheets`文件夹，并按照以下要求在`shared_sheets.json`添加项目
```javascript
{
    //乐谱名称
    "name": "SheetName",
    //乐谱文件名(于shared_sheets文件夹中)
    "file": "SheetName.txt",
    //你的id
    "author": "Author",
    //乐谱简介
    "desc": "This is a description about my sheet",
    //乐谱BPM
    "bpm" : 320,
    //暂时没用
    "suggested_instrument": 1,
    //乐谱音高
    "pitchLevel": 0
  }
```

修改完成后，申请`Pull Request`，等待merge即可。
> 请注意：在申请`Pull Request`之前请确保你的SkyAutoPlayerScript仓库已同步至最新，以免出现意外问题！


## 注意
### 请仔细阅读以下使用须知！

1. 未充分测试，若遇到BUG，请酷安私信@StageGuard或新建Issue来反馈BUG！
2. **SkyAutoPlayer是完全免费且开源的软件/脚本([https://github.com/StageGuard/SkyAutoPlayerScript](https://github.com/StageGuard/SkyAutoPlayerScript))，禁止使用本脚本作为盈利用途！**
3. 本脚本仅可用作娱乐用途，请不要在正规场合使用本脚本(请自行体会\"正规场合\"是什么意思)，若因使用本脚本所出现了一些不友好的问题，与脚本作者StageGuard无关。
4. 脚本只能给你一时满足感而不能使你进步，请适当使用，只有真正的技术才是王道，才能使你感到快乐。
5. 本脚本只是一个"弹奏机"，并不内置曲谱，请在GooglePlay下载[SkyStudio](https://play.google.com/store/apps/details?id=com.Maple.SkyStudio)编谱。

## 贡献
欢迎任何人贡献本项目，包括但不限于Pull Request，Issue，New feature request

## 图标来源
[Iconfont-阿里巴巴矢量图标库](https://www.iconfont.cn/)

## 鸣谢
[projectXero](https://gitee.com/projectXero) (提供适用于Rhino的`ListAdapter`)

## 许可证协议
```
	SkyAutoPlayer (Auto.js script)
	Copyright © 2020 StageGuard
	
    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <https://www.gnu.org/licenses/>
```