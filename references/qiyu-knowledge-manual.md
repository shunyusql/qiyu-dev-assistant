# 网易云商七鱼智能客服开发指南（知识库手册）

> 来源：b.163.com/knowledge/public/WXjbs9n3GC（【已废弃】网易云商七鱼智能客服开发指南）



---

## 总览

# 总览

网易七鱼提供了丰富的SDK和服务端开放API，方便企业快速使用七鱼，同时也可以根据自己的业务场景，进行一定程度的个性化开发。

## iOS

提供iOS访客端SDK，既包括聊天界面的样式，也包含相应的聊天逻辑。企业开发者可以方便的将客服功能集成到自己的App中。查看详情

## Android

提供Android访客端SDK，既包括聊天界面的样式，也包含相应的聊天逻辑。企业开发者可以方便的将客服功能集成到自己的App中。查看详情

## Web

提供Web和H5的访客端SDK，既包括聊天界面的样式，也包含相应的聊天逻辑。企业开发者可以方便的将客服功能集成到自己的App中。查看详情

## 微信小程序

提供微信小程序的接入方式，企业可以快捷接入小程序客服。查看详情

## 微信小程序SDK

提供微信小程序的接入方式，企业可以快捷接入小程序客服。查看详情

## 微信客服

提供微信客服的接入方式，企业可以快捷接入微信客服。查看详情

## 企业微信

提供企业微信的接入方式，企业可以快捷接入企业微信。查看详情

## 抖音企业号

提供抖音企业号的接入方式，企业可以快捷接入抖音企业号。查看详情

## 百度营销

提供百度营销的接入方式，企业可以快捷接入百度营销。查看详情

## 小红书

提供小红书企业专业号快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可。查看详情

## Facebook

提供Facebook的接入方式，企业可以快捷接入Facebook。查看详情

## Line

提供Line的接入方式，企业可以快捷接入Line。查看详情

## Discord

提供Discord的接入方式，企业可以快捷接入Discord。查看详情

## WhatsApp

提供WhatsApp的接入方式，企业可以快捷接入WhatsApp。查看详情

## Twitter

提供Twitter的接入方式，企业可以快捷接入Twitter。查看详情

## Instagram

提供Instagram的接入方式，企业可以快捷接入Instagram。查看详情

## 钉钉

提供钉钉的接入方式，企业可以快捷接入钉钉。查看详情

## 飞书

提供飞书的接入方式，企业可以快捷接入飞书。查看详情

## 服务端

提供各种应用的服务端开放API，企业开发者可以对接内部系统的数据，进行个性化开发等。查看详情


---

## iOS SDK 概述

## 概述

网易七鱼 iOS SDK 是客服系统访客端的解决方案，既包含了客服聊天逻辑管理，也提供了聊天界面，开发者可方便的将客服功能集成到自己的 App 中。iOS SDK 支持 iOS9 以上版本，同时支持 iPhone、iPad，同时支持竖屏和横屏。


---

## Android SDK 概述

# 概述

网易七鱼 Android SDK 是一个 Android 端客服系统访客解决方案，既包含了客服聊天逻辑管理，也提供了聊天界面，开发者可方便的将客服功能集成到自己的 App 中。


---

## Web/H5 SDK 概述

# 概述

七鱼客服SDK可以帮助第三方网站及应用快速构建一个完善的客服系统生态圈，SDK提供较为完善的客服应用功能，及简洁的API接口。

web端接入 Demo，H5端接入 Demo，iOS原生接入Demo。

SDK提供如下扩展功能：

自定义接入

CRM对接

访客分流

商品链接

未读消息

VIP接入支持

常见问题设置

机器人欢迎语设置

网站访问统计

多机器人接入

以下所有接入代码的例子都可以在 这里下载。

对接七鱼WEB-SDK，只需要在页面上添加七鱼后台提供的接入代码即可，代码需要加在body中。访客访问页面时，页面会执行代码，加载七鱼的JS文件。

## API列表

API

API说明

ysf('onready', callback)

设置sdk初始化成功回调

ysf('config', options)

设置用户信息、分流信息

ysf('product', info)

设置商品链接

ysf('logoff')

用户登出

ysf('open', options)

打开客服聊天窗口

ysf('url')

获取打开聊天窗口的URL地址

ysf('onunread', callback)

设置未读消息通知回调

ysf('getUnreadMsg')

获取当前未读消息

ysf('getConversation', callback)

获取会话列表

ysf('onConversation', callback)

设置会话列表新消息通知回调，返回会话列表

ysf('onSessionMessage', callback)

设置会话列表新消息通知回调，返回当前收到的新的消息（单条消息）

## ysf('config', options)

配置企业及用户信息，嵌入七鱼客服SDK后可直接配置这些信息

输入参数说明：

参数

类型

描述

options

Object

企业配置信息

options.uid

String

企业当前登录用户标识，不传表示匿名用户

options.name

String

企业当前登录用户名称

options.email

String

企业当前登录用户邮箱

options.mobile

String

企业当前登录用户手机号

options.bid

String

当企业版本为平台版本的时候，用于指定咨询商户客服的商户id

options.data

String

企业当前登录用户其他信息，JSON字符串，其中 avatar 字段用于设置访客头像；需包含 key，value，label 等信息；如果是头像，还需要 href；具体设置见下示例代码

options.staffid

String

指定客服id，非必填；如果同时存在 groupid，staffid 优先级更高

options.groupid

String

指定客服组id，非必填

options.shuntId

String

访客选择多入口分流模版id，非必填

options.robotShuntSwitch

Number

机器人优先开关（访客分配）， 0 = 关闭，1 = 开启， 非必填

options.level

Number

企业当前登录用户vip等级，支持 1-10 级

options.qtype

Number

企业常见问题模板id，非必填

options.welcomeTemplateId

Number

机器人欢迎语模板id，非必填

options.success

Function

配置成功回调

options.error

Function

配置失败回调

options.connectYunxin

Boolean

配置sdk连接云信服务，限平台企业（在七鱼开头的是平台主账号子账号的方式）使用

options.wxworkAppId

String

配置企业微信id，用于语音功能，该字段需要进行加密，加密方式采用AES加密,加密代码参考如下实例，Aes Key为App Secret(系统管理-扩展与集成-开发者ID-AppSecret字段)，使用老的授权方式绑定的企业微信应用，需要在应用中自行设置可信域名:qiyukf.com

options.spkf

Number

主动发起视频邀请开关

options.isShowBack

Number

导航头是否显示返回按钮（浮层模式设置无效）0:不显示 1:显示 ；默认是 0 不显示

options.language

String

配置当前接入的语言，非必填，默认是 zh-cn 简体中文

options.shortcutTemplateId

Number

常见快捷入口模版id

options.layerSize

Object

PC端浮层模式窗口和输入框高度自定义设置，各项最小值： { width: 360, height: 500, inputHeight: 150 } 只有配置大于最小值才生效，否则默认最小值；可以配置一项或者多项

options.emojiPopoverWidth

string

指定pc端表情悬浮窗宽度

options.channelExtendInfo

string

渠道扩展信息，其中baiduQuery，baiduKeyword，baiduChannel是固定百度渠道相关的key

options.allowNewTab

Number

值为 1 时，图片、文本、PDF 文件支持在新标签页中打开；值为 0 时在当前浮层内直接打开。默认值为 0，非必填。

options.alipayImagePermissionText

string

支付宝相册权限文案

语言参数说明：

语言

字段

中文简体

zh-cn

中文繁体

zh-tw

英文

en

阿拉伯语（沙特）

ar

德语

de

俄语

ru

法语

fr

菲律宾语

tl

韩语

ko

日语

ja

泰语

th

印尼语

id

越南语

vi

意大利语

it

