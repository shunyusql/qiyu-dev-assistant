---
name: qiyu-dev-assistant
description: "帮助开发者快速完成网易七鱼(Qiyu)客服平台的系统开发、联调测试与上线验证。融合官方开发者文档中心与云商知识库两本手册，覆盖服务端API签名与免登、Web/iOS/Android/鸿蒙/Uniapp/微信小程序SDK接入、CRM/在线消息/工单/呼叫/短信系统、知识库API、11个三方渠道详细接入步骤、合规指南、开发测试验证清单与常见问题排查。"
whenToUse: "需要开发、集成、测试或验证网易七鱼客服平台相关系统时使用，包括：服务端API对接、各端SDK接入、CRM对接、工单/消息/呼叫/短信开发、三方渠道接入、合规审查等场景。"
metadata:
  sources:
    - "手册A：官方开发者文档中心 https://b.163.com/docs/qiyu"
    - "手册B：云商七鱼智能客服开发指南知识库 https://b.163.com/knowledge/public/WXjbs9n3GC/knowdetail?docId=X9loH09Ibg&pid=431609"
---

# 网易七鱼(Qiyu)开发助手 Skill

> 帮助开发者快速完成七鱼客服平台的系统开发、联调测试与上线验证。
>
> 本 Skill 融合两本手册内容：
> - **手册A（官方开发者文档中心）**：https://b.163.com/docs/qiyu （现行、完整，含服务端API、各端SDK、合规指南）
> - **手册B（云商七鱼智能客服开发指南知识库）**：https://b.163.com/knowledge/public/WXjbs9n3GC/knowdetail?docId=X9loH09Ibg&pid=431609 （含各渠道详细接入步骤、服务端登录接口、接口频率限制等）

---

## 一、平台概览与快速入门

### 1.1 七鱼是什么

网易七鱼是网易集团旗下的智能化服务营销一体化 SaaS 平台，提供：
- **在线客服**：多端聊天、消息推送、会话管理
- **智能机器人**：自动应答、分流、知识库问答
- **呼叫中心**：IVR、呼入/呼出、双向回呼、通话录音
- **工单系统**：创建、流转、回调、SLA
- **数据大屏**：实时报表、绩效统计
- **精准营销**：CRM、标签、客户生命周期
- **企微管理**：群发、群 CRM、好友管理

手册B的总览强调：**七鱼提供了丰富的SDK和服务端开放API，方便企业快速使用七鱼，同时也可以根据自己的业务场景进行一定程度的个性化开发。**

### 1.2 开发准备

