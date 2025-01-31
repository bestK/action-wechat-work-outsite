# WeChat Work for GitHub Actions

通过企业微信机器人发送消息。

![WeChat Work Logo](./docs/wechat-work-logo.png "WeChat Work Logo")

<hr/>

## 用法

### 纯文本格式消息

![WeChat Work message](./docs/wechat-work-msg-text.png "WeChat Work message")

```yaml
- name: WeChat Work notification by text
  uses: chf007/action-wechat-work@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    msgtype: text
    content: "广州今日天气：29 度，大部分多云，降雨概率：60%"
    mentioned_list: '["wangqing","@all"]'
    mentioned_mobile_list: '["13800001111","@all"]'
```
#### 外部群
```yaml

- name: WeChat Work outside group notification by text
  uses: bestk/action-wechat-work-outsite@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    outside_group: true
    outside_msgtype: text
    outside_received_content: "外部群消息"
    outside_at_list: '["昵称x","昵称y"]'
    outside_title_list: '["XXX开发者通知群"]'

```

| 参数                  | 必须  | 说明                                                                                                                                                      |
| --------------------- | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| msgtype               | true  | 消息类型，此时固定为 text                                                                                                                                 |
| content               | true  | 文本内容，最长不超过 2048 个字节，必须是 utf8 编码                                                                                                        |
| mentioned_list        | false | userid 的列表，提醒群中的指定成员(@某个成员)，@all表示提醒所有人，如果开发者获取不到 userid，可以使用 mentioned_mobile_list，必须是序列化后的 JSON 字符串 |
| mentioned_mobile_list | false | 手机号列表，提醒手机号对应的群成员(@某个成员)，@all表示提醒所有人，必须是序列化后的 JSON 字符串                                                           |

### Markdown 格式消息

![WeChat Work message](./docs/wechat-work-msg-markdown.png "WeChat Work message")

```yaml
- name: WeChat Work notification by markdown
  uses: chf007/action-wechat-work@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    msgtype: markdown
    content: "实时新增用户反馈<font color=\"warning\">132例</font>，请相关同事注意。\n
    > 类型:<font color=\"comment\">用户反馈</font> \n
    > 普通用户反馈:<font color=\"comment\">117例</font> \n
    > VIP用户反馈:<font color=\"comment\">15例</font>"
```

| 参数    | 必须 | 说明                                                                                                                        |
| ------- | ---- | --------------------------------------------------------------------------------------------------------------------------- |
| msgtype | true | 消息类型，此时固定为 markdown                                                                                               |
| content | true | markdown 内容，最长不超过 4096 个字节，必须是 utf8 编码 支持的格式详见 https://work.weixin.qq.com/api/doc/90000/90136/91770 |

### 图片格式消息

![WeChat Work message](./docs/wechat-work-msg-image.png "WeChat Work message")

```yaml
- name: WeChat Work notification by image
  uses: chf007/action-wechat-work@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    msgtype: image
    base64: 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
    md5: 0582d8564cdee3187207666898f75205
```

| 参数    | 必须 | 说明                                                                                 |
| ------- | ---- | ------------------------------------------------------------------------------------ |
| msgtype | true | 消息类型，此时固定为 image                                                           |
| base64  | true | 图片内容的 base64 编码 注：图片（base64 编码前）最大不能超过 2 M，支持 JPG、PNG 格式 |
| md5     | true | 图片内容（base64 编码前）的 md5 值                                                   |

### 图文格式消息

![WeChat Work message](./docs/wechat-work-msg-news.png "WeChat Work message")

```yaml
- name: WeChat Work notification by news
  uses: chf007/action-wechat-work@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    msgtype: news
    articles: '[{"title":"中秋节礼品领取","description":"今年中秋节公司有豪礼相送","url":"URL","picurl":"https://res.mail.qq.com/node/ww/wwopenmng/images/independent/doc/test_pic_msg1.png"}]'
```
| 参数                 | 必须  | 说明                                                                              |
| -------------------- | ----- | --------------------------------------------------------------------------------- |
| msgtype              | true  | 消息类型，此时固定为 news                                                         |
| articles             | true  | 图文消息，一个图文消息支持 1 到 8 条图文，必须是序列化后的 JSON 字符串            |
| articles.title       | true  | 标题，不超过 128 个字节，超过会自动截断                                           |
| articles.description | false | 描述，不超过 512 个字节，超过会自动截断                                           |
| articles.url         | false | 点击后跳转的链接。                                                                |
| articles.picurl      | false | 图文消息的图片链接，支持 JPG、PNG 格式，较好的效果为大图 1068*455，小图 150*150。 |

### 文件格式消息

![WeChat Work message](./docs/wechat-work-msg-file.png "WeChat Work message")

```yaml
- name: WeChat Work notification by file
  uses: chf007/action-wechat-work@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    msgtype: file
    media_id: 3a8asd892asd8asd
```

