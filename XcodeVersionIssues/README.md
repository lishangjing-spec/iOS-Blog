# XcodeVersionIssues Q&A
> 记录 Xcode 历史版本问题


## Xcode 15.2


<details>
<summary>Xcode 15.2 无法创建分类或扩展</summary>
<br/>

**原因：**
Xcode 15 安装包目录中缺少 `CategoryNSObject` 和 `ExtensionNSObject` 文件导致

**解决方案：**

1. 下载文件：
    `CategoryNSObject` 及 `ExtensionNSObject`
    - 获取方式一: 从旧版本 Xcode 安装包中复制
    - 获取方式二: [下载地址](Xcode%2015/Objective-C%20File.xctemplate.zip)
2. 前往目录：
    `/Applications/Xcode.app/Contents/Developer/Library/Xcode/Templates/File Templates/MultiPlatform/Source/Objective-C File.xctemplate`
3. 复制文件至目录：
    `CategoryNSObject` 及 `ExtensionNSObject`
4. 重启 Xcode 15

**相关文章：**
- https://forums.developer.apple.com/forums/thread/744446


</details>

