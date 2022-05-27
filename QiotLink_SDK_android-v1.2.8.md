

## Android开发文档

### QIotLink SDK简介

QIotLinkSDK提供设备的配网(需集成快联SDK)、视频开流，视频卡录、视频通话、事件订阅功能，可以帮助APP开发者快速接入IOT设备。同时屏蔽了物联网设备认证和视频类设备认证的复杂流程，向开发者提供简单，统一的解决方案

### 版本更新日志

- v1.2.0 新增 -通过token绑定设备

```java
 /**
     * 通过token绑定设备
     */
    public void bindDeviceWithWithToken(String token, MyCallBack myCallBack) {
      
    }
```

- v1.2.4 修改startRTC，参数外漏

  ```java
   AudioParamLocal audioParamIn = new AudioParamLocal(Constants.QHVC_PACKET_TYPE_AAC, 8000, 16, 1);
                      AudioParamLocal audioParamOut = new AudioParamLocal(Constants.QHVC_PACKET_TYPE_PCM_S16LE, 16000, 16, 1);
                      QVSSDKManager.getInstance().startRTC(mSessionId,audioParamIn,audioParamOut, new QVSRTCCallbackAdapter() {
                          @Override
                          public void onStartSuccess(@NotNull String sessionId) {
                            
                          }
                      });
  ```

- v1.2.5 update 帝视 to "3.0.9.22032301"

  ```java
  修改方法downloadRecordVideo
  ```

- v1.2.6 帝视升级 到v 3.1.1.22040801

- v1.2.7 

  ```java
  // 1.createSession 新增 option 参数，此参数主要是开流或者播放时，一些配置性参数如
    Map<String ,Object> options = new HashMap<>();
  options.put(IQHVCPlayerAdvanced.KEY_OPTION_RENDER_MODE,IQHVCPlayerAdvanced.RENDER_MODE_FULL);
  ```

- v1.2.8

  ```
  分享相关api
  ```

### 分享(V1.2.8)

#### 1.创建手机分享

```java
/**
* 操作类：QilManager ->DeviceMonitor
  * @param product_key    string	必须	产品唯一标识
     * @param device_name	string	必须	设备唯一标识
     * @param mobile	string	必须	手机
     * @param level	int	非必须	级别， 1，2，3， 默认是1
     * @param myCallBack
*/
void modifyDeviceTitleWithProductkey(String product_key, String device_name, String mobile, @Nullable Integer level, MyCallBack myCallBack)
```

#### 2.创建邮箱分享

```java
/**
* 操作类：QilManager ->DeviceMonitor
     * @param product_key	string	必须	产品唯一标识
     * @param device_name	string	必须	设备唯一标识
     * @param email	string	必须	邮箱
     * @param level	int	非必须	级别， 1，2，3， 默认是1
     * @param myCallBack
*/
void createEmailDeviceShare(String product_key, String device_name, String email, @Nullable Integer level, MyCallBack myCallBack)
```

#### 3.分享列表

```java
/**
* 操作类：QilManager ->DeviceMonitor
    * product_key	string	非必须	产品唯一标识 如果不使用devices获取此参数必须
     * device_name	string	非必须	设备唯一标识 如果不使用devices获取此参数必须
     * type_id	int	非必须	查询类型id，1 单设备查询(使用pk+dn)，2 多设备查询(使用devices) 默认是1
     * devices	JsonArray类型的string	非必须	设备s的json字符串，如果使用此字段， type_id必传2
     * @param myCallBack
*/
void getDeviceShareList(Map<String, String> options, MyCallBack myCallBack)
```

#### 4.取消分享

```java
/**
* 操作类：QilManager ->DeviceMonitor
  * @param id	string	必须	待接受列表的分享id
     * @param status	int	必须	状态 1待接受，2分享中
     * @param myCallBack
*/
void cancelDeviceShare(String id, String status, MyCallBack myCallBack)
```

#### 5.接受者获取待接受列表

```java
/**
* 操作类：QilManager ->DeviceMonitor
  无参数
  * @param myCallBack
*/
void modifyDeviceTitleWithProductkey(String product_key, String device_name, String mobile, @Nullable Integer level, MyCallBack myCallBack)
```

#### 6.同意接受分享

```java
/**
* 操作类：QVLManager ->DeviceMonitor
  同意接受分享V2
*/
void agreeAcceptDeviceShare(String id, MyCallBack myCallBack)
```

#### 7.拒绝接受分享

```java
/**
* 操作类：QilManager ->DeviceMonitor
 * @param  id    string	必须	分享id
*/
void refuseAcceptDeviceShare(String id, MyCallBack myCallBack)
```




### 开发环境

- Android minSdkVersion 19

### 集成说明

#### 1-获取SDK

联系客服获取SDK

#### 2-sdk文件清单

- qiotlink_xxx.aar
-  cloud_logger_xxx.aar,
- local_logger_xxx.aar, 
- qh-soundbird.aar,
- qh-xbar-sdk-xxx.aar
- qhvc_webrtc_xxx.aar
- Android_SDK_FastConnect_xxx.aar

将下载下来的SDK放入libs目录下

#### 3-依赖配置

修改module的build.gradle文件

```
 dependencies {
 //有快联包时，以下四个包需要compileOnly	
      compileOnly(name: 'cloud_logger_v1.0.0_20201118_02', ext: 'aar')
        compileOnly(name: 'local_logger_v1.0.0_20201118_02', ext: 'aar')
        compileOnly(name: 'qh-soundbird-sdk-release-0.3.2', ext: 'aar')
        compileOnly(name: 'qh-xbar-sdk-1.0-release', ext: 'aar')
        implementation(name: 'qhvc_webrtc', ext: 'aar')
        implementation(name: 'rule-view-debug', ext: 'aar')
        implementation(name: 'qiotlink-v1.2.6', ext: 'aar')
         implementation(name: 'FastConnect_V1.4.6.8_721f65eb', ext: 'aar')
 }

```

#### 4-混淆配置

```
-keep class com.qiho.qiotlink.** { *; }
-keep class com.qiho.livecloud.tools.** { *; }
-keep class com.qihoo.cloud.logger.** { *; }
-keep class com.qihoo.local.logger.** { *; }
-keep class com.qihoo.soundbird.** { *; }
-keep class com.qihoo.xbar.encoder.** { *; }
-keep class org.webrtc.** { *; }
-keep class com.qihoo.videocloud** { *; }
-keep class net.qihoo.videocloud** { *; }
-keep class com.qiho.fastjson** { *; }
-keep class org.bouncycastle** { *; }
-keep class com.qihoo.mqtt** { *; }
-keep class com.qihoo.sdk.report** { *; }
-keep class com.qihoo.livecloud.commoneffect** { *; }
-keep class com.qihoo.videocloud.gateway** { *; }
-keep class com.onething.xyvod** { *; }
-keep class com.yunfan.net** { *; }
-keep class com.qihoo.gongxiangyun.pcdn** { *; }
-keep class com.qihoo.videocloud.p2p** { *; }
-keep class com.getkeepsafe.relinker.livecloud** { *; }
-keep class com.gpsoft** { *; }
-keep class com.qihoo.livecloud** { *; }
-keep class com.qihoo.videocloud.rtc** { *; }
-keep class com.qihoo** { *; }
```

#### 5-添加权限

```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.BIND_JOB_SERVICE" />
<uses-permission android:name="android.permission.GET_ACCOUNTS" />
<uses-permission android:name="android.permission.READ_SYNC_SETTINGS" />
<uses-permission android:name="android.permission.WRITE_SYNC_SETTINGS" />
<uses-permission android:name="android.permission.AUTHENTICATE_ACCOUNTS" />
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.ACCESS_NOTIFICATION_POLICY" />
<uses-permission android:name="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
<uses-permission android:name="android.permission.USE_CREDENTIALS" />
<uses-permission android:name="android.permission.READ_LOGS"/>
```

