# marker
a local-storage(php based) javascript api marking-off plugin.

#### 功能简介

- 多人划线标记
- 用户本地+远程验证
- 划线标记、引用、复制
- 自定义初始化参数
- 本地储存记录

![marker](https://raw.githubusercontent.com/2Broear/marker/main/marker.gif "marker.gif")

## 使用说明
可 _手动_ 加载 `main.js` 脚本后完成初始化（也可在前端中使用异步 `xhr` 加载后完成初始化），默认脚本与目录中的 `md5.js` 、 `mark.php` 处于同一目录（可手动携带指定目录参数）：

```javascript
new marker.init();
```

### 初始化参数（可选）
初始化时，可携带部分对象参数以重载默认配置，常用配置项如下列表所示（`a->b` 表示对象 `a` 的子对象 `b`）：

#### static-> 静态参数

| 参数 | 类型 | 描述 | 备注 |
| :---- | :---- | :---- | :---- |
| md5Url | String | 初始化 md5 依赖文件路径 | 默认 `/md5.js` |
| apiUrl | String | 后端 mark 依赖文件路径 | 默认 `/mark.php` |
| avatar | String | 标记用户 gravatar 头像镜像源 | 默认 `//cravatar.com/` |
| postId | String | 标记文章唯一标识符 | 默认 `window.location.pathname` |
|  |  |  |  |
| dataMin | Number | 最小标记文本字符长度 | 默认 `2` |
| dataMax | Number | 最大标记数量 | 默认 `3` |
| dataDelay | Number | 模拟标记延迟 | 默认 `2` |
| dataAlive | Number | 本地验证数据储存时效 | 默认 `365` |
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
|  |  |  |  |
| ctxMark | String | 标记文本 | 默认 `标记` |
| ctxMarking | String | 标记中文本 | 默认 `标记中` |
| ctxMarked | String | 已标记文本 | 默认 `已标记` |
| ctxMarkMax | String | 标记满文本 | 默认 `用户标记已满` |
| ctxCopy | String | 标记复制文本 | 默认 `复制` |
| ctxCopied | String | 标记已复制文本 | 默认 `已复制` |
| ctxQuote | String | 标记引用文本 | 默认 `引用` |
| ctxQuoted | String | 标记已引用文本 | 默认 `已引用` |
| ctxCancel | String | 取消标记文本 | 默认 `取消选中/删除` |

#### class-> 静态参数

| 参数 | 类型 | 描述 | 备注 |
| :---- | :---- | :---- | :---- |
| line | String | 元素：标记元素 | 默认 `markable` |
| tool | String | 元素：标记功能元素 | 默认 `tools` |
| toolIn | String | 元素：同上 | 默认 `toolInside` |
| mark | String | 元素：标记划线元素 | 默认 `mark` |
| copy | String | 元素：标记复制元素 | 默认 `copy` |
| quote | String | 元素：标记引用元素 | 默认 `quote` |
| update | String | 元素：标记更新元素 | 默认 `update` |
| close | String | 元素：标记删除元素 | 默认 `close` |
| done | String | 状态：标记完成 | 默认 `done` |
| disabled | String | 状态：禁用标记 | 默认 `disabled` |
| aniProcess | String | 状态：标记执行 | 默认 `aniProcess` |
| aniUnderline | String | 状态：标记动画 | 默认 `aniUnderline` |
| blackList | String | 数组：禁止选中 class 元素列表 | 默认 `['wp-block-quote','wp-block-code','wp-block-table','wp-element-caption']` |

#### element-> 元素参数
| 参数 | 类型 | 描述 | 备注 |
| :---- | :---- | :---- | :---- |
| effectsArea | HTMLElement | 指定可选元素范围（可配合 class->blackList 使用） | 缺省 `document` |
| commentArea | HTMLElement | 评论框（textarea） | 缺省 `null` |
| commentInfo->userNick | HTMLElement | 昵称框（input） | 缺省 `null` |
| commentInfo->userMail | HTMLElement | 邮箱框（input） | 缺省 `null` |

### 增查删改（可选）
完成初始化后，可通过 `data` 接口指定以下对象参数以查询（getter）或更新（setter）当前已储存数据，如下所示：

```javascript
// getter
marker.data;
marker.data.nick;  // ""

// setter
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
        counts: 0,  // 用户（本地）已标记数量（动态）
        pending: 0,  // 当前标记状态（1：标记中）
    },
    list: {},  // 用户标记记录
    path: "",  // 当前标记页面（可用于唯一ID，默认 window.location.pathname）
    _caches: "{}",  // 本地所有已标记 ts 验证记录（永久保存）
    _counts: 0,  // 用户（服务端）已标记数量（静态，不可写）
}
```

</details>

#### 携带参数初始化示例

```javascript
// 自定义参数对象（常用）
const custom_args = {
    static: {
        dataMin: 10, // 最少选中 10 个字符触发标记
        dataMax: 5, // 每篇文章至多可标记 5 个
        dataAlive: 30, // 每个标记最多可保留 30 天
        dataDelay: 0, // 无延迟标记
        lineColor: 'red', // 红色标记
        lineColors: 'transparent', // 红色渐变到 —> 透明
        lineDegrees: 90, // 渐变角度：90
        lineBold: 10, // 使用更细线标记
        lineBoldMax: 100, // 标记线全覆盖文本（悬浮）
        lineAnimate: false, // 关闭划线动画
        apiUrl: "<?php echo get_bloginfo('template_directory'); ?>/plugin/mark.php", // 假设文件处于 plugin 目录）
        md5Url: "<?php echo get_bloginfo('template_directory'); ?>/js/md5.js", // 假设文件处于 js 目录）
        postId: <?php echo $post->ID; ?>, // wordpress 文章唯一ID
        avatar: "//gravatar.cn/", // 使用默认 cravatar 源
    },
    class: {
        blackList: ['chatGPT','article_index','ibox'], // 不可选中元素
    },
    element: {
        effectsArea: document.querySelector('.content'), // 仅可选择 ".content" 元素区域
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

## 其他
任何问题及建议可提 issue.