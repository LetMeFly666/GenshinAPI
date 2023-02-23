<!--
 * @Author: LetMeFly
 * @Date: 2023-02-23 20:27:07
 * @LastEditors: LetMeFly
 * @LastEditTime: 2023-02-23 21:41:44
-->
# GenshinAPI

Genshin API list | 原神API列表

## Preface | 前言

Here will list some APIs of Genshin Impact and their descriptions | 这里将会列举一些原神的API及其说明

There are two reasons creating this project: | 写这个项目有两个原因：

1. There are many ready-made tools (such as card drawing analysis tools) that use Genshin Impact API for assistance, but there are few detailed API descriptions (at least I haven't found them). In this way, if I want to customize some similar functions, it is very troublesome. I must either start from scratch and analyze the package, or read the code of the existing project for analysis. | 现成的使用原神API进行辅助的工具很多（例如抽卡分析工具），但是有详细的API说明的很少（至少我没有找到）。这样我想要自定义一些类似的功能就很麻烦，要么从头开始抓包分析，要么阅读现有项目的代码进行分析。
2. No "packaged wheels" can be found for direct secondary development. | 暂未发现“打包好的轮子”可供直接二次开发使用。

This project is dedicated to providing some Genshin Impact APIs and their descriptions, so that new developers no longer need to analyze Genshin Impact APIs from scratch. | 这个项目致力于提供一些原神API及其说明，这样新的开发者就不再需要从头开始分析原神API了。

At the same time, this project will also provide some wheels of functions implemented through these APIs. If necessary, developers can directly call the packaged wheels in this project without further development. | 同时这个项目也会提供一些通过这些API实现的功能的轮子，如有需要，开发者可以直接调用本项目中打包好的轮子而无需再二次开发。

## API list | API列表

### Genshin Impact Prayer Record Access API | 原神祈愿记录获取API

This API can obtain all the praying records of a character within 6 months. | 本API可以获取某角色6个月内的所有祈愿记录。

**URL：**

```
https://hk4e-api.mihoyo.com/event/gacha_info/api/getGachaLog?authkey_ver=1&lang={lang}&authkey={authkey}&gacha_type={gacha_type}&page={page}&size={size}
```

**Request Method | 请求方式：** HTTP GET

**Parameter Description | 参数说明：**

|Param|Description|Example|
|:--:|:--:|:--:|
|lang|The language returns &#124; 返回的语言|zh-cn|
|authkey|Got from Game and so on &#124; 获取自游戏等|U696NGKpeKBPZP..|
|gacha_type|GacheType &#124; 抽卡类型|100|
|page|Page num &#124; 获取第几页的记录|1|
|size|Page size &#124; 每页记录多少条|20|

Among that: | 其中：

gacha_type:

|value|mean|
|:--:|:--:|
|100|新手祈愿|
|200|常驻祈愿|
|301|角色活动祈愿与角色活动祈愿-2|
|302|302|

size：范围0到20，至多一次获取20条

authkey：可以在游戏内浏览“祈愿->祈愿记录”时抓包获取，也可以使用别的开发者提供的方法：[胡桃工具箱](https://hut.ao/advance/Gacha-system-and-export-principal.html)、[非小酋](https://feixiaoqiu.com/rank_url_upload_init/)、[提瓦特小助手](https://mp.weixin.qq.com/s/MUA_wvb-Q-5fR_DIYO9zAA)

**For example: | 例如：**

```
https://hk4e-api.mihoyo.com/event/gacha_info/api/getGachaLog?authkey_ver=1&lang=zh-cn&authkey=U696NGKpeKBPZP&gacha_type=100&page=1&size=3
```

代表：

```
查询新手池的祈愿记录（gacha_type=100），每页获取3条（size=3），用简体中文（lang=zh-cn）返回第1页（page=1）的记录
```

**Return Type: | 返回类型：**

json格式的数据

```json
{
    "retcode": 0,
    "message": "OK",
    "data": {
        "page": "这是第几页抽卡记录",
        "size": "本页有多少条抽卡记录",
        "total": "0",
        "list": [
            抽卡记录1,
            抽卡记录2,
            ...
        ],
        "region": "数据来源服务器"
    }
}
```

其中**抽卡记录**的格式为：

```json
{
    "uid": "原神uid",
    "gacha_type": "抽卡类型",
    "item_id": "",
    "count": "1",
    "time": "抽卡时间",
    "name": "武器或角色名称",
    "lang": "语言类型",
    "item_type": "武器还是角色",
    "rank_type": "几星",
    "id": "本次祈愿id(时间戳+本时间第几次抽卡)"
}
```

**例如：**

```json
{
    "retcode": 0,
    "message": "OK",
    "data": {
        "page": "1",
        "size": "3",
        "total": "0",
        "list": [
            {
                "uid": "257224768",
                "gacha_type": "400",
                "item_id": "",
                "count": "1",
                "time": "2023-02-23 11:22:25",
                "name": "沐浴龙血的剑",
                "lang": "zh-cn",
                "item_type": "武器",
                "rank_type": "3",
                "id": "1677121560000603768"
            },
            {
                "uid": "257224768",
                "gacha_type": "400",
                "item_id": "",
                "count": "1",
                "time": "2023-02-21 15:40:07",
                "name": "沐浴龙血的剑",
                "lang": "zh-cn",
                "item_type": "武器",
                "rank_type": "3",
                "id": "1676963160001011068"
            },
            {
                "uid": "257224768",
                "gacha_type": "400",
                "item_id": "",
                "count": "1",
                "time": "2023-02-21 15:39:57",
                "name": "沐浴龙血的剑",
                "lang": "zh-cn",
                "item_type": "武器",
                "rank_type": "3",
                "id": "1676963160001006168"
            }
        ],
        "region": "cn_gf01"
    }
}
```