### 交互流程

#### 云云对接获取Token

![](https://tva1.sinaimg.cn/large/008i3skNly1gu230h2l9gj60re0f3aaw02.jpg)

#### 设备控制流程

![](https://tva1.sinaimg.cn/large/008i3skNly1gu2335nv9pj60rd0lg40402.jpg)

#### 云存及开流流程

![](https://tva1.sinaimg.cn/large/008i3skNly1gu23511ouyj60tu0lc0uf02.jpg)

### SDK初始化

#### 使用前初始化

推荐在Application的onCreate中初始化

```
//是否开启日志
QilManager.getInstance().setCanLog(true, ElogLevelType.LOG_LEVEL_WARN);
 //设置登录方式,详情见登录方式
 QilManager.getInstance().setQType(QType.TOKEN);
// appKey 和 appSecret 需要向360大圣云平台申请
QilManager.getInstance().init(this, "you_appKey", "you_appSecret");
        
//SDK在使用前必须要setToken,如果登录态有效可以初始化后直接setToken
if(haveToken){
   String token = "user_token";
   QilManager.getInstance().setToken(token);
   }
```

#### 登录方式

- QT登录

 App账号为360体系账号，登录方式设置为QType.QT时使用的是360用户中心的SDK登录,开发者需要自行集成用户中心SDK。通过用户中心SDK登录成功后拿到QT再调用loginGetSidWithQTModeCompletionCallback方法获取token，示例代码如下:

```
    //通过QT的方式登录成功了，使用QT
    private void setTokenWithQt(String q,String t){
        QilManager.getInstance().loginGetSidWithQTModeCompletionCallback(q,t, new GetSidCallBack() {
                    @Override
                    public void onSuccess(String sid, String token, String push_alias) {
                        // 设置Token
                        QilManager.getInstance().setToken(token);
                    }

                    @Override
                    public void onError(String error) {

                    }
                });
    }
```

- TOKEN登录

 第三方客户自有App获取Token需要与360智慧生活云云对接进行鉴权绑定，接入用户从自己的服务器获取token。参考云云对接获取Token

### 基础功能Api简介

#### 1.设置SDK Token (用户登录态)

SDK必须设置Token后才能对用户设备进行操作

```

 /**
 * token 用户登录态，参考登录方式获取
 */
QilManager.getInstance().setToken(String token);
```

#### 2.绑定设备

当设备配网成功后，调用该接口绑定设备和用户的关系

```

 /**
 * MyCallBack 回调
 * BindMessageParameter 入参数据结构
 * BindMessageParameter bindParameter = new BindMessageParameter();
 * bindParameter.setProduct_key("device_pk");设备ProductKey  必传
 * bindParameter.setDevice_name("device_dn");设备DeviceName  必传
 * bindParameter.setToken("token"); token 必传，快链sdk获取到的token。token中含设备相关信息。
 * bindParameter.setBindcode("bindcode");  bindcode绑定必传此参数，bindcode = timestamp+sha1(pk,dn,ds,timestamp) 时间戳单位秒
 */
QilManager.getInstance().bindDeviceWithProductKey(BindMessageParameter bindMessageParameter, MyCallBack myCallBack);
```

**返回数据**

```
//数据示例
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
            "product_key":"xxxxxxxxxxxxxxxx",
            "device_name":"xxxxxxxxxxxxxxxx",
            "device_title":"xxxxxxxxxxxxxxxx"
   }
 }
 
//错误码
202022	缺少参数 product_key
202023	缺少参数 device_name
205002	缺少参数 bindcode
205003	缺少参数 token
205004	token参数不符合规范
205005	token无效
205009	productKey 未增加绑定配置
205010	设备已绑定
205011	请求iot失败
205012	iot返回失败
205018	设置预期属性失败

```

#### 3.解绑设备

```
 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 */
QilManager.getInstance().unBindDeviceWithProductKey(String pk, String dn, MyCallBack myCallBack);
```

**返回数据**

```
//返回数据示例
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg"
}

错误码
205000 缺少productKey	
205001 缺少deviceName	
205006 productKey未加白	

```

#### 4.获取用户的设备列表信息

```
 /** 
 *单个设备信息
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 */
QilManager.getInstance().getDeviceListCompletionCallback(String pk, String dn, MyCallBack myCallBack);

 /**
 *多个设备信息
 * MyCallBack 回调
 * devices 多个设备拼接的字符串数组  "[{"product_key":"pk1","device_name":"dn1"},{"product_key":"pk2","device_name":"dn2"}]" 注意转义
 */
QilManager.getInstance().getDeviceListCompletionCallback(String devices, MyCallBack myCallBack)

 /**
 * MyCallBack 回调
 */
QilManager.getInstance().getDeviceListCompletionCallback(MyCallBack myCallBack)

```

**返回数据**

```
返回数据
{
    "code":0,
    "data":{
        "devices":[
            {
                "bind_ts":"1573488814",
                "category_name":"可视门铃",
                "device_name":"360T0546526",
                "device_title":"360智能门铃",
                "online_status":"online",
                "product_key":"905278d7ba64",
                "product_logo":"https://p2.ssl.qhimg.com/t01447b57245f34a27a.png",
                "product_name":"可视门铃V1"
            },
            {
                "bind_ts":"1578996376",
                "category_name":"音箱",
                "device_name":"M01185205678797",
                "device_title":"苗文华家的音箱",
                "online_status":"online",
                "product_key":"65cd94f3559a7aea4b79289e46ecf769",
                "product_logo":"https://p1.ssl.qhimg.com/t019d48ccc902197630.png",
                "product_name":"360音箱"
            },
            {
                "bind_ts":"1577681798",
                "category_name":"音箱",
                "device_name":"M01184400000019",
                "device_title":"360 AI音箱test",
                "online_status":"online",
                "product_key":"65cd94f3559a7aea4b79289e46ecf769",
                "product_logo":"https://p1.ssl.qhimg.com/t019d48ccc902197630.png",
                "product_name":"360音箱"
            },
            {
                "bind_ts":"1580100125",
                "category_name":"音箱",
                "device_name":"M01193901823081",
                "device_title":"",
                "online_status":"online",
                "product_key":"65cd94f3559a7aea4b79289e46ecf769",
                "product_logo":"https://p1.ssl.qhimg.com/t019d48ccc902197630.png",
                "product_name":"360音箱"
            }
        ]
    },
    "msg":"Success",
    "reqid":"1586416804597646313-bq7cp93ltipd8ergu330"
}
```

#### 5.QT退出登录

```
 /**
 * MyCallBack 回调
 */
QilManager.getInstance().logoutWithQTModeCompletionCallback(MyCallBack myCallBack)
```

#### 6.通过QT获取Token

```
 /**
 * GetSidCallBack 回调
 * q 通过360用户中心SDK获取
 * t 通过360用户中心SDK获取
 */
QilManager.getInstance().loginGetSidWithQTModeCompletionCallback(String q,  String t,  GetSidCallBack getSidCallBack)
```

#### 7.获取设备在线状态

```
 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 */
QilManager.getInstance().getDeviceOnlineStatusWithProductkey(String pk, String dn, MyCallBack myCallBack)
```

**返回数据**

```
//数据示例
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
         "status":"online"
     }
}
```

#### 8.设置设备属性(同步)

```
 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * key 设置的属性的标识符（identifier），物模型定义
 * val 属性值。取值需和您定义的属性的数据类型和取值范围保持一致
 */
QilManager.getInstance().syncSetDevicePropertyWithProductkey(String pk, String dn, String key, Object val, MyCallBack myCallBack)


 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * items 要设置的属性信息，组成为key:value，数据格式为 JSON String。仅支持单个属性的设置
 */
QilManager.getInstance().syncSetDevicePropertyWithProductkey(String pk, String dn, String items, MyCallBack myCallBack)

```

**返回数据**

```
//返回数据
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
         "result":"{\"coordinate\":\"104.07086:30.549169\"}"
     }
}
```

#### 9.获取设备属性(同步)

```
 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * items 要查询的属性信息，是用逗号(英文逗号)隔开的属性的标识符（identifier）。例如：items=“key1,key2”,如果传的值为空字符串(itmes="")，则表示获取所有属性
 * type 表示属性是从IOT云平台获取，还是从设备上获取的属性，从云平台获取速度快，但是可能不是最新的设备属性，存在一定延迟，从设备直接获取速度慢，但是状态准备，在要求不高的情况下建议使用从影子获取。设置的值：type=1 表示从iot云平台获取（影子系统），type=2表示从设备获取
 */
QilManager.getInstance().syncGetDevicePropertyWithProductkey(String pk, String dn, String items, int type, MyCallBack myCallBack)


 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 */
QilManager.getInstance().syncGetDevicePropertyWithProductkey(String pk, String dn, MyCallBack myCallBack) 


```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
           "desired":[                                            #需要返回的设备期望属性的列表
              {
                  "identifier":"prop1"                #属性的唯一标识符
                  "value":11,                               #设置的属性期望值，数据类型和物模型保持一致
                  "timestamp":"1951234320000",     #设置属性期望值的最新时间戳，精确到毫秒，字符串类型
              }
          ],
          "report":[                                            #需要返回的设备上报属性的列表
              {
                  "identifier":"prop1"                #属性的唯一标识符
                  "value":11,                               #上报的属性值，数据类型和物模型保持一致
                  "timestamp":"1951234320000",     #上报属性的最新时间戳，精确到毫秒，字符串类型
              }
          ]
 } 
}
```

#### 10.设置预期属性

```
 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * identifier 属性名
 * value 属性值
 */
QilManager.getInstance().setDeviceDesiredPropertyWithProductkey(String pk, String dn, String identifier, String value, MyCallBack myCallBack)



 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * items 要设置的期望属性值，组成为属性的Key:Value，数据格式为 JSON String，例如{“ColorTemperature”:35}
 */
QilManager.getInstance().setDeviceDesiredPropertyWithProductkey(String pk, String dn, String items, MyCallBack myCallBack) 

```

```
//返回数据示例
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
         "result":"{\"coordinate\":\"104.07086:30.549169\"}"
     }
}
```

#### 11.获取预期属性

```
 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * items 要设置的期望属性值，组成为属性的Key:Value，数据格式为 JSON String，例如{“ColorTemperature”:35}
 */
