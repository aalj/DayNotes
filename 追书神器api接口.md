# 追书神器api接口

## 排行榜

#### 排行榜目录

http://api.zhuishushenqi.com/ranking/gender

```json
{
  "female": [
    {
      "_id": "54d43437d47d13ff21cad58b",
      "title": "追书最热榜 Top100",
      "cover": "/ranking-cover/142319314350435",
      "collapse": false,
      "monthRank": "564d853484665f97662d0810",
      "totalRank": "564d85b6dd2bd1ec660ea8e2",
      "shortTitle": "最热榜"
    }
  ],
  "male": [
    {
      "_id": "54d42d92321052167dfb75e3",
      "title": "追书最热榜 Top100",
      "cover": "/ranking-cover/142319144267827",
      "collapse": false,
      "monthRank": "564d820bc319238a644fb408",
      "totalRank": "564d8494fe996c25652644d2",
      "shortTitle": "最热榜"
    }
  ],
  "picture": [
    {
      "_id": "5a322ef4fc84c2b8efaa8335",
      "title": "人气榜",
      "cover": "/ranking-cover/142319144267827",
      "collapse": false,
      "shortTitle": "人气榜"
    }
  ],
  "epub": [
    {
      "_id": "5a323096fc84c2b8efab2482",
      "title": "热搜榜",
      "cover": "/ranking-cover/142319144267827",
      "collapse": false,
      "shortTitle": "热搜榜"
    }
  ],
  "ok": true
}
```



#### 相应的排行榜内容

1. 周榜

   http://api.zhuishushenqi.com/ranking/{_id}

2. 月榜

   http://api.zhuishushenqi.com/ranking/{monthRank}

3. 总榜

   http://api.zhuishushenqi.com/ranking/{totalRank}

```json
{
  "ranking": {
    "_id": "564ef4f985ed965d0280c9c2",
    "updated": "2018-08-01T21:20:09.713Z",
    "title": "百度热搜榜",
    "tag": "baiduHotSearchRank",
    "cover": "/ranking-cover/142319217152210",
    "icon": "/cover/148945807649134",
    "__v": 970,
    "shortTitle": "百度榜",
    "created": "2018-08-02T14:21:14.387Z",
    "biTag": "false",
    "isSub": false,
    "collapse": true,
    "new": true,
    "gender": "male",
    "priority": 1050,
    "books": [
      {
        "_id": "50bef0732033d09b2f00007f",
        "author": "唐家三少",
        "cover": "/agent/http%3A%2F%2Fimg.1391.com%2Fapi%2Fv1%2Fbookcenter%2Fcover%2F1%2F41613%2F41613_4aa85589426c44098e847933cf3e2720.jpg%2F",
        "shortIntro": "将会在本周日，斗罗大陆结束的同时开始上传更新\r\n",
        "title": "斗罗大陆",
        "site": "zhuishuvip",
        "majorCate": "玄幻",
        "minorCate": "异界大陆",
        "allowMonthly": false,
        "banned": 0,
        "latelyFollower": 59554,
        "retentionRatio": "54.22"
      }
    ],
    "id": "564ef4f985ed965d0280c9c2",
    "total": 44
  },
  "ok": true
}
```

### 查看图书详情

http://api.zhuishushenqi.com/book/{_id}



### 追更新