1. 注册 [网易七鱼账号](https://b.163.com/business/register)
2. 管理后台 → **系统管理** → **扩展与集成**：获取 `AppKey`、`AppSecret`
3. 下载各端 SDK：[SDK 下载页](https://b.163.com/home/download)
4. 配置企业接口地址（如需要七鱼回调企业系统）

### 1.3 接入总览

| 接入方式 | 适用场景 | 核心能力 |
|---------|---------|---------|
| Web/H5 SDK | 网页、H5、WebView 嵌入 | 在线客服入口、聊天窗口 |
| iOS SDK | iOS App | 原生客服会话、推送、视频 |
| Android SDK | Android App | 原生客服会话、推送、视频 |
| 鸿蒙 SDK | HarmonyOS App | 鸿蒙端客服能力 |
| Uniapp 插件 | Uniapp 跨端 App | 桥接原生 SDK |
| 微信小程序插件 | 微信小程序 | 小程序端客服 |
| 服务端 API | 后端系统对接 | 工单、CRM、消息、工单回调 |
| 三方渠道接入 | 外部平台集成 | 微信、企微、钉钉、飞书、抖音、海外等 |

---

## 二、服务端 API

### 2.1 签名校验（通用说明）

七鱼存在 **4 种签名方式**，务必区分调用方向：

| 调用方向 | 校验方式 | 凭证/签名位置 | 典型场景 |
|---------|---------|-------------|---------|
| 企业服务器 → 七鱼服务器 | **七鱼签名** | Query 参数 `appKey`、`time`、`checksum` | 工单、短信、客户等 OpenAPI |
| 企业服务器 → 云商服务器 | **云商签名** | Header `X-YS-APIKEY`、`X-YS-TIME`、`X-YS-SIGNATURE` | 知识库 |
| 七鱼服务器 → 企业服务器 | **校验 token** | Query/Header 中的 `token` | CRM 信息、订单信息回调 |
| 七鱼服务器 → 企业服务器 | **校验签名** | Header `time`、`nonce`、`checksum` | 消息事件、工单回调 |

#### 七鱼签名计算（手册A + 手册B 一致）

```
checksum = SHA1(appSecret + md5 + time)
md5 = MD5(请求体原文).toLowerCase()
time = 当前UTC秒级时间戳

当 checksumAlgorithm=1（部分接口支持）:
checksum = SHA-256(appSecret + md5 + time)
```

- `checksum` 有效期 **5 分钟**，服务器需与标准时间同步（NTP）
- `appSecret` 在七鱼后台 → 系统 → 扩展与集成 → 开发者 ID 中查看
- 手册B特别提示：**AppSecret 开发者应妥善保管，可在应用的服务器端存储和使用，但不应存储或传递到客户端，也不应在网页等前端代码中嵌入**

#### Java 示例代码（手册B版本）

```java
public class QiyuPushCheckSum {
    private static final char[] HEX_DIGITS = {'0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f'};
    public static String encode(String appSecret, String md5, long time) {
        String content = appSecret + md5 + time;
        try {
            MessageDigest md = MessageDigest.getInstance("sha1");
            md.update(content.getBytes());
            return getFormattedText(md.digest());
        } catch (Exception e) { throw new RuntimeException(e); }
    }
    private static String getFormattedText(byte[] bytes) {
        StringBuilder buf = new StringBuilder(bytes.length * 2);
        for (int j = 0; j < bytes.length; j++) {
            buf.append(HEX_DIGITS[(bytes[j] >> 4) & 0x0f]);
            buf.append(HEX_DIGITS[bytes[j] & 0x0f]);
        }
        return buf.toString();
    }
}
```

#### MD5 计算 Java 代码（手册B版本）

```java
public class MD5 {
    private final static char[] hexDigits = {'0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'};
    public static String md5(String input) { return md5(input, 32); }
    public static String md5(String input, int bit) {
        try {
            MessageDigest md = MessageDigest.getInstance(System.getProperty("MD5.algorithm","MD5"));
            if (bit == 16) return bytesToHex(md.digest(input.getBytes("utf-8"))).substring(8,24);
            if (bit == 28) return bytesToHex(md.digest(input.getBytes("utf-8"))).substring(2,30);
            return bytesToHex(md.digest(input.getBytes("utf-8")));
        } catch (NoSuchAlgorithmException | UnsupportedEncodingException e) { e.printStackTrace(); }
        return null;
    }
    private static String bytesToHex(byte[] bytes) {
        StringBuilder sb = new StringBuilder();
        int t;
        for (int i = 0; i < 16; i++) {
            t = bytes[i];
            if (t < 0) t += 256;
            sb.append(hexDigits[(t >>> 4)]).append(hexDigits[(t % 16)]);
        }
        return sb.toString();
    }
}
```

#### 请求 OpenAPI 完整 Java 流程（手册B）

```java
public static void main(String[] args) {
    String content = "{\"name\":\"qiyu\",\"age\":\"8\"}";        // 请求体
    String md5 = MD5.md5(content).toLowerCase();                  // 请求体md5（小写）
    long time = System.currentTimeMillis() / 1000;                // 秒级时间戳
    String appkey = "appkey";
    String secret = "secret";
    String checksum = QiyuPushCheckSum.encode(secret, md5, time);
    String url = "https://qiyukf.com/xxx";
    url += "?appKey=" + appkey + "&time=" + time + "&checksum=" + checksum;
    String res = HttpClientPool.getInstance().postMethod(url, content);
}
```

### 2.2 API 调试窗口

每个 API 文档页右侧的调试窗口已内置七鱼签名辅助能力。填写 `appKey` 和 `appSecret` 后点击「调试」，窗口自动写入当前秒级 `time`，并按请求体原文计算 `md5` 和 `checksum`。

- 接口参数中存在 `checksumAlgorithm`：传 `1` 用 SHA-256，未传/传 `0` 用 SHA1
- 接口参数表中没有 `checksumAlgorithm` 时，按 `0` 处理

### 2.3 通用错误码（合并手册A + 手册B）

| 错误码 | 描述 |
|-------|------|
| 200 | OK（成功） |
| 14001 | AppKey 不存在 / 参数 appKey 错误 |
| 14002 | checksum 校验失败（请检查 checksum 计算方式） |
| 14003 | 请求时间戳与服务器时间误差超过 5/10 分钟（检查 NTP 同步） |
| 14004 | 请求体为空 / 内容格式校验错误 |
| 14005 | 客服不在线 |
| 14006 | 客服接待达上限，访客排队 |
| 14008 | 请求来源 IP 不在被允许的范围内 |
| 14009 | 请求频率过快，超出接口限制 |
| 14500 | 服务内部异常 |
| 14501 | 数据量过大，超出接口限制 |
| 14515 | 权限不足 |
| 67407 | 短信服务未开启或未注册 |

> 手册B备注：如果 Java 应用调用七鱼服务端接口出现证书报错问题，请升级 jdk 版本至 **1.8_202 及以上**。

### 2.4 接口请求频率限制（手册B独有）

- **接口级限流**：默认情况下，单个接口的请求速率限制为 **5 QPS**
- **租户级限流**：默认情况下，单个租户（Tenant）在所有接口上的聚合请求速率限制为 **20 QPS**
- 部分接口可能有独立高于/低于此默认值的限流策略，以具体接口文档为准

### 2.5 服务端登录接口（免登，手册B独有）

服务端登录可实现七鱼免登效果：**将七鱼客服端作为工具条嵌入第三方网站，或实现第三方认证后对七鱼免登。**

步骤：
1. 向自己服务器端发起异步请求，获取 `SDK` 地址
2. 将 SDK 地址作为 Iframe 工具条嵌入第三方页面，或直接跳转新页签实现免登

**获取 SDK 接入地址：**
```
POST https://qiyukf.com/openapi/staff/login?appKey={appKey}&checksum={checksum}&time={time}
Content-Type: application/json;charset=utf-8
Body: {"staffName": "admin"}
```

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| staffName | String | 是 | 客服在七鱼对应的客服名（客服账号） |

**响应示例：**
```json
{
  "code": 200,
  "message": "",
  "result": {
    "sdk_url": "https://xxx.qiyukf.com/toolbar/script/get?token=cb1ec3c43089493eb4039945685ebf51",
    "corp_code": "xxx",
    "token": "cb1ec3c43089493eb4039945685ebf51"
  }
}
```

- `sdk_url`：含动态登录口令的 SDK 接入地址
- `corp_code`：登录客服对应的七鱼企业域名
- `token`：动态登录口令，**token 失效时间和长连接心跳有关，心跳超时时间 270 秒（4.5 分钟）**

### 2.6 企业接口接入（七鱼回调企业）

**文档位置：** `docs/api/enterprise-interfaces`

七鱼服务器回调企业接口时，采用密钥安全认证。企业需在七鱼后台配置回调 URL。

**关键约定：**
- 接收所有通知事件后，**10 秒内**返回空字符串（事件推送）或 **1 秒内**返回解密后的明文字符串（URL 验证）
- 消息去重使用 `msgId` 字段
- CORS 要求：`Access-Control-Allow-Origin: https://{corpCode}.qiyukf.com`

**CORS 配置：**
```
Access-Control-Allow-Headers: origin, x-csrftoken, content-type, accept, x-auth-code, X-App-Id, X-Token
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Origin: https://{corpCode}.qiyukf.com
```

---

## 三、客户端 SDK 接入

### 3.1 Web / H5 SDK

#### npm 接入（推荐）

```bash
npm install @neysf/qiyu-web-sdk --save
```

```js
import YSF from '@neysf/qiyu-web-sdk';

YSF.init('你的appKey', {
  templateId: 123,
  shuntId: 123,
  sessionInvite: false,
  hidden: 1
}, 'overseas' /* 海外版传此参数 */).then(ysf => {
  ysf('open');
}).catch(err => {
  console.log('SDK加载失败---', err);
});
```

#### 脚本接入

```html
<script>
(function (w, d, n, a, j) {
  w[n] = w[n] || function () {
    return (w[n].a = w[n].a || []).push(arguments)
  };
  j = d.createElement('script');
  j.async = true;
  j.src = 'https://qiyukf.com/script/${appKey}.js?templateId=123&shuntId=123';
  d.body.appendChild(j);
})(window, document, 'ysf');
</script>
```

#### 核心 API（手册A + 手册B 完整列表）

| API | 说明 |
|-----|------|
| `ysf('onready', cb)` | SDK 初始化成功回调 |
| `ysf('config', options)` | 设置用户信息、分流信息 |
| `ysf('product', info)` | 设置商品链接 |
| `ysf('logoff')` | 用户登出 |
| `ysf('open', options)` | 打开客服聊天窗口 |
| `ysf('url')` | 获取打开聊天窗口的 URL |
| `ysf('onunread', cb)` | 未读消息通知回调 |
| `ysf('getUnreadMsg')` | 获取当前未读消息 |
| `ysf('getConversation', cb)` | 获取会话列表 |
| `ysf('onConversation', cb)` | 会话列表新消息回调 |
| `ysf('onSessionMessage', cb)` | 单条新消息回调 |

#### `ysf('config')` 完整参数（手册B详细版）

```js
ysf('config', {
  uid: 'user123',               // 用户标识，不传=匿名
  name: '张三',                 // 用户名称
  email: 'test@163.com',
  mobile: '13888888888',
  bid: '商户id（平台版本）',
  data: JSON.stringify([
    {"key":"real_name","value":"张三"},
    {"key":"avatar","label":"头像","value":"https://..."}
  ]),
  staffid: '客服id',            // 与groupid同时存在时优先
  groupid: '客服组id',
  shuntId: '分流模板id',
  robotShuntSwitch: 1,          // 机器人优先 0关/1开
  level: 1,                     // VIP等级 1-10
  qtype: 1056,                  // 常见问题模板id
  welcomeTemplateId: 1024,      // 机器人欢迎语模板id
  connectYunxin: true,          // 连接云信服务（平台企业）
  wxworkAppId: '企业微信id',    // 需AES加密，Key为AppSecret
  spkf: 1,                      // 主动发起视频邀请
  isShowBack: 1,                // 导航头返回按钮 0/1
  language: 'zh-cn',
  shortcutTemplateId: 1024,     // 常见快捷入口模板id
  layerSize: { width: 360, height: 500, inputHeight: 150 },  // PC浮层
  emojiPopoverWidth: '300px',
  channelExtendInfo: JSON.stringify([
    {"key":"baiduQuery","value":"百度搜索词"},
    {"key":"baiduKeyword","value":"百度关键词"},
    {"key":"baiduChannel","value":"百度来源"}
  ]),
  allowNewTab: 1,               // 图片/文本/PDF新标签打开
  alipayImagePermissionText: '支付宝相册权限文案',
  success: function() {},
  error: function() {}
});
```

**config 参数完整说明表（手册B）：**

| 参数 | 类型 | 描述 |
|------|------|------|
| options.uid | String | 企业当前登录用户标识，不传表示匿名用户 |
| options.name | String | 用户名称 |
| options.email | String | 用户邮箱 |
| options.mobile | String | 用户手机号 |
| options.bid | String | 平台版本时指定咨询商户客服的商户id |
| options.data | String | JSON字符串，avatar字段设置访客头像；含key/value/label；头像还需href |
| options.staffid | String | 指定客服id，非必填；与groupid同时存在时staffid优先级更高 |
| options.groupid | String | 指定客服组id |
| options.shuntId | String | 多入口分流模板id |
| options.robotShuntSwitch | Number | 机器人优先开关（0关闭/1开启） |
| options.level | Number | VIP等级，支持1-10级 |
| options.qtype | Number | 常见问题模板id |
| options.welcomeTemplateId | Number | 机器人欢迎语模板id |
| options.success | Function | 配置成功回调 |
| options.error | Function | 配置失败回调 |
| options.connectYunxin | Boolean | 连接云信服务，限平台企业（平台主账号子账号方式） |
| options.wxworkAppId | String | 企业微信id用于语音功能，需AES加密，Aes Key为AppSecret |
| options.spkf | Number | 主动发起视频邀请开关 |
| options.isShowBack | Number | 导航头返回按钮（0不显示/1显示，浮层模式无效） |
| options.language | String | 语言，默认 zh-cn |
| options.shortcutTemplateId | Number | 常见快捷入口模板id |
| options.layerSize | Object | PC浮层窗口尺寸，最小 {width:360,height:500,inputHeight:150} |
| options.emojiPopoverWidth | string | PC端表情悬浮窗宽度 |
| options.channelExtendInfo | string | 渠道扩展信息，baiduQuery/baiduKeyword/baiduChannel为百度固定key |
| options.allowNewTab | Number | 1=图片文本PDF新标签打开；0=浮层内打开，默认0 |
| options.alipayImagePermissionText | string | 支付宝相册权限文案 |

**语言参数表：** zh-cn(简中) zh-tw(繁中) en(英文) ar(阿语沙特) de(德) ru(俄) fr(法) tl(菲) ko(韩) ja(日) th(泰) id(印尼) vi(越) it(意) pl(波)

**百度营销自建站附加配置（手册B）：**
```js
ysf('config', {
  baiduUcid: 'xx',              // 百度营销通企业ID（必传）
  baiduSelfBuildSiteId: 'xx'    // 百度自建站模式网站siteId（非必传）
})
```

#### 企业微信 AppId AES 加密（手册B完整代码）

`wxworkAppId` 需用 AES 加密，Aes Key 为 App Secret。

```java
public final class AESEncryptUtil {
    private static final ThreadLocal<Map<String, Cipher>> CIPHER_THREAD_LOCAL_EN = new ThreadLocal<>();
    private static final ThreadLocal<Map<String, Cipher>> CIPHER_THREAD_LOCAL_DE = new ThreadLocal<>();

    public static String encrypt(String sSrc, String aesKey) {
        validate(aesKey);
        try { return encrypt_(sSrc, aesKey); }
        catch (Exception e) { throw new RuntimeException(e); }
    }
    public static String decrypt(String sSrc, String aesKey) {
        validate(aesKey);
        try { return decrypt_(sSrc, aesKey); }
        catch (Exception e) { throw new RuntimeException(e); }
    }
    private static void validate(String aesKey) {
        if (strIsBlank(aesKey) || (16 != aesKey.length() && 24 != aesKey.length() && 32 != aesKey.length())) {
            throw new RuntimeException("AES密钥长度只能是 16 24 32 字节");
        }
    }
    public static String encrypt_(String sSrc, String aesKey) throws Exception {
        if (strIsBlank(sSrc) || strIsBlank(aesKey)) return null;
        Cipher cipher = getCipher(CIPHER_THREAD_LOCAL_EN, aesKey, Cipher.ENCRYPT_MODE);
        return bytesToHexStr(cipher.doFinal(sSrc.getBytes("UTF-8")));
    }
    public static String decrypt_(String sSrc, String aesKey) throws Exception {
        if (strIsBlank(sSrc) || strIsBlank(aesKey)) return "";
        Cipher cipher = getCipher(CIPHER_THREAD_LOCAL_DE, aesKey, Cipher.DECRYPT_MODE);
        return new String(cipher.doFinal(hexStrToBytes(sSrc)), "UTF-8");
    }
    private static Cipher getCipher(ThreadLocal<Map<String, Cipher>> tl, String aesKey, int mode) throws Exception {
        Map<String, Cipher> map = tl.get();
        if (map == null) { map = new HashMap<>(10); tl.set(map); }
        Cipher c = map.get(aesKey);
        if (c == null) {
            SecretKeySpec key = new SecretKeySpec(aesKey.getBytes("utf-8"), "AES");
            c = Cipher.getInstance("AES");
            c.init(mode, key);
            map.put(aesKey, c);
        }
        return c;
    }
    private static byte[] hexStrToBytes(String s) {
        byte[] bytes = new byte[s.length() / 2];
        for (int i = 0; i < bytes.length; i++)
            bytes[i] = (byte) Integer.parseInt(s.substring(2*i, 2*i+2), 16);
        return bytes;
    }
    private static String bytesToHexStr(byte[] bcd) {
        StringBuilder s = new StringBuilder(bcd.length * 2);
        for (byte aBcd : bcd) {
            s.append(HEX_LOOKUP[(aBcd >>> 4) & 0x0f]);
            s.append(HEX_LOOKUP[aBcd & 0x0f]);
        }
        return s.toString();
    }
    private static boolean strIsBlank(CharSequence cs) {
        if (cs == null || cs.length() == 0) return true;
        for (int i = 0; i < cs.length(); i++)
            if (!Character.isWhitespace(cs.charAt(i))) return false;
        return true;
    }
    private static final char[] HEX_LOOKUP = {'0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f'};
    // 使用: AESEncryptUtil.encrypt(appId, appSecret)
}
```

### 3.2 iOS SDK

#### 集成方式

**CocoaPods：**
```ruby
pod 'QY_iOS_SDK', '~> 10.9.x'
# 海外版
pod 'QY_iOS_SDK/Abroad', '~> 10.9.x'
# 视频客服版
pod 'QY_iOS_SDK/Video', '~> 10.9.x'
```

**手动集成：** 下载 zip 并拖入以下文件：
- `QYKFNIMSDK.xcframework`（IM 即时通讯）
- `QYKFNIMNOS.xcframework`（IM 依赖）
- `QYSDK.xcframework`（七鱼客服核心）
- `QYVideoService.xcframework`（视频客服，可选）
- Resources 文件夹中的 bundle 资源文件

#### 初始化

```objc
QYSDKOption *option = [QYSDKOption optionWithAppKey:appKey];
option.appName = @"你的App名称";
[[QYSDK sharedSDK] registerWithOption:option completion:^(NSError *error) {
    // 回调处理
}];
```

#### 打开客服

```objc
[QYSDK showChatWindow:nil];
```

手册B概述：iOS SDK 是客服系统访客端解决方案，包含聊天逻辑管理和聊天界面，支持 iOS9 以上版本，支持 iPhone、iPad，支持竖屏和横屏。

#### iOS 版本升级（QY_NIM_iOS_SDK → QY_iOS_SDK）

- Podfile: `pod 'QY_NIM_iOS_SDK'` → `pod 'QY_iOS_SDK'`，重新 pod install
- 头文件：`#import <QYSDK_ReName/***.h>` → `#import <QYSDK/***.h>`
- 视频客服：`QYVideoService_ReName` → `QYVideoService`
- IM 模块：`#import <NIMSDK2/NIMSDK.h>` → `#import <QYKFNIMSDK/QYKFNIMSDK.h>`
- 工程中查找 NIMSDK 类调用，替换为 QYKFNIMSDK

### 3.3 Android SDK

- 支持 Android 5.0+
- 集成方式：下载 SDK 包，配置 Gradle 依赖
- 初始化：`YSF.init(appKey, options)`
- 打开客服：`YSF.showChatWindow(context)`

手册B概述：Android SDK 是 Android 端客服系统访客解决方案，包含聊天逻辑管理和聊天界面，可方便集成到自己的 App 中。

### 3.4 鸿蒙 SDK

- 支持 HarmonyOS 5.0.0(12)+
- 暂不支持七鱼海外版本
- 权限：`ohos.permission.INTERNET`（必选）、`ohos.permission.MICROPHONE`（语音可选）

### 3.5 Uniapp 插件

- 底层使用七鱼 Android/iOS 原生 SDK，通过插件桥接
- 适用于 Uniapp App 端（Android + iOS），不适用于 H5 和小程序
- 接入顺序：准备工作 → 插件接入 → 使用流程 → API 参数说明

### 3.6 微信小程序插件

- 微信小程序访客端 SDK 插件
- 提供客服聊天逻辑管理和聊天界面
- 支持 CRM 对接、视频客服、2.x 版本升级

---

## 四、CRM 对接

### 4.1 轻量 CRM 对接

通过 SDK 发起会话时，将访客信息作为参数传递给七鱼。数据展示在客服工作台的「用户资料」标签。

- **Web SDK：** `ysf('config')` 中的 `data` 字段
- **iOS/Android SDK：** 调用 `setUserInfo` 方法
- **开发难度低**，适合小规模团队

### 4.2 CRM 接口对接

企业提供 HTTP 接口，七鱼系统调用获取访客信息。数据展示在「更多信息」标签。

**错误码约定：**
- `rlt=0`：接口返回正确
- `rlt=1`：用 appid+appsecret 认证时，认证不通过
- `rlt=2`：用 appid+token 认证时，token 过期
- 其他：根据业务逻辑自定义

**七鱼用户认证机制：**
1. 第三方提供生成 `authToken` 的授权接口
2. 申请客服或上报 CRM 信息时，先调用授权接口获取 `authToken`
3. 将 `authToken` 传递给七鱼用于 uid 校验

### 4.3 轻量 vs 接口对接对比

| 对比角度 | 轻量对接 | 接口对接 |
|---------|---------|---------|
| 数据来源 | 客户端通过 SDK 提交 | 企业提供的 HTTP 接口 |
| 调用方式 | 客户端 SDK 提交给七鱼 | 客服工作台直接调企业接口 |
| 数据存储 | 加密保存在七鱼 | 数据不经七鱼服务器，完全隔离 |
| 功能 | 自定义访客信息 | 自定义访客信息+订单类数据+CRM同步+访客信息验证 |
| 展现位置 | 「用户资料」标签 | 「更多信息」标签 |
| 开发难度 | 低 | 中 |
| 适用场景 | 小规模、初期、CRM不完善 | 中大规模、CRM完善、需要订单数据 |

### 4.4 CRM 相关 API

| API | 说明 |
|-----|------|
| `POST /openapi/crm/syncCrmInfo` | 客户导入 |
| `POST /openapi/crm/queryCrmInfo` | 查询 CRM 信息 |
| `POST /openapi/crm/queryCrmInfoByPhone` | 按手机号查询 CRM |
| `POST /openapi/tag/all` | 查询全部客户标签 |
| `POST /openapi/user/serviceRecord` | 查询用户服务记录 |
| `POST /openapi/tag/update` | 更新用户标签 |
| `POST /openapi/tag/batchUpdate` | 批量更新用户标签 |

---

## 五、在线消息系统

### 5.1 消息接口调用流程

1. 用户发送消息给应用服务器
2. 应用服务器向七鱼请求分配客服
3. 七鱼分配空闲客服并生成新会话
4. 分配结果返回应用服务器（建议缓存会话状态）
5. 应用服务器将消息传到七鱼 → 七鱼传递给客服
6. 客服回复 → 七鱼推送到应用服务器 → 回复用户
7. 会话结束（客服主动关闭/超时自动关闭）
8. 七鱼通知应用服务器会话结束

**异常流程：**
- 不请求分配直接发消息：自动触发分配。首次访问且开通机器人 → 分给机器人；之前人工接待过 → 继续分给之前的客服
- 客服不在线 → 返回 `14005`，消息进留言列表；超 5 分钟用户未再发消息，留言关闭
- 客服接待达上限 → 返回 `14006`，访客排队

### 5.2 消息类型（msgType）

| msgType | 类型 | 消息体格式 |
|---------|------|-----------|
| 1 | TEXT 文本 | 字符串 |
| 2 | PICTURE 图片 | `{ext, name, url}` |
| 3 | AUDIO 音频 | `{ext, dur, size, name, url}` |
| 4 | FILE 文件 | `{ext, size, name, url, md5}` |
| 5 | VIDEO 视频 | `{dur, ext, size, name, url}` |
| 6 | TIP 提示词 | 纯文本 |
| 8 | MERGE 合并转发 | `{title, list[{content,createTime,msgType}]}` |
| 9 | MEETINGVOICECALL 企微语音通话 | `{ext, dur, sessionid, url}` |
| 100 | CUSTOM 自定义 | 自定义 JSON |

### 5.3 关键 API

| 接口 | 说明 |
|-----|------|
| 分配客服 | 请求七鱼分配空闲客服 |
| 更新用户资料 | 更新访客昵称、电话、邮件等 |
| 发送消息 | 将消息传到七鱼服务器 |
| 接收消息事件 | 七鱼推送客服消息到企业服务器 |
| 会话开始/结束通知 | 会话生命周期事件 |

---

## 六、工单系统

### 6.1 工单 API 列表

| 接口 | 说明 |
|-----|------|
| `POST /openapi/v2/ticket/create` | 创建工单 |
| `POST /openapi/v2/ticket/draft/create` | 创建工单草稿 |
| `POST /openapi/v2/ticket/search` | 搜索工单（管理员视角） |
| `POST /openapi/v2/ticket/detail` | 获取工单详情 |
| `POST /openapi/v2/ticket/modify` | 修改工单字段 |
| `POST /openapi/v2/ticket/batch-modify` | 批量修改自定义字段 |
| `POST /openapi/v2/ticket/reply` | 回复工单 |
| `POST /openapi/v2/ticket/transfer` | 转交工单 |
| `POST /openapi/v2/ticket/reassign` | 改派工单 |
| `POST /openapi/v2/ticket/apply` | 接单 |
| `POST /openapi/v2/ticket/approve` | 审批工单节点 |
| `POST /openapi/v2/ticket/finish` | 完结工单 |
| `POST /openapi/v2/ticket/reopen` | 重开工单 |
| `POST /openapi/v2/ticket/reminder` | 催单 |
| `POST /openapi/v2/ticket/log` | 查询流转日志 |
| `POST /openapi/v2/ticket/uploadAttachment` | 上传工单附件 |

### 6.2 工单回调接口（七鱼 → 企业）

配置路径：系统管理 → 扩展与集成 → CRM 信息对接 → 工单对接接口 URL

| eventType | 说明 |
|-----------|------|
| 1 | 邮件工单获取访客 CRM 信息 |
| 2 | 流程管理-条件节点的第三方条件 |
| 3 | 画布流程-接口调用节点 |

**响应格式：** `code=200` 代表成功，`message` 为说明，`result` 为数据内容。

---

## 七、呼叫系统（IPCC）

### 7.1 呼叫事件对接

配置路径：系统管理 → 扩展与集成 → CRM 信息对接 → 呼叫对接接口 URL

| eventtype | 说明 |
|-----------|------|
| 1 | 获取访客信息（呼入/呼出时） |
| 2 | IVR 校验 |
| 3 | 自定义 IVR 接口 |
| 4 | 播放内容接口 |
| 5 | 挂断时同步通话记录 |
| 6 | 同步电话服务记录字段 |
| 7 | 接起时同步通话记录 |
| 8 | 外呼时同步通话信息 |
| 9 | 外呼任务同步自定义字段 |
| 10 | 回铃音检测结果 |
| 11 | IVR 数据请求 |
| 12 | 实时 ASR 转译结果 |
| 13 | 外呼时获取用户真实号码 |

### 7.2 通话记录字段（eventtype=5）

关键字段：`sessionid`、`direction`、`from`、`to`、`user`、`staffid`、`staffname`、`status`、`duration`、`recordurl`（录音地址）、`evaluation`（满意度）、`waitDuration`、`ringDuration`、`resolved`（解决状态）等。

---

## 八、短信系统

| 接口 | 说明 |
|-----|------|
| `POST /openapi/smstask/templates` | 获取使用中的短信模板 |
| `POST /openapi/smstask/create` | 创建短信任务 |
| `POST /openapi/smstask/status` | 查询短信任务状态（taskIdList ≤ 100 个） |

**模板类型：** 1-验证码，2-通知，3-营销

---

## 九、知识库 API

### 9.1 外部开放接口（OpenApiController）

提供空间、目录、文档和切片能力。使用 **云商签名**（Header 方式：`X-YS-APIKEY`、`X-YS-TIME`、`X-YS-SIGNATURE`）。

主要接口：
- `POST /api/athena/openApi/external/getSpaceList`：获取知识库空间列表
- `POST /api/athena/openApi/external/getParentIdList`：获取空间目录列表
- `POST /api/athena/openApi/external/getDocList`：获取空间下所有文档
- `POST /api/athena/openApi/external/getDocInfo`：获取单个知识点详情
- `POST /api/athena/openApi/external/docSliceList`：获取文档切片列表
- `POST /api/athena/openApi/external/docMetaList`：批量获取文档元数据
- `POST /api/athena/openApi/external/importDoc`：导入文档
- `POST /api/athena/openApi/external/updateDoc`：更新文档文件

### 9.2 Agent Tool 接口

通过个人凭证调用查询、导航和操作类知识库工具。

---

## 十、三方渠道接入（手册B详细步骤 + 手册A概述）

### 10.1 微信生态（手册A）

| 渠道 | 接入方式 |
|-----|---------|
| 微信客服 | 微信客服渠道接入 |
| 微信公众号 | 公众号菜单接入客服 |
| 微信小店 | 小店客服接入 |
| 微信小程序 | 小程序插件接入 |
| 企业微信 | 扫码授权接入（服务商主体：网易质云） |

**企业微信授权：** 服务中心-在线系统-设置-在线接入-企业微信-员工服务（在线专业版及以上），管理员扫码授权。授权后享受 90 天企业微信接口调用试用期，之后需付费购买许可账号并激活。

### 10.2 抖音企业号（手册B详细）

**场景：** 在七鱼系统中收发抖音企业号私信。

**特别说明（抖音官方规定）：**
- 同一个账号建议每 30 天重新授权绑定，否则有自动解绑风险
- 未相互关注时，用户发一条消息后，客服 48 小时内最多回复 3 条
- 访客端只有纯文本客服才可收到（语音/图片/视频/表情包收不到）
- 客服端只有文本、图片访客才可收到

**绑定步骤：**
1. 进入【在线系统 → 设置 → 在线接入 → 抖音企业号】，点击【绑定抖音企业号】
2. 手机登录抖音企业号扫码授权（或 PC 端账号密码登录授权）
3. 确认账号权限支持授权（需蓝 V 抖音企业号）
   - 七鱼-抖音V1版：获取公开信息、私信列表、使用私信能力
   - 七鱼-抖音V2版：获取用户公开信息、私信消息管理、上传图片工具、线索组件消息卡片、消息撤回、多媒体消息
4. 完成绑定后即可收发私信，支持与其他渠道统一接待，展现抖音来源标识
5. 可在对应渠道开启机器人接待

### 10.3 百度营销（手册B详细）

**特别说明：**
- 需通过基木鱼建立投放落地页（含咨询入口）方可授权
- 完全解绑需在百度营销通后台也操作解绑
- 会话结束后无法重新向客户发起会话
- 访客端只有文本、图片、表情客服才可收到
- 客服端只有文本、图片访客才可收到

**授权绑定步骤：**
1. 【在线系统 → 设置 → 在线接入 → 百度营销】点击【立即绑定】
2. 登录百度营销通 → 【咨询】→【通用咨询】→【客服账号授权】→【新增授权】（https://yingxiaotong.baidu.com）
3. 选择第三方咨询工具"网易七鱼"授权绑定
4. 输入七鱼管理员域名、账号和密码完成授权
5. 查看绑定结果（建议在在线系统内编辑录入企业名称便于报表管理）
6. 建立接待方案：进入【咨询管理】→【新建咨询方案】，设置客服账号选七鱼，可接入机器人
7. 接入会话测试：【预览】测试会话
8. 上线：通过百度基木鱼建立投放落地页，落地页在线咨询组件关联咨询方案

**百度营销自建站接入：**
1. 完成企业绑定（步骤一至五）
2. 【在线系统 → 设置 → 在线接入 → 百度营销】点击自建站接入设置
3. 新增网站信息，生成网站 ID
4. 代码配置：`ysf('config', {baiduUcid: 'xx', baiduSelfBuildSiteId: 'xx'})`（要求接入页 referrer 是百度域名且含 xst 参数）
5. 线索转化回传设置（可选）：配置 token 和回传事件（有效咨询/三句话咨询/留线索）

### 10.4 小红书（手册B详细）

**接入前提：**
- 申请注册小红书企业专业号并完成企业认证（蓝标打勾）
- 用主账号绑定的手机号+验证码登录聚光平台开户与资质认证

**特别说明：**
- 所有消息需通过小红书社区私信风控、导流审核规则（设置欢迎语/机器人自动回复时不要用数字英文组合或携带超链接）
- 聚光平台-线索管理中开启自动抓取留资，可将对话中手机号、微信号同步至七鱼
- 仅支持企业号和 KOS 员工号私信，不支持电商客服

**授权绑定步骤：**
1. 【在线系统 → 设置 → 在线接入 → 小红书】点击【绑定小红书企业专业号】
2. 小红书商业开放平台登录（用专业号主账号）
3. 选择授权范围：仅企业号 / 企业号及其绑定的员工号，勾选"三方工具服务、私信留资数据"
4. 七鱼发起第二步授权，点击【绑定聚光账号】
5. 聚光平台登录 → 【工具-三方客服管理】→ 找到【网易七鱼】→【授权新账号】→ 用七鱼超级管理员登录授权
6. 绑定员工号（KOS）
7. 确认授权成功，查看员工账号
8. 完成绑定后收发小红书私信，展现来源标识

**获客工具（服务卡/名片/落地页）：**
1. 在小红书专业号平台和聚光平台设置用户留资卡、商家名片、商家落地页
2. 打开七鱼后台【获客工具侧边栏】开关（默认关闭）
3. 客服工作台发送已审核的获客工具卡片
4. 用户填写留资卡后，手机号/微信号通过留资接口同步至客户信息

### 10.5 Facebook（手册B详细）

**特别说明：**
- 距访客最后一次发消息超 7 天，客服无法再回复（部分用户 24h）
- 主页私信和帖子评论均可接入，支持机器人接待私信

**绑定步骤：**
1. 【在线系统 → 设置 → 在线接入 → Facebook】点击【绑定你的Facebook主页】
2. 登录 Facebook 官方账号，【Continue as ...】
3. 选择需要接入的主页，【Next】
4. 开启七鱼申请的所有必要权限，【Done】
5. 【OK】确认接入
6. 显示绑定成功
7. 账号配置：默认私信开启评论关闭。开启评论后，主页帖子生成工单，可直接回复并回外部用户评论
8. 所有发送到 Facebook 的操作需勾选"访客可见"

### 10.6 Line（手册B详细）

**特别说明：**
- 仅支持 Line 官方号
- secret 和 token 改变时需重新绑定
- 单个账号每月免费发送限额 500 条（按群成员人数计）

**绑定步骤：**
1. Line 开发者控制台创建 Messaging API channel
2. 获取 Channel access token 和 Channel secret
3. 【在线系统 → 设置 → 在线接入 → Line】点击【绑定你的Line官方号】
4. 填写 token 和 secret
5. 复制消息推送 URL（服务器地址）
6. Line 开发者控制台 → Messaging API settings → Webhook settings → 编辑 Webhook URL 填写推送 URL
7. Allow bot to join group chats → Edit
8. Line 官方号管理平台 → Settings → Response settings → 响应模式设 Bot，禁用自动响应，开启 Webhook
9. 完成绑定收发 Line 私信

### 10.7 Discord（手册B详细）

**准备：** Discord 账号+服务器，Discord 开发者门户创建应用

**绑定步骤：**
1. 应用 → 【Bot】→【Add Bot】
2. 开启权限：PUBLIC BOT、PRESENCE INTENT、SERVER MEMBERS INTENT、MESSAGE CONTENT INTENT
3. OAuth2 → URL Generator：SCOPES 勾选 bot，BOT PERMISSIONS 勾选发送/管理消息权限（或 Administrator），复制 URL
4. 将 OAuth2 URL 复制到浏览器授权 Bot 给服务器
5. 获取 Bot token（Reset Token → Copy）
6. 【在线接入-Discord-绑定Bot】，输入 Bot Token
7. 等待约 10 分钟生效，机器人显示在线即可接入会话

**会话接入：**
- 私信：服务器中给 Bot 发私信，七鱼同步生成会话
- 公屏群聊：文本聊天室发消息，七鱼生成群会话，展示来源机器人+服务器+群聊名+群成员

### 10.8 WhatsApp（手册B详细）

**特别说明：**
- 仅支持 WhatsApp Official Business API Accounts
- 需申请 Facebook 开发者账号 & FBM 平台账号并完成企业验证
- 已在 Messenger/Business App 注册的手机号需注销迁移
- 在其他供应商注册过 WABA 需先解除绑定

**接入步骤：**
1. 提供注册信息给七鱼工作人员（FBM账号名称ID、WABA账号名称、时区、企业显示名、企业类别、企业介绍、注册手机号 E.164 格式）
2. 获得 WhatsApp 批准（FBM 后台可查审核状态）
3. 七鱼后台【在线系统-在线接入-WhatsApp】绑定已验证号码
4. 用户通过「点击聊天短链接」或「关联二维码」发起会话
5. 可选：申请小绿勾（需企业网站、背景、排名、新闻报道等材料，约1周通知）

**商业账户差异：**
- Personal WhatsApp：手机app下载、无团队管理、无机器人
- WhatsApp Business：手机app下载、有企业描述/标签/快速回复/问候离开消息
- WhatsApp Business API：消息存七鱼、团队管理✅、机器人✅、自动打标签✅、绿勾✅

### 10.9 Twitter（手册B详细）

- 距访客最后一次发消息超 24 小时无法再回复
- 支持私信接入，暂不支持机器人接待
- 绑定：进入【在线系统 > 设置 > 在线接入 > Twitter】→【绑定你的Twitter账号】→ 登录授权

### 10.10 Instagram（手册B详细）

**特别说明：**
- 距用户最后一次发消息超 7 天无法回复（部分用户 24h）
- 仅支持 Instagram Professional 专业帐户，需通过 Facebook 帐户间接访问

**绑定步骤：**
1. 确认 Instagram 账号为 Professional 专业帐户（个人中心-设置-切换为专业账户）
2. 在 Facebook 官方主页绑定 Instagram 专业账号（Facebook Page → Settings → Instagram → Connect Account）
3. 七鱼【在线系统 → 设置 → 在线接入 → Instagram】→【绑定Instagram专业账户】
4. Facebook 登录授权，选择 Instagram 专业账户
5. 开启必要权限（邮箱、管理主页对话、主页列表），Done → OK

### 10.11 钉钉（手册B详细）

**员工服务 - 自建应用接入：**
1. 获取 CorpId：钉钉开发者后台获取企业 CorpId 回填七鱼
2. 创建企业内部应用（例：HR服务中心）
3. 获取 AgentId、AppKey、AppSecret 回填七鱼
4. 将七鱼生成的应用首页地址填入钉钉应用首页&PC首页
5. 应用授权：勾选企业员工手机号信息、邮箱等个人信息、通讯录部门读权限、成员信息读权限
6. 配置字段映射：钉钉员工字段与七鱼客户信息字段映射（自动身份识别，便于会话路由、机器人回复、工单协同）
7. 发布应用

**跨团队协作（轻工单）：**
1. 获取 CorpId、创建应用（七鱼轻工单）
2. 回填 AgentId、AppKey、AppSecret、RobotCode
3. 回填应用首页地址&PC首页、回调地址
4. 授权：个人手机号、通讯录个人信息/部门信息/成员读权限、部门成员读权限、企业内机器人发送消息权限
5. 发布应用，调整可见权限
6. 同步员工到【外部员工管理】（钉钉员工免登录查看处理工单）
7. 授权 im 工单权限

### 10.12 飞书（手册B详细）

**员工服务 - 自建应用接入：**
1. 登录飞书开发者后台，创建企业自建应用（例：HR服务中心）
2. 获取 App ID、App Secret 回填七鱼
3. 将七鱼生成的应用首页地址填入桌面端首页&移动端首页
4. 授权：获取用户邮箱、用户基本信息、部门基础信息、用户手机号、以应用身份读取通讯录
5. 配置字段映射
6. 创建版本发布（版本号 1.0.0，可见范围建议全员）

**跨团队协作（轻工单）：**
1. 创建企业自建应用（网易七鱼轻工单）
2. 回填 App ID、App Secret
3. 回填首页链接、安全设置重定向地址、机器人-我受理工单地址、机器人-我完结工单地址
4. 授权：用户基本信息、部门基础信息、个人手机号、应用信息、单聊群组消息、通讯录基本信息、通讯录部门组织架构
5. 创建版本发布
6. 飞书管理员审批
7. 同步员工到【外部员工管理】
8. 授权 im 工单权限

### 10.13 其他渠道（手册A概述）

- **邮件**：配置 SMTP 发件服务和自动转交
- **有赞/微盟/淘宝闪购/美团闪购**：电商店铺接入
- **Bilibili**：B 站渠道接入
- **App Store/Google Play**：应用商店评论接入
- **POPO**：POPO 相关能力

---

## 十一、合规指南（手册A完整）

### 11.1 App SDK 合规要求

**必选权限（Android）：**
- `INTERNET`：网络通信
- `ACCESS_NETWORK_STATE`：网络状态
- `ACCESS_WIFI_STATE`：Wi-Fi 状态

**可选权限（Android）：**
- `RECORD_AUDIO`：语音消息
- `CAMERA`：图片/视频
- `READ/WRITE_EXTERNAL_STORAGE`：文件读写
- `POST_NOTIFICATIONS`：消息通知
- `BLUETOOTH`/`WAKE_LOCK`：音视频通话
- `READ_PHONE_STATE`：设备状态

**可选权限（iOS - Info.plist）：**
- `NSCameraUsageDescription`：摄像头
- `NSMicrophoneUsageDescription`：麦克风
- `NSPhotoLibraryUsageDescription`：相册读取
- `NSPhotoLibraryAddUsageDescription`：相册写入
- `NSBluetoothAlwaysUsageDescription`：蓝牙（视频客服）

**鸿蒙权限：**
- 必选：`ohos.permission.INTERNET`
- 可选：`ohos.permission.MICROPHONE`

### 11.2 隐私政策披露模板

```
SDK名称：网易七鱼 SDK
第三方主体：杭州网易智企科技有限公司
合作目的：为 App 用户提供在线客服、消息收发等能力。
处理个人信息类型及用途：设备信息、网络信息、APNS Token、Bundle ID/包名、
  设备运行状态、用户主动提交的语音/图片/视频信息、视频客服相关音视频信息、
  CRM 自定义字段等；用于在线客服服务、消息投递、多端同步、问题排查、
  账户安全保障及客服服务管理。
数据处理方式：通过去标识化、加密传输及其他安全方式
隐私权政策链接：https://b.163.com/home/privacypolicy/qy-privacy
```

### 11.3 SDK 可选个人信息配置

| 个人信息 | 配置方式 |
|---------|---------|
| 语音 | Android: `RECORD_AUDIO` 权限 + `tools:node="remove"` 移除；iOS: Info.plist 配置 |
| 图片/视频 | Android: 存储/相机权限；iOS: 相册/相机权限 |
| 通知栏 | Android: `YSFOptions.statusBarNotificationConfig` |
| 分辨率 | Android: `YSFOptions.screenDisplayMetrics` |
| 复制消息 | Android/iOS: `messageCopyBtnSwitch` 参数 |
| 传感器 | Android/iOS: `proximitySensorEnable` 参数 |

---

## 十二、开发测试验证清单

### 12.1 开发前检查

- [ ] 已注册七鱼账号并登录管理后台
- [ ] 已获取 `AppKey` 和 `AppSecret`
- [ ] 确认企业版本（基础版/专业版/平台版/海外版）
- [ ] 确认需要接入的端（Web/iOS/Android/小程序/Uniapp/鸿蒙）
- [ ] 确认是否需要服务端 API 对接（工单/CRM/消息/呼叫/短信）
- [ ] 确认是否需要三方渠道接入
- [ ] 确认 NTP 时间同步已配置（checksum 有效期 5 分钟）
- [ ] 确认 JDK 版本 ≥ 1.8_202（避免证书报错）

### 12.2 签名验证测试

```bash
# 使用 cURL 验证签名
curl -X POST "https://qiyukf.com/openapi/smstask/templates?appKey=YOUR_APP_KEY&time=$(date +%s)&checksum=YOUR_CHECKSUM" \
  -H "Content-Type: application/json" \
  -d '{}'
```

**限流检查：** 单接口 5 QPS，单租户聚合 20 QPS，注意测试频率。

### 12.3 Web SDK 接入测试

- [ ] 页面加载后右下角出现客服入口
- [ ] 点击入口可打开聊天窗口
- [ ] 可正常发送文字/图片消息
- [ ] 未读消息通知正常触发
- [ ] `ysf('config')` 用户信息正确传递
- [ ] 域名白名单配置正确（SDK_LOAD_ERROR / 403 排查）
- [ ] 百度营销自建站：baiduUcid/baiduSelfBuildSiteId 配置正确，referrer 校验通过

### 12.4 iOS SDK 接入测试

- [ ] CocoaPods 依赖安装成功
- [ ] `QYSDKOption` 初始化成功（appKey 正确、无空格）
- [ ] `showChatWindow` 可正常打开聊天窗口
- [ ] 消息收发正常
- [ ] 推送功能正常（如配置）
- [ ] 视频客服功能正常（如集成视频模块）

### 12.5 Android SDK 接入测试

- [ ] Gradle 依赖配置正确
- [ ] 初始化成功
- [ ] 聊天窗口打开/关闭正常
- [ ] 消息收发正常
- [ ] 推送通知正常
- [ ] 权限申请流程正确

### 12.6 服务端 API 测试

- [ ] checksum 计算正确（SHA1 / SHA-256）
- [ ] time 参数在 5 分钟内
- [ ] AppKey 有效
- [ ] 请求体 JSON 格式正确
- [ ] Content-Type 为 `application/json;charset=utf-8`
- [ ] 回调接口 10 秒内返回空字符串
- [ ] CORS 配置正确（如七鱼工作台浏览器直接调用）
- [ ] 消息去重使用 `msgId` 字段

### 12.7 CRM 对接测试

- [ ] 轻量 CRM：SDK 传递的访客信息在客服工作台可见
- [ ] 接口 CRM：企业接口响应格式正确（`rlt=0`）
- [ ] CRM 认证机制：`authToken` 生成和校验正常
- [ ] CORS 跨域配置正确

### 12.8 工单系统测试

- [ ] 创建工单成功
- [ ] 查询工单详情正确
- [ ] 工单流转（转交/改派/回复）正常
- [ ] 工单回调 URL 配置正确
- [ ] eventType 对应的请求/响应格式正确
- [ ] 工单附件上传成功

### 12.9 呼叫系统测试

- [ ] 呼叫事件 URL 配置正确
- [ ] eventtype=1：呼入时获取访客信息正常
- [ ] eventtype=2：IVR 校验正常
- [ ] eventtype=5：通话记录同步正常（含录音地址）
- [ ] 通话时长、满意度等字段正确

### 12.10 短信测试

- [ ] 短信模板查询正常
- [ ] 短信任务创建成功
- [ ] 任务状态查询正常
- [ ] 短信服务已注册并开启

### 12.11 三方渠道接入测试（手册B补充）

- [ ] 抖音：30 天内重新授权、私信收发、消息类型限制（纯文本/文本图片）验证
- [ ] 百度营销：基木鱼落地页、咨询方案关联、线索回传事件验证
- [ ] 小红书：专业号企业认证、聚光授权、留资同步、获客工具侧边栏验证
- [ ] Facebook：主页绑定、私信+评论工单、访客可见勾选验证
- [ ] Line：Channel token/secret、Webhook URL 配置、响应模式验证
- [ ] Discord：Bot 权限、OAuth2 授权、私信+群聊会话验证
- [ ] WhatsApp：WABA 申请、号码绑定、点击聊天链接/二维码验证
- [ ] Twitter：24h 回复时效、私信接入验证
- [ ] Instagram：专业账户、FB 主页绑定、私信接入验证
- [ ] 钉钉：自建应用接入、字段映射、员工同步、im工单权限验证
- [ ] 飞书：自建应用接入、版本发布、员工同步、im工单权限验证

### 12.12 合规检查

- [ ] Android 必选权限已配置
- [ ] Android 可选权限按实际需要配置（不需要的用 `tools:node="remove"` 移除）
- [ ] iOS Info.plist 权限描述已添加
- [ ] 鸿蒙权限配置正确
- [ ] 隐私政策已披露七鱼 SDK 信息
- [ ] CRM 自定义字段在隐私政策中披露
- [ ] 第三方共享清单已更新

---

## 十三、常见问题排查

### 13.1 Web SDK 问题

| 问题 | 解决方案 |
|-----|---------|
| `SDK_LOAD_ERROR` / `403 Forbidden` | 管理后台 → 在线客服 → 设置 → 在线接入 → 网站 → 接入代码 → 设定接入域名白名单，添加完整域名（含协议） |
| WebView 中无法打开 | 确保 WebView 开启了 `localStorage` 功能 |
| 微信环境中提示非官方网页 | 点击继续访问，或参考 H5 微信环境 FAQ |

### 13.2 签名问题

| 问题 | 解决方案 |
|-----|---------|
| checksum 校验失败 (14002) | 检查 appSecret、md5 计算（小写）、time 是否正确 |
| 时间戳误差超限 (14003) | 配置 NTP 同步，确保服务器时间与标准时间一致 |
| 请求频率过快 (14009) | 单接口 5 QPS，单租户 20 QPS，降低频率 |
| IP 不在白名单 (14008) | 在七鱼后台配置允许的请求来源 IP |
| 权限不足 (14515) | 检查企业版本和接口权限 |
| 混淆七鱼签名与企业接口签名 | 七鱼签名：`appSecret + md5 + time`；企业接口：`appSecret + nonce + time` |

### 13.3 iOS SDK 升级问题

- 从 `QY_NIM_iOS_SDK` 升级到 `QY_iOS_SDK`：修改 Podfile、头文件引用（`QYSDK_ReName` → `QYSDK`）、NIMSDK 类替换
- 动态库集成：V5.10.0 后已改为动态库，注意 Embed & Sign 配置

### 13.4 回调接口问题

| 问题 | 解决方案 |
|-----|---------|
| 七鱼认为请求发送失败并重发 | 确保 10 秒内返回空字符串（事件推送） |
| URL 验证失败 | 确保 1 秒内返回解密后的明文字符串，不包 JSON、不加引号、不带 BOM |
| CORS 预检失败 | 配置 `OPTIONS` 预检和 `POST` 请求的响应头 |

### 13.5 渠道接入问题（手册B补充）

| 渠道 | 问题 | 解决方案 |
|------|------|---------|
| 抖音 | 无法授权/找不到权限 | 确认是蓝V企业号；联系抖音官方客服 |
| 百度营销 | 无法完全解绑 | 在百度营销通后台也操作解绑 |
| 小红书 | 私信进电商客服 | 按文档调整（店铺客服仍在千帆，无法接入） |
| Line | 消息收不到 | 确认 Webhook URL 已配置、响应模式为 Bot、Webhook 已开启 |
| Discord | 机器人未生效 | 绑定后需等约 10 分钟刷新服务器页面 |
| WhatsApp | 迁移 | 原供应商解除绑定后再由七鱼工作人员协助迁移 |
| 飞书/钉钉 | 员工无法处理工单 | 确认已同步员工到外部员工管理并授权 im 工单权限 |

---

## 十四、快速开发流程

### 14.1 Web 端接入流程

```
1. 获取 AppKey
2. 安装 @neysf/qiyu-web-sdk
3. 在页面中初始化 SDK: YSF.init(appKey, config)
4. 通过 ysf('config') 传递用户信息
5. 通过 ysf('open') 打开客服窗口
6. 配置域名白名单
7. 测试消息收发
8. 配置合规信息
```

### 14.2 iOS 端接入流程

```
1. 获取 AppKey + AppName
2. 添加 CocoaPods 依赖: pod 'QY_iOS_SDK'
3. pod install
4. 在 didFinishLaunchingWithOptions 中初始化 SDK
5. 调用 showChatWindow 打开客服
6. 配置 Info.plist 权限
7. 配置合规信息
```

### 14.3 服务端 API 对接流程

```
1. 获取 AppKey + AppSecret
2. 实现 checksum 签名计算（SHA1/SHA256，含MD5小写）
3. 实现目标 API（工单/CRM/消息/呼叫/短信）
4. 配置企业回调 URL（如需要七鱼推送）
5. 配置 CORS（如七鱼工作台浏览器调用）
6. 测试签名验证（注意5分钟时效、5QPS限流）
7. 测试 API 请求/响应
8. 测试回调接口（10 秒内响应）
```

### 14.4 CRM 对接流程

```
轻量对接：
1. 通过 SDK 配置用户信息
2. 在 SDK 调用中传递 data 字段
3. 验证客服工作台用户资料标签

接口对接：
1. 开发企业 HTTP 接口
2. 实现 rlt 错误码
3. 配置 CORS
4. 配置接口认证方式
5. 在七鱼后台配置接口 URL
6. 测试数据展示在「更多信息」标签
```

### 14.5 渠道接入快速参考（手册B）

```
抖音:     后台绑定 → 扫码授权 → 确认蓝V权限 → 收发私信
百度营销:  后台绑定 → 营销通授权 → 建咨询方案 → 基木鱼落地页 → 线索回传
小红书:    企业专业号认证 → 聚光开户 → 双重授权 → 绑定员工号 → 获客工具
Facebook: 后台绑定 → FB授权 → 选主页 → 开权限 → 配置私信/评论
Line:     开发者后台建channel → 拿token/secret → 七鱼绑定 → 配Webhook
Discord:  建应用/Bot → OAuth2授权 → 拿token → 七鱼绑定
WhatsApp: 提供注册信息 → 等批准 → 绑定号码 → 生成短链/二维码
Twitter:  后台绑定 → 授权 → 收发私信(24h时效)
Instagram: 切专业账户 → FB主页绑定 → 七鱼授权
钉钉:     获取CorpId → 建应用 → 回填信息/权限 → 字段映射 → 发布 → 同步员工
飞书:     建应用 → 回填AppID/Secret → 授权/重定向/机器人地址 → 发布 → 审批 → 同步员工
```

---

## 十五、文档索引

### 手册A（官方开发者文档中心 b.163.com/docs/qiyu）

| 模块 | 文档路径 |
|-----|---------|
| 七鱼简介 | `/docs/qiyu/guide/introduction` |
| 快速入门 | `/docs/qiyu/guide/quickstart/overview` |
| 开发准备 | `/docs/qiyu/guide/quickstart/prerequisites` |
| 通用说明（签名） | `/docs/qiyu/api/general-instructions` |
| 企业接口接入 | `/docs/qiyu/api/enterprise-interfaces` |
| Web SDK 概述 | `/docs/qiyu/sdks/web/overview` |
| Web SDK 接入 | `/docs/qiyu/sdks/web/integration` |
| iOS SDK 概述 | `/docs/qiyu/sdks/ios/overview` |
| iOS SDK 接入 | `/docs/qiyu/sdks/ios/integration` |
| Android SDK 概述 | `/docs/qiyu/sdks/android/overview` |
| 鸿蒙 SDK 概述 | `/docs/qiyu/sdks/harmonyos/overview` |
| Uniapp SDK | `/docs/qiyu/sdks/uniapp/overview` |
| 微信小程序插件 | `/docs/qiyu/sdks/wechat-miniprogram/overview` |
| 在线消息接口 | `/docs/qiyu/api/chat/online-message-api` |
| 消息类型 | `/docs/qiyu/api/chat/message-types` |
| CRM 对接概述 | `/docs/qiyu/api/user/crm/overview` |
| 工单接口 | `/docs/qiyu/api/ticket/integration/ticket-interface-integration` |
| 呼叫事件对接 | `/docs/qiyu/api/ipcc/call/call-events` |
| 短信模板 | `/docs/qiyu/api/sms/post-openapi-smstask-templates` |
| 合规指南 | `/docs/qiyu/guide/compliance/sdk-compliance` |
| 三方接入总览 | `/docs/qiyu/integrations` |

### 手册B（知识库：网易云商七鱼智能客服开发指南）

知识库空间：`b.163.com/knowledge/public/WXjbs9n3GC`（【已废弃】，部分内容已被手册A取代，但渠道接入步骤、服务端登录接口、频率限制仍具参考价值）

| 文档 | docId |
|------|-------|
| 总览 | `X9loH09Ibg` |
| iOS SDK 概述 | `X8GfPS3HrU` |
| Android SDK 概述 | `X8Gi1ypFRo` |
| Web/H5 SDK 概述 | `X8GdkusvgW` |
| 微信小程序 | `X8Gg3LGF7Y` |
| 微信小程序SDK | `X9lwSt4a6i` |
| 微信客服 | `X8Gag1vZUe` |
| 企业微信 | `X8Giu0r7VA` |
| 抖音企业号 | `X8GiJZfiJk` |
| 百度营销 | `X8GbfSwosK` |
| 小红书 | `i1JYTatG7c` |
| Facebook | `X8GhGUzKOe` |
| Line | `X8GhwIqd5k` |
| Discord | `X8GbKASrmC` |
| WhatsApp | `X8GjwFatYO` |
| Twitter | `X8GZy7DAlk` |
| Instagram | `XGU7mMl94y` |
| 钉钉 | `X8Gh15p8lc` |
| 飞书 | `X8Gd2F38Qy` |
| 服务端API通用说明 | `X8GeGW2MVs` |

---

> **Skill 使用建议：** 开发者在接入七鱼时，按「平台概览 → 选择接入方式 → 对应 SDK/API 章节 → 开发测试清单 → 合规检查」的顺序进行。签名、回调、CORS 问题参考「常见问题排查」。渠道接入（抖音/百度/小红书/Facebook/Line/Discord/WhatsApp/Twitter/Instagram/钉钉/飞书）详细步骤见第十章。服务端登录免登、接口频率限制见第二章。