QilManager.getInstance().getDeviceDesiredPropertyWithProductkey(String pk, String dn, String items, MyCallBack myCallBack) 
```

**返回数据**

```
//返回数据示例
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
           "desired":[                                            #需要返回的设备期望属性的列表
              {
                  "identifier":"prop1"                #属性的唯一标识符
                  "value":11,                               #设置的属性期望值，数据类型和物模型保持一致
                  "timestamp":"1951234320000",     #设置属性期望值的最新时间戳，精确到毫秒，字符串类型
              }
          ],
          "report":[                                            #需要返回的设备上报属性的列表
              {
                  "identifier":"prop1"                #属性的唯一标识符
                  "value":11,                               #上报的属性值，数据类型和物模型保持一致
                  "timestamp":"1951234320000",     #上报属性的最新时间戳，精确到毫秒，字符串类型
              }
          ]
 } 
}

```

#### 12.调用设备服务(同步)

```
 /**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * identifier 服务的标识符，物模型中查看
 * input 要启用的物模型服务的入参信息，数据格式为JSON String，如， input={“param1”:1}。若此参数为空时，需传入 input={},见input表
 */
QilManager.getInstance().invokeServiceWithProductkey(String pk, String dn, String identifier, String input, MyCallBack myCallBack);


/**
 * 此方法封装了input为空
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * identifier 服务的标识符，物模型中查看
 */
QilManager.getInstance().invokeServiceWithProductkey(String pk, String dn, String identifier, MyCallBack myCallBack)

```

**input表**

| 名称  | 类型   | 必填项 | 描述                                                         |
| :---- | ------ | ------ | ------------------------------------------------------------ |
| key   | string | 必须   | 输入参数的标识符。您在创建该服务（物模型）时，设置的输入参数的标识符 |
| value | object | 必须   | 指定参数值。该值须在您设置的输入参数的取值范围内             |

**返回参数**

```

{
    "code":0,
    "data":{
        "result":"{\"ErrorNO\":0}" 
    },
    "msg":"Success",
    "reqid":"b63cc980e1c711eaabd2910282bf7a84-bsu9a4sbqepq1rri585g"
}

返回的调用结果,数据格式为json string格式，例如：“result”: “{“coordinate”:“104.07086:30.549169”}”


```

#### 13.获取消息列表

```
/**
* MyCallBack
* MessageParameter messageParameter = new MessageParameter();
* messageParameter.setNextid(nextid); //下翻页标识,非必传
* messageParameter.setDate(date);//日期,示例:2019-10-24 默认今天, 非必传
* messageParameter.setLevel(level) //消息级别，可选值：  警戒:warning、重要:major、安全: safe ,非必传
* messageParameter.setProduct_category(product_category); //产品类型，可选值： camera，doorbell，firewall ,非必传
* messageParameter.setProduct_key(product_key); //产品唯一标识,非必传
* messageParameter.setDevice_name(device_name); //设备唯一标识,非必传
* messageParameter.setEvent_type(event_type); //事件类型（有人移动/）,非必传  见:getDeviceSupportEventTypesWithPK
* messageParameter.setLimit(limit); //获取条数：若为空一页返回20条, 非必传
* messageParameter.setIs_rollover(is_rollover); 是否流式数据:1 可以上下翻页到其它天 0 只加载选择日期对应的数据。默认0；非必传
* messageParameter.setExtend(extend); //扩展字段定义，jsonstring类型,非必传 见扩展字段定义
*/
QilManager.getInstance().getDeviceMessageListWithProductkey(MessageParameter messageParameter, MyCallBack myCallBack)

/**
* 全部参数不传
* MyCallBack
*/
QilManager.getInstance().getDeviceMessageListWithProductkey(MyCallBack myCallBack) 