波兰语

 pl

使用范例
ysf('config', {
    uid:"1442286211167",
    name:'test',
    email:'test@163.com',
    mobile:'13888888888',
    data:JSON.stringify([
       {"key":"real_name", "value":"土豪"},
       {"key":"mobile_phone", "hidden":true, "value":"13800000000"},
       {"key":"email", "value":"13800000000@163.com"},
       {"key":"avatar", "label":"头像", "value": "https://ysf.qiyukf.net/operation/080659b993a45dd546fbd71efd5ef000"}, // 访客头像
       {"index":0, "key":"account", "label":"账号", "value":"zhangsan" , "href":"http://example.domain/user/zhangsan"},
       {"index":1, "key":"sex", "label":"性别", "value":"先生"},
       {"index":5, "key":"reg_date", "label":"注册日期", "value":"2015-11-16"},
       {"index":6, "key":"last_login", "label":"上次登录时间", "value":"2015-12-22 15:38:54"}
    ]),
    channelExtendInfo: JSON.stringify([
      {"key":"baiduQuery", "value":"百度搜索词"},
      {"key":"baiduKeyword", "value":"百度关键词"},
      {"key":"baiduChannel", "value":"百度来源"},
    ])
    staffid:'123',
    groupid: '123',
    shuntId: '123',
    level: 1,
    qtype: 1056,
    welcomeTemplateId: 1024,
    layerSize: { // 只对PC端的浮层模式有效
      width: 360,
      height: 500,
      inputHeight: 150
    },
    success: function(){
    	// todo
    },
    error: function(){
    	// handle error
    }
});
AES加密代码参考
public final class AESEncryptUtil {
    private static final ThreadLocal<Map<String, Cipher>> CIPHER_THREAD_LOCAL_EN_URS_COOKIE = new ThreadLocal<>();

    private static final ThreadLocal<Map<String, Cipher>> CIPHER_THREAD_LOCAL_DE_URS_COOKIE = new ThreadLocal<>();

    private AESEncryptUtil() {
    }

