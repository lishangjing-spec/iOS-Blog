# IOSAuditRecords

iOS - 审核问题记录，所有的问题都可以在 `issues` 上交流

Github：https://github.com/lishangjing-spec/IOSAuditRecords

## 规则跟进渠道

- [Apple 新闻](https://developer.apple.com/cn/news)
- [Apple 审核指南](https://developer.apple.com/cn/app-store/review/guidelines/)
- 是否有苹果新闻推送的 App
- 如果没有的话，就自己去爬新闻网站，推送到邮箱中


---



## 审核问题

<details>
<summary>Guideline 4.3 - Design - Spam</summary>
<br/>

Tag：功能重复，产品在市场过于多
﻿
Your app primarily features dating features. As such, it duplicates the content and functionality of many other similar apps currently available on the App Store.
﻿
While these app features may be useful, informative or entertaining, we simply have enough of these types of apps on the App Store, and they are considered a form of spam. 
﻿
**苹果反馈：** 
您的应用程序主要具有约会功能。因此，它复制了App Store上目前可用的许多其他类似应用程序的内容和功能。
虽然这些应用程序功能可能有用、信息丰富或有趣，但我们在应用商店上有足够多的此类应用程序，它们被视为一种垃圾邮件。

**处理方式：**
完善产品，体现产品价值后在进行发布

</details>

<details>
<summary>Guideline 2.3.3 - Performance - Accurate Metadata</summary>
<br/>

Tag：应用截图

```
2.3.3 Screenshots should show the app in use, and not merely the title art, login page, or splash screen. They may also include text and image overlays (e.g. to demonstrate input mechanisms, such as an animated touch point or Apple Pencil) and show extended functionality on device, such as Touch Bar.
﻿
Issue Description
﻿
Some or all of the provided screenshots do not sufficiently show the app in use. Screenshots should highlight the app's core concept to help users understand the app’s functionality and value. 
﻿
Follow these requirements when adding or updating screenshots:
﻿
- Marketing or promotional materials that do not reflect the UI of the app are not appropriate for screenshots.
- The majority of the screenshots should highlight the app's main features and functionality.
- Confirm that the app looks and behaves identically in all languages and on all supported devices.
- Make sure that the screenshots show the app in use on the correct device. For example, iPhone screenshots should be taken on iPhone, not on iPad.
﻿
Next Steps
﻿
The iPad Pro (2nd Gen) and iPad Pro (6th Gen) screenshots show an iPhone image that has been modified or stretched to appear to be an iPad image. Upload new screenshots that accurately reflect the app in use on each of the supported devices.
```

**苹果反馈：** 
1. 应用截图，应该体现 App 功能，不能是简单的首页、登录注册等界面的截图
2. 截图不能拉伸，使用正确分辨率的设备或模拟器进行截图

**存在的问题：**
1. 我提供的截图过于简单
2. 我使用了小屏幕的截图，调整了分辨率进行了提交，因为当时模拟器因为一些问题导致无法运行


**处理方式：**
1. 丰富截图内容
2. 通过模拟器运行正确的设备进行截图

**处理结果：通过审核**

</details>


<details>
<summary>Guideline 2.1 - Performance - App Completeness</summary>
<br/>
Tag：内购、无法从苹果服务器获取商品信息、response.products.count == 0 

```
We found that your in-app purchase products exhibited one or more bugs which create a poor user experience. Specifically, there was no further action produced when we attempted to make a purchase. Please review the details and resources below and complete the next steps.
﻿
Review device details: 
﻿
- Device type: iPhone 12 
- OS version: iOS 17.4.1
﻿
Next Steps
﻿
When validating receipts on your server, your server needs to be able to handle a production-signed app getting its receipts from Apple’s test environment. The recommended approach is for your production server to always validate receipts against the production App Store first. If validation fails with the error code "Sandbox receipt used in production," you should validate against the test environment instead.
﻿
Resources
﻿
- Learn how to set up and test in-app purchase products in the sandbox environment.
- For more information on receipt validation, see the In-App Purchase FAQ. 
- If your app makes a SKReceiptRefreshRequest call and fails, do not retry the call. Assume the user does not have access. Continue by making the addPayment call.
- If your app makes a SKReceiptRefreshRequest call to restore previously purchased in-app purchases, make sure the app calls restoreCompletedTransactions when the user selects the "Restore" button.
﻿
Support
﻿
- Reply to this message in your preferred language if you need assistance. If you need additional support, use the Contact Us module.
- Consult with fellow developers and Apple engineers on the Apple Developer Forums.
- Help improve the review process or identify a need for clarity in our policies by suggesting guideline changes.
```


**苹果反馈：** 
在点击内购商品的时候，没有错误提示，没有下一步的操作，无法完成内购行为

**自行检查：** 
- 在苹果的反馈截图中，有错误提示：“无法获取产品信息, 请重试”
- 这个错误信息是在苹果 API 回调中触发 
    `- (void)productsRequest:(SKProductsRequest *)request didReceiveResponse:(SKProductsResponse *)response`
- 检查所有的内购配置：
    1. Xcode 与证书配置中In-App Purchases 开关是打开状态
    2. 协议、税务和银行业务也填写了信息
    3. App 也与内购项目进行了绑定

经过排查后，我认为程序上没有存在问题，可以把问题抛回给苹果，审核人员也是人，也并非不会犯错
回复中所有的涉及的截图不方便展示，根据自身项目进行截图替换文件名

**回复内容：**
 您好，在您提供的截图中，我看到了错误信息的返回，并非没有任何下一步的处理，截图文件 “Screenshot-0331-171650.png” 中显示“无法获取产品信息, 请重试”，这个错误信息是因为在苹果API回调方法

`- (void)productsRequest:(SKProductsRequest *)request didReceiveResponse:(SKProductsResponse *)response`

回调中 response.products.count 的数量为 0 ，也就是说，我并没有从苹果API的回调中获取内购列表信息

这个项目是我首次提交的项目，与内购一同在审核中，内购项目也处于“正在等待审核”的状态，请确认您的审核环境，或是缓存等问题，同时请帮我确认我的内购项目的审核状态。

同时，在您反馈的截图中，我发现一个点，我并未在您触发内购的截图中观察到网络环境，无论是wifi还是蜂窝。

在这之前，我也检查了所有的配置：

1. Xcode 与证书配置中In-App Purchases 开关是打开状态（提供了截图“WX20240401-111418.png”）
2. 协议、税务和银行业务也填写了信息
3. App 也与内购项目进行了绑定（提供了截图“WX20240401-101243.png”）

我也提供了我在 TestFlight 中的测试视频，请查看附件("test1.mp4")
以及相同系统环境下的测试视频：请查看附件("test2.mp4")

请您检查并确认后，再次进行测试，感谢

**处理结果：通过审核**
</details>

<details>
<summary>Guideline 5.1.1 - Legal - Data Collection and Storage</summary>
<br/>
Tag：内购、Storage

```
We noticed that your app requires users to register with personal information to purchase in-app purchase products that are not account based. 
﻿
Apps cannot require user registration prior to allowing access to app content and features that are not associated specifically to the user. User registration that requires the sharing of personal information must be optional or tied to account-specific functionality.
﻿
Next Steps
﻿
To resolve this issue, please revise your app to not require users to register before purchasing in-app purchase products that are not account based. You may explain to the user that registering will enable them to access the purchased content from any of their supported devices and provide them a way to register at any time, if they wish to later extend access to additional devices.
﻿
Please note that although App Review Guideline 3.1.2 requires an app to make subscription content available to all the supported devices owned by a single user, it is not appropriate to force user registration to meet this requirement; such user registration must be optional.
﻿
Resources 
﻿
- Watch a video from App Review with tips for doing more for users with less data. 
- See guideline 5.1.1(v) - Account Sign-In to learn more about our requirements for apps with account-based content and features.
﻿
```

大致意思，App 需要支持不登录就能支付内购
解决方案：与后端配合做一个游客模式，同时这个游客也有自己的 token 进行内购

如果审核之后，会关闭游客模式，可以不考虑后续游客内购内容如何与后续登录的实际账号进行关联
如果你们不关闭，可以多考虑这些优化

**结果：通过审核**

</details>

<details>
<summary>Guideline 2.5.4 - Performance - Software Requirements</summary>
<br/>
Tag：UIBackgroundModes、画中画

```

Guideline 2.5.4 - Performance - Software Requirements
﻿
The app declares support for audio in the UIBackgroundModes key in your Info.plist, but we are unable to play any audible content when the app is running in the background.
﻿
Background audio is intended for use by apps that provide audible content to the user while in the background, such as music player, music creation, or streaming audio apps. 
﻿
Next Steps
﻿
If the app has a feature that requires persistent audio, reply to this message and let us know how to locate this feature. If the app does not have a feature that requires persistent audio, it would be appropriate to remove the "audio" setting from the UIBackgroundModes key.
﻿
Resources 
﻿
- Learn more about software requirements in guideline 2.5.4.
- Review documentation for the UIBackgroundModes key.
```

项目中开启了 `UIBackgroundModes` 但是审核人员并么有发现对应功能，无论是后台播放音乐还是画中画

在我的项目中，我用到了画中画功能，所以我将其开启了

从苹果的反馈 `If the app has a feature that requires persistent audio, reply to this message and let us know how to locate this feature` ，苹果审核人员并没有找到项目中画中画的功能，所以这种情况我们录制 app 中，触发画中画功能的视频给苹果就可以通过。

**解决方式：录制功能视频提交至苹果，并反馈(备注、回复)中进行详细描述**

当用户点击 “xxx” 时，我们会弹出一个教程视频，引导用户如何开启xxx功能、xxxx等行为。为了方便用户在手机桌面一边观看视频一边进行操作，所以我们需要UIBackgroundModes来进行视频播放，这样用户可以在观看教程的同时，进行 xxx 行为。我们录制了一段演示视频，说明具体的应用场景，演示视频的链接：https://xxx.mp4

| 来源：https://blog.51cto.com/u_16099186/9399269 |
|---|
|![](https://raw.githubusercontent.com/lishangjing-spec/IOSAuditRecords/master/assets/WechatIMG161.jpg) |

**结果：通过审核**
</details>

## 邮箱警告



<details>
<summary>项目中存在 UIWebView 的调用</summary>
<br/>
**邮箱描述：**

Deprecated API Usage - Apple will stop accepting submissions of apps that use UIWebView APIs . See https://developer.apple.com/documentation/uikit/uiwebview for more information.

**翻译：**
不推荐使用API-苹果将停止接受使用UIWebView API的应用程序提交。看见https://developer.apple.com/documentation/uikit/uiwebview了解更多信息

**排查方案：**
通过终端命令，搜索项目中谁用到了 `UIWebView`，将其替换或移除，涉及第三方可通过脚本、升级等方案解决

``` sh
grep -r "UIWebView" .
```

**解决方案：**

Cocoapods 涉及的库，都可以通过在 Podfile 添加脚本解决

可能涉及的依赖：
- WebViewJavascriptBridge
    ```
    pre_install do |installer|
        dir_web = File.join(installer.sandbox.pod_dir('WebViewJavascriptBridge'), 'WebViewJavascriptBridge')
        Dir.foreach(dir_web) do |x|
          real_path = File.join(dir_web, x)
          if !File.directory?(real_path) && File.exist?(real_path)
            if x == 'WebViewJavascriptBridge.h' || x == 'WebViewJavascriptBridge.m'
              File.delete(real_path)
            end
          end
        end
      end
    ```
- AFNetwoking 3.x
    升级至 4.x
</details>


<details>
<summary>第三方 SDK 隐私清单和签名（2024年5月1号后上传的app都需要增加隐私描述）</summary>
<br/>

## 
```
Hello,

We noticed one or more issues with a recent submission for App Store review for the following app:

Although submission for App Store review was successful, you may want to correct the following issues in your next submission for App Store review. Once you've corrected the issues, upload a new binary to App Store Connect.

ITMS-91053: Missing API declaration - Your app’s code in the “PlugIns/XXXIntent.appex/XXXIntent” file references one or more APIs that require reasons, including the following API categories: NSPrivacyAccessedAPICategoryUserDefaults. While no action is required at this time, starting May 1, 2024, when you upload a new app or app update, you must include a NSPrivacyAccessedAPITypes array in your app’s privacy manifest to provide approved reasons for these APIs used by your app’s code. For more details about this policy, including a list of required reason APIs and approved reasons for usage, visit: https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api.

ITMS-91053: Missing API declaration - Your app’s code in the “PlugIns/XXXIntent.appex/XXXIntent” file references one or more APIs that require reasons, including the following API categories: NSPrivacyAccessedAPICategoryDiskSpace. While no action is required at this time, starting May 1, 2024, when you upload a new app or app update, you must include a NSPrivacyAccessedAPITypes array in your app’s privacy manifest to provide approved reasons for these APIs used by your app’s code. For more details about this policy, including a list of required reason APIs and approved reasons for usage, visit: https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api.

ITMS-91053: Missing API declaration - Your app’s code in the “XXX” file references one or more APIs that require reasons, including the following API categories: NSPrivacyAccessedAPICategoryFileTimestamp. While no action is required at this time, starting May 1, 2024, when you upload a new app or app update, you must include a NSPrivacyAccessedAPITypes array in your app’s privacy manifest to provide approved reasons for these APIs used by your app’s code. For more details about this policy, including a list of required reason APIs and approved reasons for usage, visit: https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api.

ITMS-91053: Missing API declaration - Your app’s code in the “XXX” file references one or more APIs that require reasons, including the following API categories: NSPrivacyAccessedAPICategorySystemBootTime. While no action is required at this time, starting May 1, 2024, when you upload a new app or app update, you must include a NSPrivacyAccessedAPITypes array in your app’s privacy manifest to provide approved reasons for these APIs used by your app’s code. For more details about this policy, including a list of required reason APIs and approved reasons for usage, visit: https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api.

ITMS-91053: Missing API declaration - Your app’s code in the “XXX” file references one or more APIs that require reasons, including the following API categories: NSPrivacyAccessedAPICategoryDiskSpace. While no action is required at this time, starting May 1, 2024, when you upload a new app or app update, you must include a NSPrivacyAccessedAPITypes array in your app’s privacy manifest to provide approved reasons for these APIs used by your app’s code. For more details about this policy, including a list of required reason APIs and approved reasons for usage, visit: https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api.

ITMS-91053: Missing API declaration - Your app’s code in the “XXX” file references one or more APIs that require reasons, including the following API categories: NSPrivacyAccessedAPICategoryUserDefaults. While no action is required at this time, starting May 1, 2024, when you upload a new app or app update, you must include a NSPrivacyAccessedAPITypes array in your app’s privacy manifest to provide approved reasons for these APIs used by your app’s code. For more details about this policy, including a list of required reason APIs and approved reasons for usage, visit: https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api.

Apple Developer Relations

```
规则更新时间：2023 年 12 月 7 日
主要内容：开发者在5月1号后上传的app都需要增加隐私描述
- [官方新闻](https://developer.apple.com/cn/news/?id=r1henawx)
- [NSPrivacyAccessedAPITypeReasons 配置](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api)
- [三方SDK的应对参考](https://cloud.tencent.com/document/product/269/104138)
- [开发者文章](https://www.jianshu.com/p/633f9778efd7)

### 1.1 添加隐私文件

|  |  |
|---|---|
|![](https://raw.githubusercontent.com/lishangjing-spec/IOSAuditRecords/master/assets/17126394224407.jpg)|![](https://raw.githubusercontent.com/lishangjing-spec/IOSAuditRecords/master/assets/17126394297212.jpg)|


### 1.2 依据警告添加相应的原因说明，添加后的文件内容如下
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>NSPrivacyAccessedAPITypes</key>
    <array>
        <dict>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>E174.1</string>
            </array>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryDiskSpace</string>
        </dict>
        <dict>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>35F9.1</string>
            </array>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategorySystemBootTime</string>
        </dict>
        <dict>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>CA92.1</string>
            </array>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryUserDefaults</string>
        </dict>
        <dict>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>C617.1</string>
            </array>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryFileTimestamp</string>
        </dict>
    </array>
</dict>
</plist>
```

关于 `NSPrivacyAccessedAPITypeReasons` 配置，可查看 [官方文档](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api)

![](https://raw.githubusercontent.com/lishangjing-spec/IOSAuditRecords/master/assets/17126409112246.jpg)