| 参数     | 必须 | 说明                                                                                    |
| -------- | ---- | --------------------------------------------------------------------------------------- |
| msgtype  | true | 消息类型，此时固定为 file                                                               |
| media_id | true | 文件 id，通过 [文件上传接口](https://work.weixin.qq.com/api/doc/90000/90136/91770) 获取 |

### 模版卡片类型消息 - 文本通知模版卡片

![WeChat Work message](./docs/wechat-work-msg-text-notice.webp "WeChat Work message")

```yaml
- name: WeChat Work notification by template_card.text_notice
  uses: chf007/action-wechat-work@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    msgtype: template_card
    template_card: '{"card_type":"text_notice","source":{"icon_url":"https://wework.qpic.cn/wwpic/252813_jOfDHtcISzuodLa_1629280209/0","desc":"企业微信","desc_color":0},"main_title":{"title":"欢迎使用企业微信","desc":"您的好友正在邀请您加入企业微信"},"emphasis_content":{"title":"100","desc":"数据含义"},"quote_area":{"type":1,"url":"https://work.weixin.qq.com/?from=openApi","appid":"APPID","pagepath":"PAGEPATH","title":"引用文本标题","quote_text":"Jack：企业微信真的很好用~\nBalian：超级好的一款软件！"},"sub_title_text":"下载企业微信还能抢红包！","horizontal_content_list":[{"keyname":"邀请人","value":"张三"},{"keyname":"企微官网","value":"点击访问","type":1,"url":"https://work.weixin.qq.com/?from=openApi"},{"keyname":"企微下载","value":"企业微信.apk","type":2,"media_id":"MEDIAID"}],"jump_list":[{"type":1,"url":"https://work.weixin.qq.com/?from=openApi","title":"企业微信官网"},{"type":2,"appid":"APPID","pagepath":"PAGEPATH","title":"跳转小程序"}],"card_action":{"type":1,"url":"https://work.weixin.qq.com/?from=openApi","appid":"APPID","pagepath":"PAGEPATH"}}'
```

| 参数          | 必须 | 说明                                                                                                                                                                 |
| ------------- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| msgtype       | true | 消息类型，此时固定为 template_card                                                                                                                                   |
| template_card | true | 模版卡片参数，必须是序列化后的 JSON 字符串，参数详见 https://developer.work.weixin.qq.com/document/path/91770#%E6%A8%A1%E7%89%88%E5%8D%A1%E7%89%87%E7%B1%BB%E5%9E%8B |

### 模版卡片类型消息 - 图文展示模版卡片

![WeChat Work message](./docs/wechat-work-msg-news-notice.webp "WeChat Work message")

```yaml
- name: WeChat Work notification by template_card.news_notice
  uses: chf007/action-wechat-work@master
  env:
    WECHAT_WORK_BOT_WEBHOOK: ${{secrets.WECHAT_WORK_BOT_WEBHOOK}}
  with:
    msgtype: template_card
    template_card: '{"card_type":"news_notice","source":{"icon_url":"https://wework.qpic.cn/wwpic/252813_jOfDHtcISzuodLa_1629280209/0","desc":"企业微信","desc_color":0},"main_title":{"title":"欢迎使用企业微信","desc":"您的好友正在邀请您加入企业微信"},"card_image":{"url":"https://wework.qpic.cn/wwpic/354393_4zpkKXd7SrGMvfg_1629280616/0","aspect_ratio":2.25},"image_text_area":{"type":1,"url":"https://work.weixin.qq.com","title":"欢迎使用企业微信","desc":"您的好友正在邀请您加入企业微信","image_url":"https://wework.qpic.cn/wwpic/354393_4zpkKXd7SrGMvfg_1629280616/0"},"quote_area":{"type":1,"url":"https://work.weixin.qq.com/?from=openApi","appid":"APPID","pagepath":"PAGEPATH","title":"引用文本标题","quote_text":"Jack：企业微信真的很好用~\nBalian：超级好的一款软件！"},"vertical_content_list":[{"title":"惊喜红包等你来拿","desc":"下载企业微信还能抢红包！"}],"horizontal_content_list":[{"keyname":"邀请人","value":"张三"},{"keyname":"企微官网","value":"点击访问","type":1,"url":"https://work.weixin.qq.com/?from=openApi"},{"keyname":"企微下载","value":"企业微信.apk","type":2,"media_id":"MEDIAID"}],"jump_list":[{"type":1,"url":"https://work.weixin.qq.com/?from=openApi","title":"企业微信官网"},{"type":2,"appid":"APPID","pagepath":"PAGEPATH","title":"跳转小程序"}],"card_action":{"type":1,"url":"https://work.weixin.qq.com/?from=openApi","appid":"APPID","pagepath":"PAGEPATH"}}'

```

| 参数          | 必须 | 说明                                                                                                                                                                 |
| ------------- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| msgtype       | true | 消息类型，此时固定为 template_card                                                                                                                                   |
| template_card | true | 模版卡片参数，必须是序列化后的 JSON 字符串，参数详见 https://developer.work.weixin.qq.com/document/path/91770#%E6%A8%A1%E7%89%88%E5%8D%A1%E7%89%87%E7%B1%BB%E5%9E%8B |

### WECHAT_WORK_BOT_WEBHOOK

* **`WECHAT_WORK_BOT_WEBHOOK`**: 企业微信 Webhook URL (**required**)
* 在 GitHub 仓库的 Settings - Secrets 中设置

### 更多企业微信机器人详细说明，参见 [https://work.weixin.qq.com/api/doc/90000/90136/91770](https://work.weixin.qq.com/api/doc/90000/90136/91770)