    public static String encrypt(String sSrc, String aesKey) {
        validate(aesKey);
        try {
            return encrypt_(sSrc, aesKey);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    public static String decrypt(String sSrc, String aesKey) {
        validate(aesKey);
        try {
            return decrypt_(sSrc, aesKey);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

	private static void validate(String aesKey) {
		if (strIsBlank(aesKey) || (16 != aesKey.length() && 24 != aesKey.length() && 32 != aesKey.length())) {
			throw new RuntimeException("AES密钥长度只能是 16 24 32 字节");
		}
	}

    /**
     * 加密
     *
     * @param sSrc
     * @return
     * @throws Exception
     */
    public static String encrypt_(String sSrc, String aesKey) throws Exception {
        if (strIsBlank(sSrc) || strIsBlank(aesKey)) {
            return null;
        }
        Map<String, Cipher> cipherMap = CIPHER_THREAD_LOCAL_EN_URS_COOKIE.get();

        if (cipherMap == null) {
            cipherMap = new HashMap<>(10);
            CIPHER_THREAD_LOCAL_EN_URS_COOKIE.set(cipherMap);
        }
        Cipher aesCipher = cipherMap.get(aesKey);
        if (aesCipher == null) {
            byte[] raw = aesKey.getBytes("utf-8");
            SecretKeySpec skeySpec = new SecretKeySpec(raw, "AES");
            aesCipher = Cipher.getInstance("AES");
            aesCipher.init(Cipher.ENCRYPT_MODE, skeySpec);
            cipherMap.put(aesKey, aesCipher);
        }
        return bytesToHexStr(aesCipher.doFinal(sSrc.getBytes("UTF-8")));
    }

    /**
     * 解密
     *
     * @param sSrc
     * @return
     * @throws Exception
     */
    public static String decrypt_(String sSrc, String aesKey) throws Exception {
        if (strIsBlank(sSrc) || strIsBlank(aesKey)) {
            return "";
        }

        Map<String, Cipher> cipherMap = CIPHER_THREAD_LOCAL_DE_URS_COOKIE.get();
        if (cipherMap == null) {
            cipherMap = new HashMap<>(10);
            CIPHER_THREAD_LOCAL_DE_URS_COOKIE.set(cipherMap);
        }
        Cipher aesCipher = cipherMap.get(aesKey);
        if (aesCipher == null) {
            byte[] raw = aesKey.getBytes("utf-8");
            SecretKeySpec skeySpec = new SecretKeySpec(raw, "AES");
            aesCipher = Cipher.getInstance("AES");
            aesCipher.init(Cipher.DECRYPT_MODE, skeySpec);
            cipherMap.put(aesKey, aesCipher);
        }
        return new String(aesCipher.doFinal(hexStrToBytes(sSrc)), "UTF-8");
    }

    /**
     * 将16进制字符串还原为字节数组.
     */
    private static byte[] hexStrToBytes(String s) {
        byte[] bytes;

        bytes = new byte[s.length() / 2];

        for (int i = 0; i < bytes.length; i++) {
            bytes[i] = (byte) Integer.parseInt(s.substring(2 * i, 2 * i + 2), 16);
        }

        return bytes;
    }

    /**
     * 将字节数组转换为16进制字符串的形式.
     */
    private static String bytesToHexStr(byte[] bcd) {
        StringBuffer s = new StringBuffer(bcd.length * 2);

        for (byte aBcd : bcd) {
            s.append(HEX_LOOKUP_STRING[(aBcd >>> 4) & 0x0f]);
            s.append(HEX_LOOKUP_STRING[aBcd & 0x0f]);
        }

        return s.toString();
    }

	private static boolean strIsBlank(CharSequence cs) {
		int strLen;
		if (cs != null && (strLen = cs.length()) != 0) {
			for (int i = 0; i < strLen; ++i) {
				if (!Character.isWhitespace(cs.charAt(i))) {
					return false;
				}
			}
			return true;
		} else {
			return true;
		}
	}

    private static final char[] HEX_LOOKUP_STRING = { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c',
            'd', 'e', 'f' };

    public static void main(String[] args) {
        String appId = "XXXXX"; // 企业微信appId
        String aeskey = "XXXX"; //app secret key
        System.out.println(AESEncryptUtil.encrypt(appId, aeskey));
    }
}


---

## 微信小程序

# 概述

此文档说明了微信小程序接入方式，以及小程序卡片功能。


---

## 微信小程序SDK

# 概述

网易七鱼微信小程序访客端SDK插件是一个微信小程序端客服系统访客解决方案，既包含了客服聊天逻辑管理，也提供了聊天界面，开发者可方便的将客服功能集成到自己的微信小程序中。


---

## 微信客服

# 概述

此文档说明了微信客服接入方式。


---

## 企业微信

# 概述

此文档说明了企业微信接入方式。


---

## 抖音企业号

# 接入说明

七鱼提供了抖音客服授权接入方式。

## 授权接入

七鱼提供抖音快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可。

## 场景

需要在七鱼系统中可以收发抖音企业号私信。

## 特别说明

根据抖音官方规定，同一个账号建议每30天重新进行授权绑定，否则会有自动解除绑定的风险。重新进行授权绑定的方式为：点击绑定抖音企业号按钮，重新进行扫码授权。

根据抖音官方规定，未相互关注的情况下，用户主动发送一条消息后，客服在48小时内最多回复3条消息

根据抖音官方规定，访客端只有发送纯文本，客服工作台才可收到，语音/图片/视频/表情包等消息类型客服工作台无法接收到

根据抖音官方规定，客服工作台只有发送文本、图片，访客端才可收到，语音/表情包/文件等消息类型访客无法接收到

## 绑定抖音企业号

抖音企业号接入绑定操作区域以及引导说明样例如下：

步骤一：进入【在线系统-》设置-》在线接入-》抖音企业号】，点击【绑定抖音企业号】按钮

步骤二：手机登录抖音企业号进行扫码授权（手机登录抖音企业号，点击扫一扫），或者直接在pc界面通过抖音企业号账号密码进行登录授权

*注意：扫码授权时，请确保账号的权限支持授权下列权限

若无下列权限，建议检查是否蓝v抖音企业号，如果已经是蓝v抖音企业号了，仍然找不到权限，建议联系抖音官方客服

七鱼-抖音V1版：

获取你的公开信息（头像、昵称、地区和性别）

获取你的私信列表

授权对象可以使用你的企业号私信能力

七鱼-抖音V2版：

获取用户公开信息（头像、昵称）

私信消息管理（接收和发送抖音私信消息）

上传图片工具

授权应用查询线索组件消息卡片信息

消息撤回管理（撤回私信、群聊消息）

获取你接收到的多媒体消息内容

 

完成绑定后，即可直接在七鱼收发私信消息，支持和其他渠道的访客统一接待，同时，从抖音来的访客，系统会展现抖音的来源标识

如您需要在对应渠道启用机器人接待，只需在对应渠道开启机器人即可。


---

## 百度营销

# 接入说明

七鱼提供了百度营销授权接入方式。

## 授权接入

七鱼提供百度营销快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可，同时该渠道支持机器人接待。

 受百度官方能力限制，百度营销需通过基木鱼建立投放落地页（包含咨询入口）方可采用以下方式授权，您可以在七鱼工作台统一接收和回复营销通咨询组件的消息。并且在七鱼查看会话信息时，可获知访客的【百度搜索词】、【百度关键词】、【百度流量渠道】等详细信息。

## 特别说明

受百度官方能力限制，如果您需要完全解绑，请务必在百度营销通后台也操作解绑，以避免出现潜在问题。

根据百度规定，会话结束后无法重新向客户发起会话。

根据百度规定，访客端只有发送文本、图片、表情，客服工作台才可收到，语音/图片/视频/表情包等消息类型客服工作台无法接收到

根据百度规定，客服工作台只有发送文本、图片，访客端才可收到，语音/表情包/文件等消息类型访客无法接收到

## 授权绑定百度营销

### 步骤一：点击立即绑定

进入【在线系统 > 设置 > 在线接入 > 百度营销】，点击【立即绑定】按钮

### 步骤二：登录百度营销通

进入 【咨询】>【通用咨询】>【客服账号授权】，点击【新增授权】。（https://yingxiaotong.baidu.com/)

### 步骤三：新增客服账号授权

选择第三方咨询工具，找到网易七鱼，点击进行授权绑定

### 步骤四：完成授权

输入七鱼管理员域名、账号和密码 完成授权

### 步骤五：查看绑定结果

绑定成功后可点击 “查看绑定结果” 或 在七鱼后台【主页】-【在线系统】-【设置】-【在线接入】-【百度营销】中查看到绑定的百度营销企业账号。

受百度限制，七鱼无法获取您百度营销通账号的企业名称，为了方便在数据报表里管理，强烈建议您在在线系统内已绑定的百度营销企业名称处编辑录入企业名称，以便查看和管理（不要求和百度侧一致，自行能识别即可）。

### 步骤六：建立接待方案

返回百度营销通，进入【咨询】>【通用咨询】>【客服账号授权】，找到已授权的【网易七鱼】，点击【去应用】；或也可以进入【咨询管理】，点击【新建咨询方案】，均可进入新建咨询方案页面。

在基础设置 > 设置客服账号，选择七鱼作为咨询接待客服账号，其余设置项根据自己需要进行设置。

若需要接入机器人接待，请在接待方案中选择七鱼机器人。

### 步骤七：接入会话测试

咨询方案设置完成后，点击【预览】即可测试会话接入七鱼。

### 步骤八：上线

受百度官方能力限制，完成会话接入测试后，真实访客咨询需要通过百度基木鱼（可在百度营销通一级菜单中进入），即 ： 1）已经通过百度基木鱼建立相关投放落地页站点； 2）落地页中在线咨询组件关联至前面设置的【咨询管理】-【咨询方案名称】 设置完成，在百度投放相关落地页后，访客能够通过基木鱼进行咨询，您可以在七鱼工作台统一接收和回复营销通咨询组件的消息。并且在七鱼查看会话信息时，可获知访客的【百度搜索词】、【百度关键词】、【百度流量渠道】等详细信息。

## 解除绑定百度营销

受百度官方能力限制，如果您需要完全解绑，请在七鱼后台解绑百度营销账号后，需要在百度营销通后台 咨询>通用咨询>客服账号授权 中删除之前设置的绑定七鱼应用，以避免出现潜在问题。

## 百度营销自建站接入

说明：进行百度营销自建站接入前，需先完成上文【授权绑定百度营销】流程中的步骤一至步骤五，完成百度营销通企业绑定。

### 步骤一：点击设置按钮

进入【在线系统 > 设置 > 在线接入 > 百度营销】页面，找到对应已授权绑定的百度营销通企业，点击自建站接入的设置按钮，进入自建站设置页面。

### 步骤二：自建站列表新增网站信息

在自建站列表中点击【新增网站信息】按钮，在弹出的网站信息弹框中填写网站名称，填写完成确定保存后系统会生成该网站ID编码。

### 步骤三：自建站代码配置

websdk 接入的需要增加以下配置
ysf('config', {
	baiduUcid: 'xx',  // 百度营销通企业 ID 必传；对应百度营销接入页面中的【百度营销通企业ID】
  	baiduSelfBuildSiteId: 'xx', // 百度自建站模式网站的 siteId  非必传；对应自建站设置页面的自建站列表中的【网站ID】
})
自建站接入除上述代码配置外，还要求接入页的 referrer 是百度域名的链接且包含 xst 参数才生效。

完成代码配置后，自建站即可接收访客进线咨询消息。

### 步骤四：线索转化回传设置

此功能可将自建站的线索转化事件回传给百度营销；若无线索回传需求，可不用配置。线索转化功能介绍可参考百度营销官网：线索API接入指南。

设置中需要配置token和线索转化回传事件：

①token：用于将线索转化事件回传给百度营销的接口中使用，多个百度营销账户可使用同一个token回传线索转化信息；

②线索转化回传事件：现在支持三种事件

有效咨询（开口）：当访客消息数大于0条时，回传事件有效咨询；需配置事件适用网站；

三句话咨询：当访客消息数大于等于3条时，回传事件三句话咨询；需配置事件适用网站；

留线索（服务小记）：服务小记中需配置所选的自定义字段，当会话服务小记保存时，若该字段有值则回传事件留线索；需配置自定义字段和事件适用网站。


---

## 小红书

# 授权接入

七鱼提供小红书企业专业号和KOS员工号快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可。接入后，可使用七鱼客服工作台统一接收与回复粉丝私信。

# 特别说明

小红书渠道的所有消息，都需要正常通过小红书平台社区私信风控、导流等审核规则。​在设置欢迎语等提示语时、机器人自动回复时建议不要进行数字英文组合，或携带超链接；

在聚光平台-线索管理中开启自动抓取留资，可以将用户对话过程中提到相应手机号、微信号码同步至七鱼客户信息。但请注意当客服和访客发的消息包含一些数字信息时，仍旧可能会有一部分误识别；

仅支持小红书企业号和KOS员工号私信，不支持小红书电商客服（其中私信也是进电商客服的情况也无法接入。）

若发现主页私信进的是电商客服，可以按照以下步骤调整（但是店铺客服还是在电商客服-千帆，无法接入）

# 接入前账号准备

首先请申请注册小红书企业专业号并完成专业号企业认证（即有企业认证蓝标打勾的），其他普通号和个人专业号均不适用；

并且使用该小红书主账号绑定的手机号+验证码登录小红书聚光平台，进行开户与推广资质认证

两者可以同步一起认证

# 授权接入小红书

## 步骤一：七鱼发起授权

进入【在线系统-》设置-》在线接入-》【小红书】，点击【绑定小红书企业专业号】按钮

## 步骤二：小红书专业号登录授权

跳转至小红书商业开放平台后，点击右上角「登录」，使用手机验证码或者小红书账号扫码登录。

即使用小红书专业号主账号=在聚光开户投放的小红书主账号 进行登录

登录后，选择授权范围，勾选授权功能：三方工具服务、私信留资数据，并点击下方【确认授权】。

①若只对接企业号，不对接KOS员工号；则授权范围选择：仅企业号；

②若对接企业号和KOS员工号；则授权范围选择：企业号及其绑定的员工号；

③若企业之前已绑定，想要更改授权范围，则点击七鱼侧小红书接入页面中的【绑定小红书企业专业号】按钮，进入授权认证页面，重新选择授权信息并确认授权即可。

## 步骤三：七鱼发起第二步授权

跳转回七鱼的绑定页面，再次点击【绑定聚光账号】按钮

## 步骤四：聚光平台登录授权

登录后，在顶部tab下点击「工具-三方客服管理」。

找到【网易七鱼】并点击【授权新账号】，点击前往绑定

在授权绑定页面使用七鱼超级管理员账号进行登录授权，完成后会回到上一个页面提示“绑定成功”

查看账号列表可以看到相应的企业号客服账号，点击【绑定员工号】，可查看该企业关联的所有员工号，勾选员工号后点击【确认授权】，可进行KOS员工号绑定授权

## 步骤五：确认授权成功

返回七鱼客服系统，并重新进入小红书渠道页面，小红书绑定状态正常，表示小红书与七鱼绑定成功，点击【查看员工账号】可查看员工账号绑定信息。

同时也可以通过配置相关的字段，获取小红书的广告留资信息字段。在聚光平台-线索管理中开启自动抓取留资，可以将用户对话过程中提到相应手机号、微信号码同步至七鱼客户信息。但请注意当客服和访客发的消息包含一些数字信息时，仍旧可能会有一部分误识别。

## 步骤六：会话接入

完成绑定后，即可直接在七鱼收发小红书私信消息，支持和其他渠道的访客统一接待，同时，从小红书来的访客，系统会展现小红书的来源标识。

私信来源主要是用户手机端直接找到企业号或KOS员工号主页点击私信进入，也可以通过聚光平台投流帖子的私信胶囊等入口进入；

# 发送小红书获客工具-服务卡/名片/落地页

## 步骤一：设置相应物料库

在小红书专业号平台和聚光平台后台，设置用户留资卡、商家名片、商家落地页

## 步骤二：设置获客工具侧边栏

打开七鱼后台【获客工具侧边栏】开关（默认关闭）

## 步骤三：发送获客工具卡片

客服工作台查看并进行发送在小红书后台「获客工具」中已经经过审核的服务卡、名片、落地页。

客服看到的是访客消息对应账号的获客工具信息，企业专业号与KOS员工号各不同。

由于小红书平台限制，客服无法看到具体名片号码内容，但是可以根据标题进行选择并发送。访客在小红书端私信内可以正常看到发送的名片号码信息。

 

客服发送留资卡片后，访客在小红书端私信内可以正常看到在小红书后台已经设置好的留资卡片表单信息，并可以自助进行填写

由于小红书平台限制，信息客服不会直接看到访客填写后的表单内容，但是用户填写的手机号、微信号信息等可以通过留资接口同步至客户信息；（因此客服只会看到已提交信息，并且自动发送信息补充卡，目前小红书接口并没有兼容补充卡片，因此如果展示一条暂不支持的消息类型属于正常现象）

## 备注：如何配置留资信息同步？

在聚光平台-线索管理中开启自动抓取留资

在七鱼-小红书接入-配置可以通过配置相关的字段，获取小红书的广告留资信息字段。

通过投流广告进入的用户，会同步相应投流计划的计划名称和创意名称，帮助客服识别用户更感兴趣的内容，辅助转化。

当用户在对话过程中提到相应手机号&微信号码，或者用户填写了客服发送的留资卡片信息，也会实时同步至七鱼客户信息。但请注意当客服和访客发的消息包含一些数字信息时，可能会有一部分误识别。


---

## Facebook

# 接入说明

七鱼提供了Facebook授权接入方式。

## 授权接入

七鱼提供Facebook快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可。

## 特别说明

​

根据Meta规定，距离访客最后一次发送消息超过7天，客服无法再回复，无法向客户发起会话。（若部分用户不同意在标准消息时间范围后接收您公共主页发送的消息，则消息回复时效在24h）

目前Facebook主页私信和帖子评论均可接入七鱼，且支持机器人接待Facebook渠道私信咨询！

## 接入前准备

​

首先请申请注册Facebook账号，并可以正常登录使用。

确定该账号下已创建需绑定的官方主页或已开通相应主页的管理权限。

如果还未创建公共主页，请前往Facebook创建。

## 授权绑定Facebook

步骤一：进入【在线系统-》设置-》在线接入-》Facebook】，点击【绑定你的Facebook主页】按钮

步骤二：在弹出的Facebook页面中登录相应的官方Facebook账号，完成登录后点击【Continue as …】进入下一步。

  

步骤三：进入页面选择页，在该Facebook账号下管理的主页中选择需要将私信接入七鱼的主页，点击【Next】。  

步骤四：选择打开七鱼申请管理的所有必要权限（所有权限均需要开启），点击【Done】。

  

步骤五：点击【OK】确认接入。

  

步骤六：完成后即会显示绑定成功，并在绑定列表中展示已绑定的Facebook主页。

  

完成绑定后，即可直接在七鱼收发Facebook主页私信消息，支持和其他渠道的访客统一接待，同时，从Facebook来的访客，系统会展现Facebook的来源标识

步骤七：点击账号配置，默认私信开启评论关闭，可以更改配置。

开启私信后，即可直接在七鱼收发Facebook主页私信消息，支持和其他渠道的访客统一接待，同时，从Facebook来的访客，系统会展现Facebook的来源标识；

开启主页评论后，Facebook绑定主页发布的帖子会生成一个工单。

可以对帖子工单直接进行回复，也可以看到外部用户在帖子下的评论，同时也可以自由回复用户评论内容

所有发送到Facebook的操作都需要勾选访客可见


---

## Line

# 接入说明

七鱼提供了Line绑定接入方式

## 授权接入

七鱼提供Line快速接入方案，只需管理员在七鱼后台和Line开发者后台按照提示绑定即可。

## 特别说明

仅支持绑定Line 官方号，若还未创建官方账号，请前往Line开发者平台创建。

若已绑定账号的secret和token发生改变，为保证消息正常接入和回复，请重新绑定。

因Line官方消息传递API的定价规则，单个账号每月能发送的消息数量有免费限额（500条），请自行前往Line 进行查看与升级。群聊消息的数量按消息发送给的群成员人数计算。（定价说明：https://developers.line.biz/en/docs/messaging-api/overview/#line-official-account-plan）

## 接入前准备

首先请申请注册Line个人账号或商用账号，并登录Line开发者控制台创建开发者帐户可以正常登录使用。

确定该账号下已创建需绑定的官方主页或已开通相应主页的管理权限。

## 绑定Line

步骤一：登录 Line 开发者控制台( https://developers.line.biz/en/ )，建立消息API频道（Create a Messaging API channel）。创建频道后，Line官方号管理平台（ https://manager.line.biz/ ）也同时会显示该频道。频道可以拥有多个。

 

如果已经在Line官方号管理平台创建official account并在使用当中，却发现开发者后台没有该channel，请在官方号管理平台找到该账号 > Settings >  Message API，点击Enable Message API按钮根据步骤即可同步至开发者后台。

步骤二：在Line开发者控制台获取Channel access token和Channel secret。 点击选择需要绑定的频道channel > Basic settings > 获得Channel secret， 同时在该频道 channel > Messaging API settings > 获得Channel access token (long-lived)

步骤三：进入【在线系统-》设置-》在线接入-》Line，点击【绑定你的Line官方号】按钮。 

步骤四：在弹出的绑定弹窗中填写刚获取的Channel access token和Channel secret，点击下一步。 

步骤五：确保已复制消息推送URL（服务器地址）后点击确定。完成后绑定账号会展示在列表中，但接入消息仍需要完成后面的操作。

步骤六：转到Line开发者控制台，选择频道channel >Messaging API settings > Webhook settings > 编辑Webhook URL，填写刚复制的消息推送URL并核实认证成功。

步骤七：在频道channel >Messaging API settings > Allow bot to join group chats  > Edit 点击编辑

步骤八：跳转Line官方号管理平台（ https://manager.line.biz/ ），进入Settings > Response settings，设置该频道响应模式（Response mode）为Bot，禁用自动响应（Auto-response），并开启Webhook。

步骤九：完成绑定接入。即可直接在七鱼收发Line官方号的私信消息，支持和其他渠道的访客统一接待，同时，从Line来的访客，系统会展现Line的来源标识；


---

## Discord

# 接入说明

七鱼提供了Discord授权接入方式。

## 授权接入

七鱼提供Discord快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可。

## 接入前准备

请确认拥有Discord账号，并创建你的服务器（https://discord.com/）

用该账号进入Discord开发者门户，并创建你的应用 （https://discord.com/developers/applications）

## 授权绑定Discord

步骤一：点击创建好的应用，进入Application 主页后点击【Bot】导航项进入 Bot 主页，点击右上角的【Add Bot】，在弹出的窗口中单击【Yes,do it!】，即可成功添加 Bot。

设定好Bot资料，这将成为服务器中的机器人成员

 

步骤二：请开启以下Bot权限

1.Authorization Flow中的【PUBLIC BOT】

2.Privileged Gateway Intents中的【PRESENCE INTENT】【SERVER MEMBERS INTENT】【MESSAGE CONTENT INTENT】

 

步骤三：单击左侧导航栏中的 OAuth2 > URL Generator，进行 OAuth2 授权配置。

1.SCOPES勾选【bot】

2.BOT PERMISSIONS 内勾选【Send message】【Send message in Threads】【Manage message】【Read Message/View Channels】（或直接勾选【 Administrator】）

3.复制GENERATED URL

 

步骤四：将bot授权给服务器

1.将 OAuth2 授权 URL 信息复制到浏览器并回车，根据引导登录 Discord。登录成功后选择链接 Bot 的服务器。

2.选择服务器后，点击【继续】按钮，为机器人授予相关的权限（查看信息、发送消息、管理消息、在子区内发送消息），并点击【授权】按钮进行确认。操作完成后可以看到授权成功的提示界面。

3.授权完成后，该Bot会放入刚才选择服务器的文本聊天室text channels中，单个服务器中所有的文本聊天室text channels都会拥有该机器人。

4.当不同服务器均需要进行集成设置时，每个服务器都需要 Bot 进行授权。

步骤五：获取Bot token

回到Bot页面，点击【Reset Token】，然后点击【Copy】机器人的token信息（请注意Bot token应保密，避免泄露导致机器人被恶意控制）

步骤六：绑定到七鱼

1.点击在线接入-Discord-绑定Bot

2.输入绑定刚才获取的Bot Token

 

七鱼显示绑定成功后，等待Discord平台生效，过程会需要十分钟左右的时间，请耐心等待后刷新服务器页面。当群聊中该机器人显示在线，即可接入七鱼会话状态。

## 会话接入

1.Discord 私信

进入 Discord 服务器，点击添加的 Bot 发送私信。七鱼后台会同步生成会话接入。

 

2.Discord 公屏群聊

进入 Discord 服务器，进入文本聊天室发送消息。七鱼后台会生成群会话接入，同步展示来源机器人+来源的服务器+来源的公屏群聊名+群成员列表。


---

## WhatsApp

# 接入说明

七鱼提供了WhatsApp接入方式。

## 特别说明

目前仅支持WhatsApp Official Business API Accounts的接入，即不支持手机app自主注册的「个人WhatsApp Messenger App账号」和「商家WhatsApp Business App账号」。

WhatsApp Official Business API Accounts接入需要企业申请Facebook开发者账号& Facebook Business Manager（FBM）平台账号并完成企业验证。

根据WhatsApp官方规定，若手机号已经在WhatsApp Messenger 或 WhatsApp Business App 中注册，要转用为API Accounts接入使用，需要注销并迁移，该过程相关好友号码&历史消息记录需要自行备份。

如果您在别的供应商下已经注册有WABA和认证sender，需要提前联系原供应商解除绑定，再联系网易七鱼的工作人员辅助您做迁移工作。

## 接入前准备

请企业确保已经完成申请Facebook开发者账号& Facebook Business Manager（FBM）平台账号，并完成FBM企业验证，这一步表示企业信息包括营业执照等已经通过Facebook团队的审核。

注册Facebook开发者账号：https://developers.facebook.com/

注册FBM账号：https://business.facebook.com/

如何完成FBM认证（Meta Business Help Center）：https://www.facebook.com/business/help/2058515294227817?id=180505742745347

## 接入说明

### 1. 提供注册WhatsApp企业账号的相关信息

确保您已经完成注册Facebook企业账号的注册和认证。

若您需要新建WhatsApp Business API Accounts（WABA）企业账户，请您根据以下表格内容提供给网易七鱼的工作人员，他们将会协助您开通相关企业账号的能力。表格中填写的内容将会是WhatsApp官方号账号信息页面展示信息。

注意：

根据WhatsApp官方规定，如果您在别的供应商下已经注册有WABA和认证sender，需要提前联系原供应商解除绑定，再联系网易七鱼的工作人员辅助您做迁移工作；

如果您的组织隶属于政府，您需要获得 WhatsApp 的额外批准。请联系网易七鱼的工作人员以获取指导。

序号

所需信息

1

FBM账号名称和ID（用户不可见）

此商业帐户将与您的 WhatsApp 商业帐户相关联，但您的客户不会在您的 WhatsApp 个人资料中看到此信息

2

新建WABA账号名称（非显示名，用户不可见）

3

时区，如（GMT-07:00）America / Los Angeles

4

企业显示名（用户可见）

显示名称请与企业名称匹配并符合 WhatsApp规范

审核之后任何新的显示名称更改都必须经过审核和批准才能使用

5

企业类别（用户可见）

6

（可选）企业介绍（用户可见）

7

注册可用的手机号码（区号）+号码

开通后用户能通过此号码找到您的WABA帐号；

电话号码必须是E.164 国际格式，不允许使用短代码；

保证在使用中的状态，在注册过程中将收到短信或 2FA 语音电话的所有权确认

不能使用WhatsApp Messenger 或 WhatsApp Business App 中注册的电话号码，若已经注册请更换号码或注销

8

上述手机号在注册过程中会收到6位验证码或2FA语音验证

### 2. 获得 WhatsApp 批准

网易七鱼的工作人员会提交相关账号申请，要获得对 WhatsApp Business API 的访问权限，您必须满足 WhatsApp 的要求。WhatsApp 将验证帐户和显示名称，并验证您的企业是否符合其 商业政策 和商务政策。如果您的企业合规，WhatsApp 将批准您的企业帐户使用。你可以在FBM后台实时查看审核状态。

### 3. 在七鱼绑定已经通过验证的手机号码

当完成WABA帐号申请并通过后，您可以在七鱼后台【在线系统-在线接入- WhatsApp】处绑定已经验证的号码。

完成绑定后，即可直接在七鱼收发WhatsApp私信消息，支持和其他渠道的访客统一接待，同时，从WhatsApp来的访客，系统会展现WhatsApp的来源标识；

### 4. 如何让用户找到WABA官方账号？

WhatsApp官方支持无需将电话号码保存在手机通讯录中即刻发起会话，因此您可以选择根据WABA电话号码生成「点击聊天短链接」，或者在WhatsApp后台生成「关联二维码」。

您可以将二维码和短链接放置于任何您对外的官网联系方式或社交媒体账号简介上，Facebook也支持在公共主页中添加WhatsApp按钮，让用户轻触按钮即可直接通过WhatsApp给您发消息，您也可以通过Facebook投放的广告直接把用户引导至WhatsApp。如此一来，客户无需输入电话号码，而只需使用移动设备的相机扫描二维码、点击联系按钮、点击或输入短链接即可发起对话。

## 可选的 - 申请小绿勾

如果您暂时不需要，请跳过此部分。

通过上述步骤创建WhatsApp商业帐号后已经可以正常使用了。另外，使用API的客户可以在满足 Facebook 的批准条件后可以申请绿色徽章。作为经过验证的发件人，让客户知道 Meta 已确保您的公司是真实且知名的商业帐号，从而使他们对您的品牌充满信心。目前只有拥有官方 Whatsapp Business API 帐户的企业才有资格提交 Green Badge 绿色徽章申请。

如果您需要申请小绿勾，请联系七鱼的工作人员。期间我们可能会需要您提供包括但不限于企业网站、企业背景、业务亮点、在地区的垂直排名和3则重要新闻网站对企业的报道文章等可证明企业真实且知名的材料。通常会在1周内通知审查结果。

此外，WhatsApp 对于 “绿勾” 的申请标准非常严格，通常只有小部分的企业可以成功申请。不过即便没有 “绿勾” ，作为 WhatsApp Business API 的用户还是可以享受所有 API 的进阶功能。

## 说明 - WhatsApp 商业帐户之间的差异

在 WhatsApp 上，有2种商业账户类型：非官方商业账户&还有以上说到的官方商业账户。

非官方商业帐户：商家无需营业执照即可在免费的WhatsApp Business产品上创建帐户。然而，他们访问的智能功能并不是那么先进。聊天页面只向客户显示他们的电话号码，且公司名称在业务资料中以较小的字体显示。

官方商业账户：这些大多是 WhatsApp 认证的知名品牌的 API 商业账户。即使客户尚未将企业添加到他们的通讯录中，他们也可以查看他们的身份。只有拥有官方 Whatsapp Business API 帐户的企业才有资格提交 Green Badge 申请。

Personal WhatsApp 

WhatsApp Business 

WhatsApp Business API 

通讯方式&消息存储

手机app下载

手机app下载

存储在七鱼系统内，可以在任何时间任何地点任何团队成员可访问

团队管理

❌

❌

✅ 通过七鱼会话可以分配给不同的团队伙伴

在线机器人自动回复

❌

❌

✅

申请

免费下载

免费下载

企业号必须通过Meta伙伴注册，无法自主申请，必须使用WhatsApp Business API

企业描述

❌

✅

✅

标签

❌

✅

✅

自动打标签

❌

❌

✅

快速回复

❌

✅

✅

问候消息和离开消息

❌

✅

✅

配置人工自动回复消息&智能辅助回答

❌

❌

✅

企业官方认证（绿勾）

❌

❌ May lose the account

✅


---

## Twitter

# 接入说明

七鱼提供了Twitter授权接入方式。

## 授权接入

七鱼提供Twitter快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可。

## 特别说明

根据Twitter规定，距离访客最后一次发送消息超过24小时，客服无法再回复，无法向客户发起会话。

目前支持Twitter私信接入七鱼，暂时不支持机器人接待Twitter渠道咨询，功能即将上线，敬请期待！

## 接入前准备

首先请申请注册Twitter账号，并可以正常登录使用。

## 授权绑定Twitter

步骤一：进入【在线系统 > 设置 > 在线接入 > Twitter】，点击【绑定你的Twitter账号】按钮

步骤二：在弹出的Twitter登录页面中登录相应的Twitter账号，完成登录后点击【授权应用程序】进入下一步。

步骤三：完成后即会显示绑定成功，并在绑定列表中展示已绑定的Twitter账号。

完成绑定后，即可直接在七鱼收发Twitter私信消息，支持和其他渠道的访客统一接待，同时，从Twitter来的访客，系统会展现Twitter来源标识；


---

## Instagram

# 接入说明

七鱼提供了Instagram授权接入方式。

## 授权接入

七鱼提供Instagram快速接入方案，只需管理员在七鱼后台按照提示绑定授权即可。

## 特别说明

​

根据Meta官方政策规定，当距离用户最后一次发送消息超过7天，则客服无法再在七鱼回复用户，需要前往Instagram回复用户（若部分用户不同意在标准消息时间范围后接收您公共主页发送的消息，则消息回复时效在24h）

目前支持Instagram私信接入七鱼，使用七鱼统一接收查看和回复。

仅支持绑定Instagram Professional帐户，且需要通过 Facebook 帐户间接访问，因此您需要拥有 Facebook 帐户并在Facebook主页-设置-绑定该Instagram账户

## 接入前准备

​

首先请申请注册Facebook账号并创建需绑定的官方主页，并可以正常登录使用。

如果还未创建 Facebook账号或公共主页，请前往Facebook创建。

## 授权绑定Instagram

步骤一：确认你的Instagram账号为Professional专业帐户

若目前您的账户还不是Professional专业帐户，请在ins个人中心-设置-切换为专业账户

根据流程进行信息的填写，完成后会显示Instagram Business Account已经切换成功

步骤二：在您的Facebook官方主页绑定您的Instagram专业账号

若还没有Facebook账号和主页，请前往Facebook创建。 在Facebook Page - Page Settings - Instagram，点击按钮Connect Account，登录您上面的ins专业账号后绑定成功

步骤三：进入七鱼绑定 Instagram账号

进入【在线系统-》设置-》在线接入-》Instagram，点击【绑定Instagram专业账户】按钮

在弹出的Facebook登录页面中登录刚才绑定的Facebook账号，完成后点击下一步

进入Instagram账户选择页，选择您刚绑定的Instagram专业账户，点击下一步

选择打开七鱼申请管理的所有必要权限（获取邮箱地址、在 Messenger 中管理和访问主页对话、显示你管理的主页列表），点击【Done】。

点击【OK】确认接入。

完成后即会显示绑定成功，并在绑定列表中展示已绑定的Instagram专业账户。

完成绑定后，即可直接在七鱼收发Instagram私信消息，支持和其他渠道的访客统一接待，同时，Instagram来的访客，系统会展现Instagram的来源标识；


---

## 钉钉

# 接入说明

此文档说明了钉钉接入方式。

## 接入方式

七鱼提供钉钉自建应用接入方案，只需管理员在七鱼后台按照提示绑定应用相关信息即可。

## 接入前准备

请准备钉钉管理员权限账号

确保钉钉企业状态正常

## 员工服务

### 自建应用接入钉钉

步骤一：获取CorpId：登录钉钉开发者后台 ,将企业 CorpId 回填入七鱼，如图1、图2。

图1:获取企业CorpId

图2:将获取到的企业CorpId回填七鱼

步骤二：创建企业内部应用：点击【创建应用】按钮，在创建应用弹框中根据自身需求填写应用信息（例：应用名称为HR服务中心），填写完毕后创建应用，如图3，图4。

图3:创建企业内部应用

图4:填写应用信息

步骤三：回填应用信息：点击创建成功的应用，获取应用信息，将AgentId、AppKey、AppSecret回填进七鱼对应字段内，如图5、图6。

图5:获取应用信息

图6:将应用信息回填七鱼

步骤四：回填七鱼应用链接：将七鱼生成的应用首页地址填入钉钉应用的应用首页地址&PC首页地址，如图7、图8。

图7:复制七鱼首页链接

图8:应用配置信息

步骤五：应用授权：点击权限管理，按需勾选应用权限，建议选择以下4个权限方便进行后续身份认证和业务进行，参考图9（七鱼不会泄漏和主动存储您的员工信息）：

​

企业员工手机号信息

邮箱等个人信息

通讯录部门读权限

成员信息读权限

图9:勾选应用权限信息

步骤六：配置字段映射：将钉钉内员工信息字段与七鱼客户信息字段建立映射关系，可自动完成身份识别，方便后续进行会话路由分配、机器人问题回复、工单协同等业务使用。如图10:

图10:配置映射字段

步骤七：发布应用：如图11:

图11:发布应用

## 跨团队协作

### 自建应用接入钉钉

步骤一：获取CorpId：登录钉钉开发者后台 ,将企业 CorpId 回填入七鱼，如图1、图2。

图1:获取企业CorpId

图2:将获取到的企业CorpId回填七鱼

步骤二：创建企业内部应用：点击【创建应用】按钮，在创建应用弹框中根据自身需求填写应用信息（例：应用名称为七鱼轻工单），填写完毕后创建应用，如图3。

图3:创建企业内部应用

 步骤三：回填应用信息：点击创建成功的应用，获取应用信息，将AgentId、AppKey、AppSecret、RobotCode回填进七鱼对应字段内，如图4、图5。

图4-1:填写应用信息

图4-2:填写应用信息

图5:填写应用信息

步骤四：回填七鱼应用链接：保存第一步内容后，进入第二步。将七鱼生成的应用首页地址填入钉钉应用的应用首页地址&PC首页地址，和回调地址如图6、图7。

图6:复制七鱼首页链接

图7-1:编辑应用信息

图7-2:编辑应用信息

图7-3:编辑应用信息

步骤五：应用授权：点击权限管理，勾选应用权限，开通以下权限方便进行后续身份认证和业务进行，参考图8（七鱼不会泄漏和主动存储您的员工信息），：

​

个人手机号信息

通讯录个人信息读权限

通讯录部门信息读权限

成员信息读权限

通讯录部门成员读权限

企业内机器人发送消息权限

图8:勾选应用权限信息

步骤六：发布应用：完成应用发布，发布后根据需要调整应用可见权限。如图9、图10。

图9:发布应用

图10:发布应用

步骤七：同步员工：在七鱼系统中同步钉钉员工信息到【外部员工管理】，可以使钉钉员工在不登录网易七鱼账号密码的情况下即可直接在钉钉中查看及处理工单。如图11。

图11:同步员工

步骤八：授权im工单权限：在七鱼【员工管理-外部员工管理】中为钉钉员工开启im工单权限，如图12。

图12:授权im工单权限


---

## 飞书

# 接入说明

七鱼提供了飞书客服授权接入方式。

## 接入方式

七鱼提供飞书自建应用接入方案，只需管理员在七鱼后台按照提示绑定应用相关信息即可。

## 接入前准备​

请准备飞书管理员权限账号

确保飞书企业状态正常

## 员工服务

### 自建应用接入飞书

步骤一：创建企业内部应用，登录 飞书开发者后台，选择【创建企业自建应用】，选择类型为【企业自建应用】其它信息请根据自身需求填写（例：应用名称为HR服务中心），填写完毕后创建应用，如图1、图2。

图1:创建应用

图2:创建应用并选择应用类型

步骤二：回填应用信息：点击创建成功的应用，获取应用信息，并上传应用头像，随后将App ID、App Secret回填进七鱼对应字段内，如图3、图4。

图3:获取应用信息

图4:将应用信息回填七鱼

步骤三：回填七鱼应用链接：将七鱼生成的应用首页地址，填入应用的桌面端首页&移动端首页，如图5、图6。

图5:复制七鱼首页链接

图6:回填应用信息

步骤四：应用授权：点击权限管理，按需勾选应用权限，建议选择以下5个权限方便进行后续身份认证和业务进行，参考图7（七鱼不会泄漏和主动存储您的员工信息）：

​

获取用户邮箱信息

获取用户基本信息

获取部门基础信息

获取用户手机号

以应用身份读取通讯录

图7:勾选应用权限信息

步骤五：配置字段映射：将飞书内员工信息字段与七鱼客户信息字段建立映射关系，可自动完成身份识别，方便后续进行会话路由分配、机器人问题回复、工单协同等业务使用。如图8:

图8:配置映射字段

步骤六：创建版本发布：版本号建议1.0.0，根据需求选择应用员工可见范围（无特殊情况建议全员），随后确认发布，如图9:

图9:发布应用

## 跨团队协作

### 自建应用接入飞书

#### 创建企业内部应用：

登录飞书开发者后台（https://open.feishu.cn/app) ，选择【创建企业自建应用】，选择类型为【企业自建应用】其它信息请根据自身需求填写（例：应用名称为网易七鱼轻工单），填写完毕后创建应用，如图1、图2

图1：创建应用

图2：创建应用并选择应用类型

#### 回填应用信息：

点击创建成功的应用，获取应用信息，并上传应用头像，随后将App ID、App Secret回填进七鱼对应字段内，如图3、图4

图3：获取应用信息

图4：将应用信息回填七鱼

#### 回填七鱼应用链接：

将七鱼生成的首页链接、安全设置重定向地址、机器人-我受理工单地址、机器人-我完结工单地址，填入应用设置的对应位置中，如图5、图6

图5：复制七鱼链接

图6-1：回填应用信息-回填首页链接

图6-2：回填应用信息-回填安全设置重定向地址

图6-3：回填应用信息-启用机器人，并回填【机器人-我受理工单地址】和【机器人-我完结工单地址】

#### 应用授权：

点击权限管理，勾选应用权限，开通以下权限方便进行后续身份认证和业务进行，参考图7（七鱼不会泄漏和主动存储您的员工信息）：

​

获取用户基本信息

获取部门基础信息

获取个人手机号信息

获取应用信息

获取与发送单聊、群组消息

获取通讯录基本信息

获取通讯录部门组织架构信息

#### 创建版本发布：

版本号建议1.0.0，根据需求选择应用员工可见范围（无特殊情况建议全员），随后确认发布，如图8

图8：发布应用

#### 应用审核：

飞书管理员在飞书管理后台进行应用上线审批，如图9

飞书管理后台

图9：飞书管理员在飞书管理后台进行应用上线审批

#### 同步员工：

应用审核通过后，在七鱼系统中同步飞书员工信息到【外部员工管理】，可以使飞书员工在不登录网易七鱼账号密码的情况下即可直接在飞书中查看及处理工单。如图10:

图10：应用完成后，同步员工

#### 授权im工单权限：

在七鱼【员工管理-外部员工管理】中为飞书员工开启im工单权限，如图11、图12

图11:为飞书员工开启im工单权限

图12:即可使用


---

## 服务端API通用说明

# 通用说明

七鱼提供了丰富的API接口，方便企业快速使用七鱼，同时也可以根据自己的业务场景，进行一定程度的个性化开发。

## 数据校验

七鱼服务器和您的服务器进行数据传递时，POST请求会带以下这些参数：

参数

参数类型

参数说明

是否必填

appKey

String

你的企业的appKey (仅在您的服务器向七鱼服务器发送数据时需要，七鱼服务器向您的服务器发送数据时无此参数)

是

time

Long

当前 UTC 时间戳，从 1970 年 1 月 1 日 0 点 0 分 0 秒开始到现在的秒数

是

checksum

String

SHA1(appSecret + md5 + time), 三个参数拼接的字符串，进行SHA1哈希计算，转化成16进制字符(String，小写)

是

其中，checksum 用于校验数据的完整性，其计算规则中，各字段取值如下

appsecret 可在七鱼管理后台->「系统」->「扩展与集成」页面找到

md5 为整个请求 json 字符串的 md5 哈希值（小写形式），即 md5 = MD5(content).toLowerCase()，如果是上传文件，则是整个文件内容的 md5，

time 就是请求参数中的 time

特殊说明：出于安全性考虑，每个 checksum 的有效期为 5 分钟，请确认发起请求的服务器是与标准时间同步的，比如有NTP服务。

以下为参考代码

### checksum 的Java 示例代码：
 public class QiyuPushCheckSum {

    private static final char[] HEX_DIGITS = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};

    public static String encode(String appSecret, String md5, long time) {
        String content = appSecret + md5 + time;
        try {
            MessageDigest messageDigest = MessageDigest.getInstance("sha1");
            messageDigest.update(content.getBytes());
            return getFormattedText(messageDigest.digest());
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    private static String getFormattedText(byte[] bytes) {
        int len = bytes.length;
        StringBuilder buf = new StringBuilder(len * 2);
        for (int j = 0; j < len; j++) {
            buf.append(HEX_DIGITS[(bytes[j] >> 4) & 0x0f]);
            buf.append(HEX_DIGITS[bytes[j] & 0x0f]);
        }
        return buf.toString();
    }
}

### MD5计算Java代码示例：
public class MD5 {	
    private final static char[] hexDigits = { '0', '1', '2', '3', '4', '5',
            '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F' };
  
    public static String md5(String input) {
        return md5(input, 32);
    }

    public static String md5(String input, int bit) {
        try {
            MessageDigest md = MessageDigest.getInstance(System.getProperty(
                    "MD5.algorithm", "MD5"));
            if (bit == 16)
                return bytesToHex(md.digest(input.getBytes("utf-8")))
                        .substring(8, 24);
            if (bit == 28) {
                return bytesToHex(md.digest(input.getBytes("utf-8")))
                        .substring(2, 30);
            }
            return bytesToHex(md.digest(input.getBytes("utf-8")));
        } catch (NoSuchAlgorithmException | UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        return null;
    }

	private static String bytesToHex(byte[] bytes) {
        StringBuffer sb = new StringBuffer();
        int t;
        for (int i = 0; i < 16; i++) {
            t = bytes[i];
            if (t < 0)
                t += 256;
            sb.append(hexDigits[(t >>> 4)]);
            sb.append(hexDigits[(t % 16)]);
        }
        return sb.toString();
    }
}

### 请求七鱼openapi接口Java代码示例：
	public static void main(String[] args) {
        //请求内容，用来放到body体中
        String content = "{\"name\":\"qiyu\",\"age\":\"8\"}";
        //对content进行md5哈希，然后转化为小写形式
        String md5 = MD5.md5(content).toLowerCase();
        //取时间戳，注意是秒，不是毫秒
        long time = System.currentTimeMillis() / 1000;
        String appkey = "appkey";
        String secret = "secret";
        //生成checksum
        String checksum = QiyuPushCheckSum.encode(secret, md5, time);
        //拼接请求url
        String url = "https://qiyukf.com/xxx";
        url += "?appKey=" + appkey;
        url += "&time=" + time;
        url += "&checksum=" + checksum;
        //发送POST请求，content放到body体中
        String res = HttpClientPool.getInstance().postMethod(url, content);
    }

## 服务端登录接口

服务端登录功能可以达到七鱼免登的效果，用于将七鱼客服端作为工具条嵌入第三方网站或者实现第三方认证之后对七鱼进行免登。 功能使用步骤拆解:
 步骤1: 向自己服务器端发起异步请求，获取`SDK`地址；
 步骤2: 将SDK地址作为Iframe工具条嵌入第三方页面或者直接跳转新页签实现免登

### 获取SDK接入地址

第三方向七鱼请求SDK接入地址的时候，七鱼会执行客服在七鱼的登录事件，并生成含动态口令的SDK接入地址返回给第三方

接口调用及数据校验规则参考上面数据校验

### curl请求样例如下：
curl -X POST \
  'https://qiyukf.com/openapi/staff/login?appKey=1c088a89de51d0af457616605f28390f&checksum=f570a5eb049eb803b086e45829b07e48&time=1511832531' \
  -H 'content-type: application/json;charset=utf-8' \
  -d '{"staffName": "admin"}'

### 请求参数

#### Query

参数

是否必填

参数类型

说明

appKey

String

是

企业appkey

time

Integer

是

当前 UTC 时间戳，从 1970 年 1 月 1 日 0 点 0 分 0 秒开始到现在的秒数

checksum

String

是

SHA1(appSecret + md5 + time), 三个参数拼接的字符串，进行SHA1哈希计算，转化成16进制字符(String，小写)

#### Body

参数

是否必填

参数类型

说明

staffName

是

String

客服在七鱼这边对应的客服名(此处对应的参数值为客服账号)

### 响应参数

接口返回数据为JSON格式，最外层参数如下：

参数

参数说明

code

错误码。200表示设置成功。

result

code为200时，返回值有效。

message

请求错误时，填错误提示信息

### 返回示例

如果code等于200表示客服登录成功，result内容为json数据，返回样例如下：
{
    "code": 200,
    "message": "",
    "result": {
        "sdk_url": "https://xxx.qiyukf.com/toolbar/script/get?token=cb1ec3c43089493eb4039945685ebf51",
        "corp_code": "xxx",
        "token": "cb1ec3c43089493eb4039945685ebf51"
    }
}

result内的参数说明如下：

参数

数据类型

参数说明

sdk_url

String

生成的含动态登录口令的SDK接入地址

corp_code

String

登录客服对应的七鱼企业域名

token

String

动态登录口令

token失效时间和长连接心跳有关系，心跳超时时间270秒(4.5分钟)

重要提示: 本文档中提供的所有接口均面向开发者服务器端调用，用于计算 checksum 的 AppSecret 开发者应妥善保管,可在应用的服务器端存储和使用，但不应存储或传递到客户端，也不应在网页等前端代码中嵌入。

## 通用错误码列表

错误码

含义

200

OK

14001

参数 appKey 错误

14002

参数 checksum 错误，请重新检查 checksum 计算方式

14003

参数 time 错误，请确保 time 参数与当前时间差小于 5 分钟，或检查服务的时间是否与标准时间同步

14004

内容格式校验错误，请参加详细错误信息，并对照对应的接口文档修正

14008

请求来源 IP 不在被允许的范围内

14009

请求频率过快，超出接口限制

14500

服务器内部错误，请反馈给我们具体场景

14501

数据量过大，超出接口限制

14515

权限不足

备注：如果Java应用调用服七鱼服务端接口出现证书报错问题，请升级jdk版本至1.8_202及以上。

## 接口请求频率 

并发限制规则：

接口级限流：默认情况下，单个接口的请求速率限制为 5 QPS。

租户级限流：默认情况下，单个租户（Tenant）在所有接口上的聚合请求速率限制为 20 QPS。

特殊说明：部分接口可能拥有独立的、高于或低于此默认值的限流策略，请以具体接口文档为准。