```

*扩展字段定义*

**输入参数extend**

{ "member_ids":"",//按照标注后的member_id进行搜索：多个以"英文逗号"分割 "human_ids":"", //按照图片识别人物id搜索：多个以"英文逗号"分割 "ai_img":1,     //是否展示ai识别头像1是0否,不需要时尽量不要传递该参数,会影响性能 "ai_member":1, //是否展示标注名称1是0否,不需要时尽量不要传递该参数,会影响性能 "get_detail":1,  //获取详细信息：1是，0 否，默认0 }

**返回参数Extend结构**

- 有AI识别时信息时:

`"extend"``:`{``"ai_info"``:{``"human_id"``:``"xxxx"``,``"member_id"``:``"xxx"``,``"member_name"``:``"xxx"``,``"img_info"``:{``"url"``:``""``,``"secretkey"``:``""``,``"encrypt"``:``""``}}}``

- get_detail=``1``时:

  `"extend"``:`{``"detail"``:{物模型上报数据}}`` 

  

**ImageInfo结构**

| 名称     | 标识符    | 数据类型 | 数据说明                                                     |
| :------- | :-------- | :------- | :----------------------------------------------------------- |
| 图片地址 | url       | string   | <BucketName>.<Endpoint>/<ObjectName>                         |
| 密钥     | secretkey | string   | 解密密钥                                                     |
| 加密方式 | encrypt   | enum     | 可选值:none 未加密、 AES128、AES256、old_version:老摄像机数据 |



**videoInfo结构**

| 名称     | 标识符    | 数据类型 | 数据说明                                                     |
| :------- | :-------- | :------- | :----------------------------------------------------------- |
| 视频地址 | url       | string   | <BucketName>.<Endpoint>/<ObjectName>;通过playUrl接口获取实际播放地址 |
| 视频时长 | duration  | int      | 单位秒                                                       |
| 密钥     | secretkey | string   | 解密密钥                                                     |
| 加密方式 | encrypt   | enum     | 可选值:none 未加密;old_version 旧版插件方式; XXOO 使用帝视sdk加解密、old_version:老摄像机数据 |

**返回数据**

```
{
"reqid":"xxx-xxx-xxx",
"code":0,
"msg":"xxx",
"data":{
     "previd":"_2423899768881241",
     "nextid":"_2423899768881247",
     "items":[
              {
                  "id":"xxxx1571846401",
                  "tname":"text2",
                  "tdata":{
                      "ctime":123456789,
                      "title":"xxxx",
                      "author":"xxxx",
                      "icon":"xxxx",
                      "device_name":"xxxx",
                      "product_key":"xxxx",
                  }
               },
              {
                  "id":"xxx",
                  "tname":"video",
                  "tdata":{
                      "ctime":123456789,
                      "title":"xxxx",
                      "author":"xxxx",
                      "image":{                         
                            "url":"https://aliyun-abc.oss-cn-hangzhou-internal.aliyuncs.com/abc/myphoto.jpg",
                            "secretkey":"xxxxxxxxxxxxxxxx",
                            "encrypt":"AES256",
                         },"video":{                         
                            "url":"https://aliyun-abc.oss-cn-hangzhou-internal.aliyuncs.com/abc/myphoto.jpg",
                            "secretkey":"xxxxxxxxxxxxxxxx",
                            "encrypt":"AES128",
                       },
                      "device_name":"xxxx",
                      "product_key":"xxxx",
                      "extend":`{"ai_info":{"human_id":"xx","member_name":"xxx","member_id":"xxx","img_info":{"url":"","secretkey":"","encrypt":""}}}`
                  }
             },
             {
                  "id":"xxx",
                  "tname":"video_cloud",
                  "tdata":{
                      "ctime":123456789,
                      "title":"xxxx",
                      "author":"xxxx",
                      "image":{                         
                            "url":"https://aliyun-abc.oss-cn-hangzhou-internal.aliyuncs.com/abc/myphoto.jpg",
                            "secretkey":"xxxxxxxxxxxxxxxx",
                            "encrypt":"AES256",
                         },
                      "video":{                         
                            "url":"https://aliyun-abc.oss-cn-hangzhou-internal.aliyuncs.com/abc/myphoto.jpg",
                            "secretkey":"xxxxxxxxxxxxxxxx",
                            "encrypt":"AES128",
                       },
                      "cloud_info":{"cloud_id":"xxxx","ctime":123456789}
                      "device_name":"xxxx",
                      "product_key":"xxxx", 
                  }
              }
          ]
    }
```

| 名称  | 类型   | 描述         |
| :---- | :----- | :----------- |
| code  | int    | 响应状态码   |
| msg   | string | 响应状态描述 |
| reqid | string | 请求唯一标识 |
| data  | map    | 响应数据     |

**data**

| 名称     | 类型      | 描述           |
| :------- | :-------- | :------------- |
| nextid   | string    | 下翻页位置标识 |
| messages | message[] | 消息列表       |

**message**

| 名称  | 类型   | 描述                                                         |
| :---- | :----- | :----------------------------------------------------------- |
| id    | string | 消息id                                                       |
| tname | string | 模版名称，每个模版定义了模版数据的结构，通过模版名称，选择相应的展现形式。 |
| tdata | map    | 模版数据                                                     |

**tname值说明**

| 值          | 描述                       | 效果图   |
| :---------- | :------------------------- | :------- |
| text1       | 标题+时间+作者             | 文本模式 |
| text2       | 标题+时间+作者+icon布局    | 文本模式 |
| text3       | 标题+时间+作者+去设置按钮  | 文本模式 |
| text4       | 消息总数                   | 文本模式 |
| video       | 标题+时间+作者+image+video | 视频模式 |
| video_cloud | 标题+时间+作者+image+video | 视频模式 |
| image       | 标题+时间+作者+image       | 图片模式 |

**text1 tdata**

| 名称        | 类型       | 描述                                                         |
| :---------- | :--------- | :----------------------------------------------------------- |
| title       | string     | 标题                                                         |
| ctime       | timestamp  | 消息创建时间                                                 |
| author      | string     | 消息创建者                                                   |
| level       | string     | 消息级别                                                     |
| product_key | string     | 硬件产品标识                                                 |
| device_name | string     | 设备唯一标识                                                 |
| event_type  | string     | 消息类型                                                     |
| extend      | jsonstring | [扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode) |

**text2 tdata**

| 名称        | 类型       | 描述                                                         |
| :---------- | :--------- | :----------------------------------------------------------- |
| title       | string     | 标题                                                         |
| ctime       | timestamp  | 消息创建时间                                                 |
| author      | string     | 消息创建者                                                   |
| icon        | url        | 消息图标                                                     |
| level       | string     | 消息级别                                                     |
| product_key | string     | 硬件产品标识                                                 |
| device_name | string     | 设备唯一标识                                                 |
| event_type  | string     | 消息类型                                                     |
| extend      | jsonstring | [扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode) |

**video tdata**

| 名称        | 类型       | 描述                                                         |
| :---------- | :--------- | :----------------------------------------------------------- |
| title       | string     | 标题                                                         |
| ctime       | timestamp  | 消息创建时间                                                 |
| author      | string     | 消息创建者                                                   |
| image       | imageInfo  | 图片地址:[扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode) |
| video       | videoInfo  | 视频地址:[扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode) |
| level       | string     | 消息级别                                                     |
| product_key | string     | 硬件产品标识                                                 |
| device_name | string     | 设备唯一标识                                                 |
| event_type  | string     | 消息类型                                                     |
| extend      | jsonstring | [扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode) |

**video_cloud tdata**

| 名称        | 类型       | 描述                                                         |
| :---------- | :--------- | :----------------------------------------------------------- |
| 名称        | 类型       | 描述                                                         |
| title       | string     | 标题                                                         |
| ctime       | timestamp  | 消息创建时间                                                 |
| author      | string     | 消息创建者                                                   |
| image       | imageInfo  | 图片地址:[扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode) |
| video       | videoInfo  | 视频地址:[扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode) |
| level       | string     | 消息级别                                                     |
| product_key | string     | 硬件产品标识                                                 |
| device_name | string     | 设备唯一标识                                                 |
| event_type  | string     | 消息类型                                                     |
| cloud_info  | cloudInfo  | 云存信息                                                     |
| extend      | jsonstring | **[扩展字段定义](http://wiki.360iot.qihoo.net/pages/viewpage.action?pageId=22373110&src=contextnavpagetreemode)** |

**cloudInfo 结构**

| 名称         | 标识符   | 数据类型 | 数据说明 |
| :----------- | :------- | :------- | :------- |
| 云存id       | cloud_id | string   |          |
| 云存开始时间 | ctime    | int64    |          |

#### 14.非云存消息删除

```
/**
 * MyCallBack 回调
 * message_ids，消息列表页返回：json_encode(["id1","id2"]),单次最多支持100条数据删除
 * pk 设备ProductKey 
 * dn 设备DeviceName
 */
QilManager.getInstance().deleteCloudStorageMessageByMessageIds(String pk,String dn,String[] message_ids, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
 
 }
```

#### 15.查看指定云存消息

```
/**
 * MyCallBack 回调
 * cloud_id云存消息id，列表请求返回
 */
QilManager.getInstance().watchCloudStorageMessageWithCloudId(String cloud_id, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code":0,
    "data":{
        "messages":[
            {
                "id":"0_20190101-88368027115527319",
                "ctime":1611915841,       //消息创建时间
                "title":"Someone passing by",
                "event_type":"dsl.event.post.HumanPass",
                "image":{
                    "url":"https://iot-7days.oss-cn-beijing.aliyuncs.com/sapp/9b9ccc346a1a/073/000073/86XCR3X10501000073/20210129/cloud_jpg/161191584233881192.jpg?OSSAccessKeyId=LTAI4G3kjciZuMY13HsAF9Wf&Expires=1611919541&Signature=Bzr9Ej0n5yAd5WVyT8zyBwixkhc%3D",
                    "encrypt":"none",
                    "expire_time":3600
                },
                "video":{
                    "url":"aliyun://iot-7days.oss-cn-beijing.aliyuncs.com/sapp/9b9ccc346a1a/073/000073/86XCR3X10501000073/20210129/cloud_m3u8/161191584165786218.m3u8?sign=1a7cdd306bd77a3937cb6df9615ee78e",
                    "secretkey":"265cf032e7e207e13e1571e9c016ffe4",
                    "encrypt":"chacha2"
                }
            }
        ]
    },
    "msg":"Success",
    "reqid":"1611915959534786000-c09u5dseou7arjgtftj0"
}
```

#### 16.获取云存视频列表

```
/**
 * MyCallBack 回调
 * pk 设备ProductKey 必传
 * dn 设备DeviceName 必传
 * MessageListParameter mlp = new MessageListParameter();
 * mlp.setNextid(nextid) //下翻页标识，下次请求返回 非必传
 * mlp.setPrevid(previd) //上翻页标识，上次请求返回 非必传
 * mlp.setDate(date) //日期,示例:2019-10-24 , 若不传默认为今天。 非必传
 * mlp.setCloud_event_type(type) //事件类型，非必传  见参考36:getDeviceSupportEventTypesWithPK 
 * mlp.setLimit(limit) //日期,示例:2019-10-24 ,// 获取条数：若为空一页返回20条 非必传
 */
QilManager.getInstance().getCloudStorageVideoListWithProductkey(String pk, String dn, MessageListParameter messageListParameter, MyCallBack myCallBack)



/**
 * MyCallBack 回调
 * pk 设备ProductKey 必传
 * dn 设备DeviceName 必传
 */
QilManager.getInstance().getCloudStorageVideoListWithProductkey(String pk, String dn, MyCallBack myCallBack)

```

**返回数据**

```
{
    "reqid":"xxx-xxx-xxx",
    "code":0,
    "msg":"xxx",
    "data":{
        "nextid":"_2423899768881247",
        "previd":"_2423899768881247"
        "messages":[
            {
                "id":"89_20200803-55140721298641877",
                "cloud_id":"test_009_1596443181", 
                "ctime":1596443213,
                "image":{                         
                   "url":"aliyun-abc.oss-cn-hangzhou-internal.aliyuncs.com/abc/myphoto.jpg",
                   "secretkey":"xxxxxxxxxxxxxxxx",
                   "encrypt":"xx",
                },
                "video":{                         
                   "url":"aliyun-abc.oss-cn-hangzhou-internal.aliyuncs.com/abc/myphoto.m3u8",
                   "secretkey":"xxxxxxxxxxxxxxxx",
                   "encrypt":"xx",
                   "duration":10,
               },
 
               "device_name":"xxxx",
               "product_key":"xxxx",
               "extend":"{\"ai_info\":{\"human_id\":\"xxx\",\"member_name\":\"xxx\",\"member_id\":\"xxx\",\"img_info\":{\"url\":\"\",\"secretkey\":\"\",\"encrypt\":\"\"}}}"
            }
        ]
    }
}
```

#### 17.获取设备云存有效时间

```
/**
 * MyCallBack 回调
 * pk 设备ProductKey 必传
 * dn 设备DeviceName 必传
 */
QilManager.getInstance().getCloudStorageValidTimeWithProductkey(String pk, String dn, MyCallBack myCallBack) 
```

**返回数据**

```
{
    "code":0,
    "data":{
        "commodity_name":"赠送30天滚动云存储服务270天",
        "device_name":"device_name10",
        "end_timestamp":1641225599,  //结束时间戳
        "product_key":"product_key10",
        "service_day_num":270,  //云存服务天数
        "start_timestamp":1617790303,//开始时间戳
        "storage_day_num":30,  //云存存储天数
        "surplus_second":23435296  //剩余秒数
    },
    "msg":"Success",
    "reqid":"1617973279226058042-c1o507rltipclnd1s5a0"
}
```

#### 18.获取人员列表

```
/**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 * HumanListParameter listParameter = new HumanListParameter();
 *listParameter.setMark_type(); //是否标注：0否,1是,2返回所有;默认0
 *listParameter.setBorder_time(); //时间戳，未标注列表返回多少天内的数据，比如7天内，border_time = now_time-7*24*3600，默认7天内数据
 */
QilManager.getInstance().getMemberListWithProductkey(String pk, String dn, HumanListParameter parameter, MyCallBack myCallBack)

/**
 * MyCallBack 回调
 * pk 设备ProductKey
 * dn 设备DeviceName
 */
QilManager.getInstance().getMemberListWithProductkey(String pk, String dn, MyCallBack myCallBack) 
```

**返回数据**

```
{
    "reqid": "2557e4b15b89ab39",
    "code": 1,
    "msg": "success",
    "data": {
        "unmark_list": {
            "list": [{
                "human_id": "bf93414460db9183884fe981ccbd1b20_1",
                "img_info": {"url":"","secretkey":"","encrypt":"","expire_time":1678923623}
            }],
            "total": 1
        },
        "marked_list": {
            "list": [{
                "member_id": "bf93414460db9183884fe981ccbd1b20_1",
                "member_name": "test2",
                "img_info": {"url":"","secretkey":"","encrypt":"","expire_time":1678923623}
            }],
            "total": 1
        }
 
    }
 
}

```

| 名称  | 类型   | 描述         |
| ----- | ------ | ------------ |
| code  | int    | 响应状态码   |
| msg   | string | 响应状态描述 |
| reqid | string | 请求唯一标识 |
| data  | map    |              |

data

| 名称        | 类型   | 描述       |
| ----------- | ------ | ---------- |
| unmark_list | humans | 未标注列表 |
| marked_list | humans | 已标注列表 |

humans

| 名称  | 类型  | 描述 |
| ----- | ----- | ---- |
| list  | array |      |
| total | int   | 数量 |

list

| 名称        | 类型   | 描述                                                         |
| ----------- | ------ | ------------------------------------------------------------ |
| human_id    | string | 人脸id                                                       |
| member_id   | string | 成员id：标注时生成                                           |
| member_name | string | 成员名称                                                     |
| img_info    | map    | {"url":"","secretkey":"","encrypt":"","expire_time":1678923623} |

**img_info 结构**

| 名称     | 标识符    | 数据类型 | 数据说明                                                     |
| :------- | :-------- | :------- | :----------------------------------------------------------- |
| 图片地址 | url       | string   | <BucketName>.<Endpoint>/<ObjectName>                         |
| 密钥     | secretkey | string   | 解密密钥                                                     |
| 加密方式 | encrypt   | enum     | 可选值:none 未加密、 AES128、AES256、old_version:老摄像机数据 |

#### 19.获取视频播放地址

```
/**
* MyCallBack 回调 
* url 消息列表返回
*/
QilManager.getInstance().getVideoURLWithMessageURLString(String url, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
        "url":"https://xxx.mp4?sign=xx" //视频播放地址
        "expire_time":3600     //url过期时间，单位3:比如1800为30分钟失效
        }
}
```

#### 20.获取签名url地址

```
/**
* MyCallBack 回调 
* pk 设备ProductKey
* dn 设备DeviceName
* url 
*/
QilManager.getInstance().getSignUrl(String product_key, String dn, String url, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
        "sign_urls":{"https://xxx.mp4":""https://xxx.mp4?sign=xx""} //签名url地址 key 是未签名的url，value是签名后的url
        }
}
```

#### 21.获取视频云鉴权及轮转密钥

```
/**
* MyCallBack 回调 
* pk 设备ProductKey
* dn 设备DeviceName
*/
QilManager.getInstance().getLicenseCodebookWithProductkey(String pk, String dn, KeyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
            "secret_keys":[                 //轮转密钥,index为轮转序号，key为密钥
             {"202":"xxxxxxxxxxxxxxxxxxxx"},
             {"203":"xxxxxxxxxxxxxxxxxxxx"},
             {"204":"xxxxxxxxxxxxxxxxxxxx"},
             {"205":"xxxxxxxxxxxxxxxxxxxx"},
             {"206":"xxxxxxxxxxxxxxxxxxxx"},
             {"207":"xxxxxxxxxxxxxxxxxxxx"},
             {"208":"xxxxxxxxxxxxxxxxxxxx"},
             ],
             "secret_interval":3600,       //轮转密钥轮换间隔，单位：秒
             "license":{											//签名许可信息
                 "auth_time":1234567890,		 //当前请求时间戳
                 "rand_num":88888,            //随机数
                 "authorization":"xxxxxxxx",  //签名
                 "product_id":"xxxxxxxx", 		//视频云产品标识
                 "sn":"xxxxxxxx",    					//视频云设备标识， 一般与iot云device_name一致
                 "expire":3600,			 					//签名过期时长，单位：秒
             }
      }
 }
