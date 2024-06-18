# iOS-Blog
iOS  工作记录

- [iOS 审核问题](IOSAuditRecords/README.md)
- [Xcode 历史版本问题](XcodeVersionIssues/README.md)


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
