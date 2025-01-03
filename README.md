# iOS-Blog
iOS  工作记录

- [iOS 审核问题](IOSAuditRecords/README.md)
- [Xcode 历史版本问题](XcodeVersionIssues/README.md)


iOS 升级
https://www.apple.com/ios/ios-18-preview/
https://juejin.cn/post/7381645766446579752

--- 

### M1 电脑通过 Cocoapods 引入三方 SDK 后，无法运行模拟器

**原因：**
三方 SDK 没有使用 XCFramework ，导致包无法自适应不同系统

**解决方案：**
```
platform :ios, '12.0'
  target 'XXX' do
  ...

  # M1 电脑模拟器无法运行的问题
  post_install do |installer|
      installer.pods_project.build_configurations.each do |config|
        config.build_settings["EXCLUDED_ARCHS[sdk=iphonesimulator*]"] = "arm64"
      end
  end
end
```
修改完 Podfile 后 `pod install`


### 编译公司项目的时候出现错误信息：Pods-xxx-frameworks.sh: Permission denied

`/Users/.../xxx-00000.sh: line 2: /Users/.../xxx/Pods/Target Support Files/Pods-xxx/Pods-xxx-frameworks.sh: Permission denied`

**原因：**

项目交接的时候，直接给我一个 `zip` 并非 `git` 仓库，所以也没有过滤掉 `Pod` 文件，正常我们在写项目的时候，是不会将 `Pod` 文件上传至 `Git`

可能是因为上个开发者的 pod 版本不同、权限等问题导致

**解决：**

删除 `Pod` 产生的文件
- Podfile.lock
- Pods
- ...

`pod install`