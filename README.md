# marker
a local-storage(php based) javascript api marking-off plugin.

#### 功能简介

- 多人划线标记（多评论系统共用）
- 划线文本引用、复制、注释（他人非注释标记可点赞）
- 用户本地浏览器 + 远程服务端双校验
- 本地储存（localStorage+Cookie）验证记录
- 大量自定义初始化参数（文本、元素、class..）
- 可配置生效选区、选区内禁止选中黑名单
- 可配置最大标记数量、最小选中字符长度
- 可配置标记颜色（可选渐变、角度）、标记粗细
- 可配置标记（保存、校验）时效
- 可配置开启或关闭标记动画、标记功能组件

![marker](https://raw.githubusercontent.com/2Broear/marker/main/marker2.gif "marker.gif")

## 使用说明
可 _手动_ 加载 `marker.js` 脚本后初始化，也可异步 `xhr` 加载完成初始化：

```javascript
new marker.init();
```

### 初始化参数（可选）
初始化时，可携带部分对象参数以重载默认配置，常用配置项如下列表所示：

#### static-> 静态参数

| 参数 | 类型 | 描述 | 备注 |
| :---- | :---- | :---- | :---- |
| md5Url | String | 初始化 md5 依赖文件路径 | 默认 `/md5.js` |
| apiUrl | String | 后端 mark 依赖文件路径 | 默认 `/mark.php` |
| avatar | String | 标记用户 gravatar 头像镜像源 | 默认 `//cravatar.com/` |
| postId | String | 标记文章唯一标识符 | 默认 `window.location.pathname` |
|  |  |  |  |
| likeMax | Number | 多用户标记头像最大数量 | 默认 `2` |
| dataMax | Number | 最大标记数量 | 默认 `3` |
| dataMin | Number | 最小标记文本字符长度 | 默认 `2` |
| dataDelay | Number | 模拟标记延迟 | 默认 `500` |
| dataAlive | Number | 标记（本地校验数据）储存时效 | 默认 `365` |
| dataCount | Number | 用户远程标记数量（页面刷新时自动初始化） | 默认 `0` |
| dataPrefix | String | 本地储存（cookie）前缀 | 默认 `marker-` |
| dataCaches | String | 本地储存（localStorage）前缀 | 默认 `markerCaches` |
|  |  |  |  |
| lineColor | String | 标记颜色（基本） | 默认 `orange` |
| lineColors | String | 标记颜色（渐变，可选 transparent） | 默认 `red` |
| lineDegress | Number | 标记颜色角度（渐变，范围 0-360） | 默认 `0` |
| lineBold | Number | 标记线粗（基本） | 默认 `15` |
| lineBoldMax | Number | 标记线粗（悬浮） | 默认 `30` |
| lineAnimate | Boolean | 标记划线动画 | 默认 `true` |
| lineKeepTop | Boolean | 始终显示标记 | 默认 `false` |
|  |  |  |  |
| useNote | Boolean | 开启注释功能 | 默认 `true` |
| useCopy | Boolean | 开启复制功能 | 默认 `true` |
| useQuote | Boolean | 开启引用功能 | 默认 `true` |
|  |  |  |  |
| ctxMark | String | 标记文本 | 默认 `标记` |
| ctxMarking | String | 标记中文本 | 默认 `标记中` |
| ctxMarked | String | 已标记文本 | 默认 `已标记` |
| ctxMarkMax | String | 标记满文本 | 默认 `用户标记已满` |
| ctxNote | String | 注释文本 | 默认 `注释` |
| ctxNoted | String | 已注释文本 | 默认 `已注释` |
| ctxCopy | String | 标记复制文本 | 默认 `复制` |
| ctxCopied | String | 标记已复制文本 | 默认 `已复制` |
| ctxQuote | String | 标记引用文本 | 默认 `引用` |
| ctxQuoted | String | 标记已引用文本 | 默认 `已引用` |
| ctxLike | String | 标记点赞文本 | 默认 `+1` |
| ctxCancel | String | 取消标记文本 | 默认 `取消选中/删除` |

#### class-> 静态参数

| 参数 | 类型 | 描述 | 备注 |
| :---- | :---- | :---- | :---- |
| line | String | 元素：标记元素 | 默认 `markable` |
| tool | String | 元素：标记功能元素 | 默认 `tools` |
| toolIn | String | 元素：同上 | 默认 `toolInside` |
| mark | String | 元素：标记划线元素 | 默认 `mark` |
| done | String | 状态：标记完成 | 默认 `done` |
| note | String | 元素：标记注释元素 | 默认 `note` |
| copy | String | 元素：标记复制元素 | 默认 `copy` |
| quote | String | 元素：标记引用元素 | 默认 `quote` |
| like | String | 元素：标记点赞元素 | 默认 `like` |
| update | String | 元素：标记更新元素 | 默认 `update` |
| close | String | 元素：标记删除元素 | 默认 `close` |
| disabled | String | 状态：禁用标记 | 默认 `disabled` |
| underline | String | 状态：标记动画 | 默认 `underline` |
| processing | String | 状态：标记执行 | 默认 `processing` |
| blackList | Array | 禁止选中 class 元素列表 | 默认 `['markable','wp-block-quote','wp-block-code','wp-block-table','wp-element-caption']` |

#### element-> 元素参数
| 参数 | 类型 | 描述 | 备注 |
| :---- | :---- | :---- | :---- |
| effectsArea | HTMLElement | 指定可选元素范围 | 缺省 `document` |
| commentArea | HTMLElement | 评论框（textarea） | 缺省 `null` |
| commentInfo -> userNick | HTMLElement | 昵称框（input） | 缺省 `null` |
| commentInfo -> userMail | HTMLElement | 邮箱框（input） | 缺省 `null` |

### 增查删改（可选）
完成初始化后，可通过 `data` 接口查询（getter）或更新部分（setter）已储存数据，如下所示：

```javascript
// getter
marker.data; // all data
marker.data.nick;  // ""

// setter（nick, mail, counts, pending, promised）
marker.data = {nick: "test"};
marker.data.nick;  // "test"
```
<details>
      <summary>详细 setter 结构</summary>
  
```javascript
// setter constructure
{
    user: {
        nick: "",  // 用户昵称
        mail: "",  // 用户邮件
        mid: "",  // 用户邮件 md5
    },
    stat: {
        counts: 0,  // 用户（本地）已标记数量（动态，可写）
        pending: 0,  // 当前标记状态（1：标记中）
        promised: {},  // 当前 fetch 状态（promise）
    },
    list: {},  // 用户标记记录
    path: window.location.pathname,  // 当前标记页面（可用于 static->postID 作为标识符使用）
    _caches: "{}",  // 本地所有已标记 ts 验证记录（永久保存）
    _counts: 0,  // 用户（远程）已标记数量（静态，不可写）
}
```

</details>

#### 携带参数初始化（示例）

```javascript
// 自定义参数对象（常用）
const custom_args = {
    static: {
        dataMin: 10, // 至少选中 10 个字符标记
        dataMax: 5, // 每篇文章至多 5 个标记
        dataAlive: 30, // 每个标记至多保留 30 天
        dataDelay: 0, // 无延迟标记
        lineColor: 'red', // 红色标记
        lineColors: 'transparent', // 红色渐变到 —> 透明
        lineDegrees: 90, // 渐变角度旋转 90°
        lineBold: 10, // 使用更细线标记
        lineBoldMax: 100, // 标记线全覆盖文本（悬浮）
        lineAnimate: false, // 关闭划线动画
        useQuote: false, // 关闭引用功能
        apiUrl: "<?php echo get_bloginfo('template_directory'); ?>/plugin/mark.php", // 假设文件处于 plugin 目录）
        md5Url: "<?php echo get_bloginfo('template_directory'); ?>/js/md5.js", // 假设文件处于 js 目录）
        postId: <?php echo $post->ID; ?>, // 使用 wordpress 文章ID
    },
    class: {
        blackList: ['chatGPT','article_index','ibox'], // 不可标记元素
    },
    element: {
        effectsArea: document.querySelector('.content'), // 可标记 ".content" 元素区域
        commentArea: document.querySelector('#vcomments textarea'), // 评论框
        commentInfo: {
            userNick: document.querySelector('#vcomments input[name=nick]'), // 用户昵称框
            userMail: document.querySelector('#vcomments input[name=mail]'), // 用户邮箱框
        }
    },
}
// 初始化并重载自定义配置
new marker.init(custom_args);
```

### todo

- ❎ 可选 Baas 无后端化数据储存

## 其他
任何问题及建议可提 issue.