豆瓣FM网页版API

## 登陆接口

### 获取验证码编号

* URL: `http://douban.fm/j/new_captcha`
* Method: `GET`
* Arguments: None
* Header: None
* Response(`application/json; charset=utf-8`)

```
csnOQgLL24gsst7QGNb6mR3a:en
```

### 获取验证码图片

* URL: `http://douban.fm/misc/captcha`
* Method: `GET`
* Arguments:
    * size: `m`
    * id : `csnOQgLL24gsst7QGNb6mR3a:en`
* Header: None
* Response(`application/json; charset=utf-8`)

### 登陆

这里主要获取cookie以供登陆使用

* URL: `http://douban.fm/j/login`
* Method: `POST`
* Arguments:
    * source: `radio`
    * alias: `********@qq.com`
    * form_password: `******`
    * captcha_solution: `memory`
    * captcha_id: 'csnOQgLL24gsst7QGNb6mR3a:en'
    * task: `sync_channel_list`
* Header: None
* Response(`application/json; charset=utf-8`)

```
{
  "user_info": {
    "ck": "-VQY",
    "play_record": {
      "fav_chls_count": 4,
      "liked": 802,
      "banned": 162,
      "played": 28368
    },
    "is_new_user": 0,
    "uid": "taizilongxu",
    "third_party_info": null,
    "url": "http://www.douban.com/people/taizilongxu/",
    "is_dj": false,
    "id": "2053207",
    "is_pro": false,
    "name": "刘小备"
  },
  "r": 0
}
```

## 频道获取

固定频道

* 私人兆赫: 0
* 红心兆赫: -3
* 豆瓣精选兆赫: -10

## 切换频道

* URL: `http://douban.fm/j/change_channel`
* Method: `GET`
* Arguments:
    * fcid: `0`
    * tcid: `-10`
    * area: `system_chis`
* Header: None
* Response(`application/json; charset=utf-8`)

```
{"r":"0"}
```

## 歌曲控制

### type参数

|类型|需要参数|含义|
|:---|:---|:---|
|n||新歌单, 返回新歌曲|
|b|sid|不在播放, 返回新歌曲|
|s|sid|下一首歌曲, 返回新歌曲|
|r|sid|标记喜欢歌曲, 返回新歌曲|
|u|sid|取消喜欢歌曲, 返回新歌曲|
|e|sid|歌曲结束标记, 返回{"r":0}|
|p|sid|播放结束时, 返回新歌曲, 这个歌曲作为下一首|

### 网页版功能使用逻辑

1. 第一次播放: `切换频道` -> `n` -> `p`
2. 下一首: `s` -> `p`
3. 不在播放: `b` -> `p`
4. 标记喜欢: `r`(这里会返回歌曲作为下一首歌曲)
5. 取消标记喜欢: `u`(和上面一样)
6. 自动连续播放下一首: `e` -> `p`

### 参数示例

* URL: `http://douban.fm/j/mine/playlist`
* Method: `GET`
* Arguments:
    * type: `n` 下一首
    * pt: `3.1` 不明
    * channel: `-10` 频道
    * pb: `128` 码率
    * from: `mainsite`
    * r: `***` 不明
* Header: None
* Response(`application/json; charset=utf-8`)

```
{
  "r": 0,
  "is_show_quick_start": 0,
  "song": [
    {
      "status": 0,
      "picture": "http://img3.douban.com/lpic/s11164525.jpg",
      "alert_msg": "",
      "albumtitle": "永远的永远",
      "promo": {},
      "singers": [
        {
          "related_site_id": 0,
          "is_site_artist": false,
          "id": "24874",
          "name": "关喆"
        }
      ],
      "file_ext": "mp4",
      "like": 1,
      "album": "/subject/3923451/",
      "ver": 0,
      "ssid": "e595",
      "title": "想你的夜",
      "url": "http://mr7.doubanio.com/e57b3e49f182face15ac1e877ce65990/1/fm/song/p1534519_128k.mp4",
      "artist": "关喆",
      "subtype": "",
      "length": 266,
      "sid": "1534519",
      "aid": "3923451",
      "sha256": "92832f6c49fba44cc29209b44a8fc4353549d6e623d3790566e52e3cbb0b0554",
      "kbps": "128"
    }
  ]
}
```