```

#### 22.检查固件版本信息

```
/**
* MyCallBack 回调 
* pk 设备ProductKey
* dn 设备DeviceName
* CheckVersionParameter checkVersionParameter = new CheckVersionParameter()
* checkVersionParameter.setGroup_identifier(group_identifier);  //分组的唯一标识，默认为 default 非必传
* checkVersionParameter.setCheck_online(Integer.parseInt(check_online));//是否需要校验设备在线状态 1:否 2:是 默认为 2非必传
*/
QilManager.getInstance().checkOTAVersionWithProductkey(String pk, String dn, CheckVersionParameter checkVersionParameter, MyCallBack myCallBack)
/**
* MyCallBack 回调 
* pk 设备ProductKey
* dn 设备DeviceName
*/
QilManager.getInstance().checkOTAVersionWithProductkey(String pk, String dn, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
    "data":{
        "current_version": "1.22",   		//当前ota版本
        "descriptions": "固件描述-中文", 	//固件的具体描述,当upgrade = 1时，该字段存在
        "is_upgrading": 0,             	//升级是否成功:0 升级成功；1 正在升级中
        "ota_type": 1,									//固件类型,当upgrade = 1时，该字段存在
        "size": 1,											//固件大小，单位,当upgrade = 1时，该字段存在
        "upgrade": 1,     							 //检测是否需要升级:0 已检测，无需升级；1 已检测，需要升级
        "version": "4.1.2"  						//需要升级的固件版本号,当upgrade = 1时，该字段存在

} }
```

#### 23.开始固件升级

```
/**
* MyCallBack 回调 
* pk 设备ProductKey
* dn 设备DeviceName
* group_identifier 分组的唯一标识，默认为 default。 检查升级接口中如果是多个分组，包含default的话，传递default； 如果不包含default，随机传递一个，upgrade为 1（需要升级）的分组；all_group 会下发该设备所有分组下，未开始升级的配置
*/
QilManager.getInstance().otaUpgradeVersionWithProductkey(String pk, String dn, String group_identifier, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
 }
