# 网抑云音乐接口 说明文档

## 1. 说明

hhh对于寒假考核想要写更高级的作业：音乐app 的童鞋，本次开放了网易云的音乐接口~

服务器地址为：http://sandyz.ink:3000/

只供两个月~，注意不要频繁请求服务君，以防他生气不理你了hhh，其实我也要生气 : )



## 2. 接口概览

这个接口的服务端是一个开源库~

[Binaryify/NeteaseCloudMusicApi: 网易云音乐 Node.js API service (github.com)](https://github.com/Binaryify/NeteaseCloudMusicApi)

详细可以访问上面的网址，里面附有更详细的接口使用文档



##### 歌单首页

说明 : 调用此接口 , 可获取网友精选碟歌单

**可选参数 :** `order`: 可选值为 'new' 和 'hot', 分别对应最新和最热 , 默认为 'hot'

`cat`:`cat`: tag, 比如 " 华语 "、" 古风 " 、" 欧美 "、" 流行 ", 默认为 "全部",可从歌单分类接口获取(/playlist/catlist)

`limit`: 取出歌单数量 , 默认为 50

`offset`: 偏移数量 , 用于分页 , 如 :( 评论页数 -1)*50, 其中 50 为 limit 的值

**接口地址 :** `/top/playlist`

**调用例子 :** `http://sandyz.ink:3000/top/playlist?limit=10&order=new`



##### 获取精品歌单

说明 : 调用此接口 , 可获取精品歌单

**可选参数 :** `cat`: tag, 比如 " 华语 "、" 古风 " 、" 欧美 "、" 流行 ", 默认为 "全部",可从精品歌单标签列表接口获取(`/playlist/highquality/tags`)

`limit`: 取出歌单数量 , 默认为 20

`before`: 分页参数,取上一页最后一个歌单的 `updateTime` 获取下一页数据

**接口地址 :** `/top/playlist/highquality`

**调用例子 :** `http://sandyz.ink:3000/top/playlist/highquality?before=1503639064232&limit=3`



##### 获取歌曲详情

说明 : 调用此接口 , 传入音乐 id(支持多个 id, 用 `,` 隔开), 可获得歌曲详情(注意:歌曲封面现在需要通过专辑内容接口获取)

**必选参数 :** `ids`: 音乐 id, 如 `ids=347230`

**接口地址 :** `/song/detail`

**调用例子 :** `http://sandyz.ink:3000/song/detail?ids=347230`,`/song/detail?ids=347230,347231`



##### 搜索

说明 : 调用此接口 , 传入搜索关键词可以搜索该音乐 / 专辑 / 歌手 / 歌单 / 用户 , 关键词可以多个 , 以空格隔开 , 如 " 周杰伦 搁浅 "( 不需要登录 ), 搜索获取的 mp3url 不能直接用 , 可通过 `/song/url` 接口传入歌曲 id 获取具体的播放链接

**必选参数 :** `keywords` : 关键词

**可选参数 :** `limit` : 返回数量 , 默认为 30 `offset` : 偏移数量，用于分页 , 如 : 如 :( 页数 -1)*30, 其中 30 为 limit 的值 , 默认为 0

`type`: 搜索类型；默认为 1 即单曲 , 取值意义 : 1: 单曲, 10: 专辑, 100: 歌手, 1000: 歌单, 1002: 用户, 1004: MV, 1006: 歌词, 1009: 电台, 1014: 视频, 1018:综合

**接口地址 :** `/search` 或者 `/cloudsearch`(更全)

**调用例子 :** `http://sandyz.ink:3000/search?keywords= 海阔天空` `http://sandyz.ink:3000/cloudsearch?keywords= 海阔天空`



##### 获取音乐 url

说明 : 使用歌单详情接口后 , 能得到的音乐的 id, 但不能得到的音乐 url, 调用此接口, 传入的音乐 id( 可多个 , 用逗号隔开 ), 可以获取对应的音乐的 url,未登录状态返回试听片段(返回字段包含被截取的正常歌曲的开始时间和结束时间)

> 注 : 部分用户反馈获取的 url 会 403,[hwaphon](https://github.com/hwaphon)找到的解决方案是当获取到音乐的 id 后，将 https://music.163.com/song/media/outer/url?id=id.mp3 以 src 赋予 Audio 即可播放

**必选参数 :** `id` : 音乐 id

**可选参数 :** `br`: 码率,默认设置了 999000 即最大码率,如果要 320k 则可设置为 320000,其他类推

**接口地址 :** `/song/url`

**调用例子 :** `http://sandyz.ink:3000/song/url?id=33894312` `http://sandyz.ink:3000/song/url?id=405998841,33894312`







更多接口详见：

https://binaryify.github.io/NeteaseCloudMusicApi/



## 3. 简单引导

希望新人们都能学到些知识，多些尝试，而不是抱怨道“这tm是什么玩意，无从下手呀”，我写下了这篇简单引导（会做网络请求app的就可以忽略下文）



**接下来跟我一步步走就行啦~**

接口简单理解其实就是一个返回字符串的网址~，如果访问：

```
http://sandyz.ink:3000/search?keywords=海阔天空
```

则浏览器上出来了一堆不知道是啥的东西：

```
{"result":{"songs":[{"id":347230,"name":"海阔天空","artists":[{"id":11127,"name":"Beyond","picUrl":null,"alias":[],"albumSize":0,"picId":0,"img1v1Url":"https://p2.music.126.net/6y-UleORITEDbvrOLV0Q8A=

 ... 此次省略5k字 ...
```

以我们精湛的百度技术，这可难不倒我们！

很快，你就在百度上知道这一坨东西是叫JSON的东西（就是按照一定格式排列的字符串）。

然后我们将它复制到 [JSON在线视图查看器(Online JSON Viewer) (bejson.com)](http://www.bejson.com/jsonviewernew/)

然后按一下格式化（添加空格达到提升可视化的目的），就会发现原来这厮居然有点好看！

![p1](C:\Users\lenovo\Desktop\WYYServerApi\p1.png)

那么它有什么用呢？它是传输数据的一种规范，想想java中怎么用字符串表示一个对象？有点难吧，但是用JSON就能很好地表示一个简单的对象（再看看上图想一想？）。这样就能凭借JSON，将“对象”在网络中传输啦，对象转成JSON的过程叫“序列化”，JSON还原成java对象的过程叫“反序列化”。

比如说淘宝app，会显示商品，那么商品就有很多属性比如说

![p2](C:\Users\lenovo\Desktop\WYYServerApi\p2.png)

“价格”，“商品名称”等等。那么淘宝的开发者就会创建一个类：

```java
class Good{
    float price;
    String name;
}
```

那么淘宝是怎么传输这一个对象的呢，其实就是用JSON：

```json
{
    price : 40,
    name : "现货 时间旅行..."
}
```

换句话说，接口只要返回这一串文本就行了（接口简单理解就是一个网页，只有文字嘛），淘宝app拿到这串文本，就去“反序列化”，得到Good对象，进而显示到屏幕上。

淘宝网不止有返回商品的接口，还有许许多多不同功能的接口（许许多多的网址），返回的JSON也是各式各样。

（以上只是举个例子，淘宝网不一定是这么做的）

那么，安卓怎么获得网络返回的数据呢，那就用OKHttp（一个库，Android自带，不用导入依赖）吧，或者《第一行代码》里的方式，这样就能将网址返回回来的数据，变成String的形式给你拿到。

那String怎么转换成java类呢，提示一下：用Gson（一个库，需要导入依赖，可以上网搜“Gson如何导入依赖？使用教程”）。这个时候就要发挥你的神技：忍 · 秘技 · 百度 了（[doge]逃

回到网抑云接口上，给你提供许许多多的接口，有的接口是搜索（上文那个网址就是，现在再看看是不是有点眉目了），有的接口是首页推荐曲目，有的接口是获取歌曲详细信息，这样做出一个简单的在线音乐播放器应该不是很难了吧？

而“接口”也不是一昧的给你提供数据，你还可以给他发数据（即“请求参数”），注意到上面接口的“海阔天空”了吗，“搜索”这个功能的接口，肯定需要你提供搜索关键词，它才能给你结果鸭，所以“海阔天空”就是你app中搜索框的内容，你只要：

```
http://sandyz.ink:3000/search?keywords=你要搜索的关键词
```

就能搜索对应关键词的歌曲啦。（搜索“get post请求详解”）

而播放音乐可以使用Android自带的MediaPlayer类实现。