```

#### 24.查看固件升级进度

```
/**
* MyCallBack 回调 
* pk 设备ProductKey
* dn 设备DeviceName
*/
QilManager.getInstance().otaUpgradeProgressWithProductkey(String pk, String dn, MyCallBack myCallBack) 
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
 "data": {
        "description": "等待设备拉取配置",
        "message": "",
        "percent": 0,  //当前升级进度百分比
        "progress": 1   //升级当前已进行到第几步:0 升级成功；1 等待设备拉取配置；2:设备已拉取配置；3 固件下载失败；4 设备正在升级；5 升级失败
    },

 }
```

#### 25.获取配置

```
/**
* MyCallBack 回调 
*/
QilManager.getInstance().getConfigInfoCompletionCallback(MyCallBack myCallBack)
```

**返回数据**

```
{
    "code":0,
    "data":{
        "video_appid":"xxxxxx",
        "fastnet":{
            "app_key":"xxxx",
            "tuis":["xxxx","xxxx1"],
            "region":"cn",
            "iot_port":"80",
            "iot_tls_port":"443",
            "fc_tls_port":"443",
         }
 
    },
    "msg":"Success",
    "reqid":"202008032049397832-bsk0forltipdkpaicgn0"
}
```

#### 26.设置设备基本信息

```
/**
* MyCallBack 回调 
* pk 设备ProductKey
* dn 设备DeviceName
* device_title  设备名称，用户自定义
*/
QilManager.getInstance().modifyDeviceTitleWithProductkey(String pk, String dn, String device_title, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",

}
```

#### 27.创建分组

```
/**
* MyCallBack 回调 
* group_name  组名称，允许: 汉字,0-9,A-Z,a-z， 不超过60字符
* GroupParameter p = new GroupParameter()
* p.setDescription(description) //组描述，不超过200字符 非必传
* p.setParent_id(parent_id) // 父分组id，为空，则创建顶层分组 非必传
*/
 QilManager.getInstance().createGroup(String group_name, GroupParameter groupParameter, MyCallBack myCallBack)

/**
* MyCallBack 回调 
* group_name  组名称，允许: 汉字,0-9,A-Z,a-z， 不超过60字符
*/
QilManager.getInstance().createGroup(String group_name, MyCallBack myCallBack) 
```

**返回数据**

```
{
    "code":0,
    "msg":"",
    "data":{
        "group_id":123456, 
        "group_name":"组1",
        "description":"组的描述",
        "parent_id":0,
        "device_count":0
        "create_time": 1608090942 //创建时间戳
    }
}
```

#### 28.修改分组

```
/**
* MyCallBack 回调 
* group_name  组名称，允许: 汉字,0-9,A-Z,a-z， 不超过60字符
* description  组描述，不超过200字符
* group_id    组id
*/
QilManager.getInstance().updateGroup(String group_name, String description, String group_id, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code":0,
    "msg":"",
    "data":{
        "group_id":123456, 
        "group_name":"组1",
        "description":"组的描述"
    }
}
```

#### 29.删除分组

```
/**
* MyCallBack 回调 
* group_id    组id
*/
QilManager.getInstance().deleteGroup(String group_id, MyCallBack myCallBack)
```

**返回数据**

```
{

    "reqid":"",
    "code":0,
    "msg":"",
}
```

#### 30.查看分组列表

```
/**
* 则获取顶层设备分组
* MyCallBack 回调 
*/
QilManager.getInstance().getGroupList(MyCallBack myCallBack)

/**
* MyCallBack 回调 
* group_id    组id
*/
QilManager.getInstance().getGroupList(int group_id, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code":0,
    "msg":"",
    "data": {
        "list": [{
                "group_id": 1,
                "group_name": "组名称",
                "description": "用户的自定义描述",
                "parent_id": 29,
                "device_count":18,//设备数量
                "create_time": 1608090942 //创建时间戳
            },
            {
                "group_id": 2,
                "group_name": "组名称",
                "description": "用户的自定义描述",
                "parent_id": 29,
                "device_count":5,//设备数量
                "create_time": 1608090942 //创建时间戳
            }
        ]
    }
}
```

#### 31.批量向组中添加设备

```
/**
* MyCallBack 回调 
* group_id    组id
* devices json序列化格式, 结构为list<Device>, Device为map结构,由product_key及device_name组成。示例: [ {"product_key":"xxxx","device_name":"xxxxxxx"}, {"product_key":"xxxx","device_name":"xxxxxxx"} ]
*/
QilManager.getInstance().addDeviceForGroup(String group_id, String devices, MyCallBack myCallBack)
```

**返回数据**

```
{

   "reqid":"",
    "code":0,
    "msg":"",
}
```

#### 32.批量删除组中设备

```
/**
* MyCallBack 回调 
* group_id    组id
* devices json序列化格式, 结构为list<Device>, Device为map结构,由product_key及device_name组成。示例: [ {"product_key":"xxxx","device_name":"xxxxxxx"}, {"product_key":"xxxx","device_name":"xxxxxxx"} ]
*/
QilManager.getInstance().deleteDeviceForGroup(String group_id, String devices, MyCallBack myCallBack)
```

**返回数据**

```

{
    "reqid":"29e2f4b4-0ffd-4eaf-8185-f55b45cc6f87",
    "code":0,
    "msg":"success"
}
```

#### 33.获取免打扰设置

```
/**
* MyCallBack 回调 
*/
QilManager.getInstance().getDevicesNoDisturbSetWithCompletionCallback(MyCallBack myCallBack)
```

**返回数据**

```
{
    "code":0,
    "data":{
        "end_time":"07:00",
        "is_open":0,
        "start_time":"22:00"
    },
    "msg":"Success",
    "reqid":"202005121718382392-bqt6jrjltipbce4bi03g"
}
```

#### 34.设置免打扰

```
/**
* MyCallBack 回调 
* is_open 是否开启
* start_time	 开始时间(is_open传1时校验) 00:00格式
* end_time 结束时间(is_open传1时校验) 00:00格式
*/
QilManager.getInstance().setNoDisturbWithIsOpen(int is_open, String start_time, String end_time, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
 
 }
```

#### 35.获取设备设置列表

```
/**
* MyCallBack 回调 
*/
QilManager.getInstance().getDevicesSetListWithDeviceList(MyCallBack myCallBack)



/**
* MyCallBack 回调 
* DeviceParameter dp = new DeviceParameter()
* dp.setDevice_name("dn"); 设备唯一标识, 非必传
* dp.setProduct_key("pk"); 产品唯一标识, 非必传 
*/
QilManager.getInstance().getDevicesSetListWithDeviceList(DeviceParameter deviceParameter, MyCallBack myCallBack) 

```

**返回数据**

```
{
    "code":0,
    "data":{
        "28a083be2a12_3600R05001453932":{
            "end_time":"14:00",
            "event":{
                "dsl.event.post.HumanMove":{
                    "is_open":0
                },
                "dsl.event.post.HumanMove2":{
                    "is_open":0
                }
            },
            "is_morrow":0,
            "is_open":1,
            "is_open_time_point":0,
            "is_time_interval":0,
            "second":3600,
            "start_time":"07:00",
            "time_point":"1607509377"
        },
        "28a083be2a12_3600R05001453933":{
            "event":{
                "dsl.event.post.HumanMove":{
                    "is_open":0
                }
            },
            "is_open":0,						//是否开启事件推送
            "is_time_interval":0
        }
    },
    "msg":"Success",
    "reqid":"202012142045093913-bvblslbltipd7kmuc6g0"
}
```

data：

| 字段                              | 类型 | 说明                        |
| :-------------------------------- | :--- | --------------------------- |
| {product_key + "_" + device_name} | map  | pk+dn生成的变量：详情见下表 |

product_key + "_" + device_name

| 字段       | 类型   | 说明                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| is_open    | int    | 是否开启设备推送                                             |
| event      | map    | 事件map                                                      |
| second     | int    | 秒 设置的秒数                                                |
| time_point | string | 不推送时间的时间戳 is_open_time_point 为0有效，表示时间戳以内的时间不推送消息 |



event

| 字段          | 类型 | 说明               |
| :------------ | :--- | :----------------- |
| `{eventType}` | map  | 变量：事件唯一标识 |

{eventType}

| 字段      | 类型 | 说明             |
| :-------- | :--- | :--------------- |
| `is_open` | int  | 是否开启事件推送 |

#### 36.获取指定设备支持的事件消息类型

```
/**
* MyCallBack 回调 
* product_key 产品唯一标识
*/
QilManager.getInstance().getDeviceSupportEventTypesWithPK(String product_key, MyCallBack myCallBack) 
```

**返回数据**

```
{
    "code":0,
    "msg":"Success",
    "reqid":"xxxx-bn2ic95rurq6lg38ledg",
    "data":{
        "select":[
            {
                "event_type":"dsl.event.post.HumanMove", //事件类型
                "name":"有人移动",
                "icon":"xx"
            }
        ]
    }
}
```

#### 37.设置设备事件类型的订阅或者取消

```
/**
* MyCallBack 回调 
* event_type事件type  参考36.获取指定设备支持的事件消息类型 getDeviceSupportEventTypesWithPK
* product_key 产品唯一标识
* device_name  设备唯一标识
*/
QilManager.getInstance().setDeviceSupportEventTypesWithPK(String event_type, String product_key, String device_name, MyCallBack myCallBack)



/**
* MyCallBack 回调 
* event_type事件type  参考36.获取指定设备支持的事件消息类型 getDeviceSupportEventTypesWithPK
* product_key 产品唯一标识
* device_name  设备唯一标识
* is_open 是否开启，默认为0
*/
QilManager.getInstance().setDeviceSupportEventTypesWithPK(String event_type, String product_key, String device_name, int is_open, MyCallBack myCallBack)
```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
 
 }
```

#### 38.设置设备某段时间内免打扰

```

/**
* MyCallBack 回调 
* second 秒 is_open传0时必传
* product_key 产品唯一标识
* device_name  设备唯一标识
* is_open 是否开启
*/
QilManager.getInstance().setDeviceStopPushTimeWithPK(int second, String product_key, String device_name, int is_open, MyCallBack myCallBack) 


/**
* MyCallBack 回调 
* product_key 产品唯一标识
* device_name  设备唯一标识
* is_open 是否开启
*/
QilManager.getInstance().setDeviceStopPushTimeWithPK(String product_key, String device_name, int is_open, MyCallBack myCallBack)

```

**返回数据**

```
{
    "code": 0,
    "msg": "Success",
    "reqid": "xxxx-bn2ic95rurq6lg38ledg",
 
 }
```

### 视频相关功能

- 生成mSessionId 

```
mSessionId = String.valueOf(System.currentTimeMillis());
```

- 创建QHVCTextureView

```
 QHVCTextureView playView = findViewById(R.id.textureView); //布局里面使用播放器View
```

- 创建Session

在Activity的onResume()中创建Session

```
/**
* Context context
* String pk;产品唯一标识
* String dn;设备唯一标识
* SessionType SessionType.LIVE; SessionType类型
* QHVCTextureView playView 播放器View
* QVSPlayCallbackAdapter qVSPlayCallbackAdapter 回调
**/
QVSSDKManager.getInstance().createSession(context,pk,dn,mSessionId,SessionType.LIVE,playView,qVSPlayCallbackAdapter);
```

示例代码

```
 protected void onResume(){
 			super.onResume();
        mSessionId = String.valueOf(System.currentTimeMillis());
        QVSSDKManager.getInstance().createSession(this, pk, dn, mSessionId, SessionType.LIVE, playView, new QVSPlayCallbackAdapter() {
            @Override
            public void onPlayerPrepared(@NotNull String sessionId, @NotNull IQHVCPlayerAdvanced qhvcPlayer) {
                qhvcPlayer.start();
            }

            @Override
            public void onPlayerNetRateChanged(@NotNull String sessionId, @NotNull RateModel rateModel) {
                super.onPlayerNetRateChanged(sessionId, rateModel);
            }

            @Override
            public void onVideoSizeChanged(@NotNull String sessionId, @NotNull IQHVCPlayerAdvanced qhvcPlayer, float width, float height) {
                super.onVideoSizeChanged(sessionId, qhvcPlayer, width, height);
                if (playView != null) {
                    playView.setVideoRatio((float) width / (float) height);
                }
            }

            @Override
            public void onSuccess(String sessionId, IQHVCPlayerAdvanced qhvcPlayer, QHVCSdkValue qhvcSdkValue) {
                mQhvcPlayer = qhvcPlayer;
            }

            @Override
            public void onPullDataStreamSuccess(@NotNull String sessionId) {
                super.onPullDataStreamSuccess(sessionId);
            }


            @Override
            public void onStartServiceFail(@NotNull String sessionId, int code, @NotNull String msg, @org.jetbrains.annotations.Nullable Throwable throwable) {
                super.onStartServiceFail(sessionId, code, msg, throwable);
            }

            @Override
            public void onPullDataStreamFail(@NotNull String sessionId, int code, @NotNull String msg, @org.jetbrains.annotations.Nullable Throwable throwable) {
                super.onPullDataStreamFail(sessionId, code, msg, throwable);
            }

            @Override
            public void onCreatePlayerFail(@NotNull String sessionId, int code, @NotNull String msg, @org.jetbrains.annotations.Nullable Throwable throwable) {
                super.onCreatePlayerFail(sessionId, code, msg, throwable);
            }

            @Override
            public void onFail(@NotNull String sessionId, int code, @NotNull String msg, @org.jetbrains.annotations.Nullable Throwable throwable) {
                super.onFail(sessionId, code, msg, throwable);

            }
        });

        if (playView != null) {
            playView.resumeSurface();
        }
 
 }

```

在Activity的onPause中暂停

```
    protected void onPause() {
        super.onPause();
        QVSSDKManager.getInstance().destorySession(mSessionId, sessionId -> {
            useRTC = false;
            rtcButton.setImageResource(R.drawable.btn_icon_rtc_on);
        });
        if (playView != null) {
            playView.pauseSurface();
        }
    }
```

#### 开启RTC

QVSSDKManager API

```
             QVSSDKManager.getInstance().startRTC(mSessionId, new QVSRTCCallbackAdapter() {
                        @Override
                        public void onStartSuccess(@NotNull String sessionId) {
                            
                        }
                    });
```

#### 关闭RTC

QVSSDKManager API

```
   QVSSDKManager.getInstance().stopRTC(mSessionId, new QVSRTCCallbackAdapter() {
                        @Override
                        public void onStopSuccess(@NotNull String sessionId) {
  												
                        }


                        @Override
                        public void onStopFail(@NotNull String sessionId, int code, @NotNull String msg) {
                           
                        }
                    });
```

#### 设置静音/打开声音

mQhvcPlayer API

```
                    if (mQhvcPlayer.isMute()) {
                        mQhvcPlayer.setMute(false);
             
                    } else {
                        mQhvcPlayer.setMute(true);
          
                    }
```

#### 开启录制

QVLManager API

```
QVLManager.getInstance().startRecorder(mQhvcPlayer, FileUtils.getIDFileSdcardPath() + "/recorder_xxx.mp4", new IQHVCPlayerAdvanced.OnRecordListener() {
                            @Override
                            public void onRecordSuccess() {
                               
                            }

                            @Override
                            public void onRecordFailed(int i) {
                              
                            }
                        }
                );
```

#### 停止录制

QVLManager API

```
QVLManager.getInstance().stopRecorder(mQhvcPlayer);
```

#### 下载录制卡录视频

QVLManager API

```
/**
* QHVCSdkValue -> QVSSDKManager.getInstance().createSession 成功后获取
* sessionId 会话id
* startTime 开始时间
* endTime 结束时间
* destMp4FileName 文件路径
* Map<String, Object> options 配置参数                  options.put(QHVCNetGodSees.QHVC_NET_GODSEES_KEY_STREAM_TYPE,QHVCNetGodSees.QHVC_NET_GODSEES_STREAM_TYPE_DEPUTY);
* qvlDownloadListener 回调
*/
QVLManager.getInstance().downloadRecordVideo(final QHVCSdkValue qhvcSdkValue, String sessionId,long startTime, long endTime, String destMp4FileName, Map<String, Object> options,final OnQVLDownloadListener qvlDownloadListener)
```

#### 强制停止下载卡录文件(结束本次会话)

QVLManager API

```
/**
* deleteMp4File 是否删除视频文件
* sessionId 会话id
*/
QVLManager.getInstance().stopGodSeesDownloadRecordFile(boolean deleteMp4File, String sessionId)
```

#### 暂停下载卡录文件

QVLManager API

```
/**
* sessionId 会话id
* return 状态码
*/
int pauseGodSeesDownloadRecordFile(String sessionId)
```

#### 恢复下载卡录文件

QVLManager API

```
/**
* sessionId 会话id
* return 状态码
*/
int resumeGodSeesDowloadRecordFile(String sessionId)
```

#### 获取下载卡录时间轴

QVLManager API

```

/**
* sessionId 会话id
* Context context
* pk 
* dn
* streamType 1,2,3
* startTime 开始时间
* endTime 结束时间
* onQVLTimelinesListener 回调
*/
QVLManager.getInstance().getGodSeesRecordTimeline(Context mContext, String pk, String dn, int streamType, String sessionId, final long startTime, final long endTime, final OnQVLTimelinesListener onQVLTimelinesListener)
```

#### 下载云视频

QVLManager API

```
/**
* Transcode transcode = new Transcode();
* url 播放地址
* key 秘钥
* dst 文件存放目录
* cloudDownloadListener 回调
*/
QVLManager.downloadCloudVideo(final Transcode transcode, String url, String key, String dst, final OnCloudDownloadListener cloudDownloadListener) 
```

#### 停止下载云视频

QVLManager API

```
/**
* transcode 是下载时创建的那个Transcode
*/
QVLManager.stopDownloadCloudVideo(final Transcode transcode) 
```

#### 视频播放

QILPlayManager

```

/**
* Context context
* url 云存播放地址
* secretkey 秘钥
* qhvcPlayer 播放器   qhvcPlayer = new QHVCPlayer(context);
* playView 播放器View
* qilPlayCallback 回调
*/
QILPlayManager.getInstance().createSession(final Context mContext, String url, final String secretkey, final IQHVCPlayerAdvanced qhvcPlayer, final QHVCTextureView playView, final QILPlayCallback qilPlayCallback)
```

示例代码

```
            QILPlayManager.getInstance().createSession(this, url, secretkey, qhvcPlayer, playView, new QILPlayCallback() {
                @Override
                public void OnPreparedListener(IQHVCPlayerAdvanced qhvcPlayer) {
          
                    qhvcPlayer.start();//开始播放
                }

                @Override
                public void onError(int handle, int what, int extra) {
                   
                }

                @Override
                public void onVideoSizeChanged(int handle, int width, int height) {
                   
                }

                @Override
                public void onProgressChange(int handle, int total, int progress) {
                   
                }

                @Override
                public void onNetRateModelCallback(RateModel rateModel) {
                   
                }

                @Override
                public void onBufferingStart(int handle) {
                   
                }

                @Override
                public void onBufferingProgress(int handle, int progress) {
                   
                }

                @Override
                public void onBufferingStop(int handle) {
                   
                }

                @Override
                public void onCompletion(int handle) {
                  
                }

                @Override
                public void onFirstFrameRender(int handle, int what, int extra) {
                   
                }
            });

```

#### 暂停视频播放

```
/**
* qhvcPlayer 播放器
* playView 播放器View
*/
QILPlayManager.getInstance().stopPlay(qhvcPlayer, playView);
```

