+++
title = '如何使用免费 Apple 开发者账号进行本地测试'
date = 2024-10-27T00:33:41-07:00
# draft = true
+++

在 iOS 开发中，Apple 为个人提供免费的开发者账号。虽然免费账号相较于付费版本存在一些限制，但仍可用于应用的本地测试和调试。本文将介绍如何使用免费 Apple 开发者账号进行本地测试，并给出具体操作步骤。

## 免费开发者账号的限制

使用免费 Apple 开发者账号时，需要了解以下限制：

1. **不支持 TestFlight**：免费账号无法通过 TestFlight 分发和测试应用。
2. **设备数量限制**：最多可在 3 台设备上部署应用。
3. **证书有效期**：每次部署所使用的证书有效期只有 7 天，过期后需要重新部署应用。
4. **无法发布到 App Store**：免费账号不能向 App Store 上传应用，也无法使用推送通知等高级功能。

## 基本测试步骤

整个测试流程可以分为以下几个主要步骤：

1. **注册 Apple ID**
2. **获取证书和描述文件**
3. **将证书添加到钥匙串并配置 Xcode**
4. **将应用部署到设备或生成 IPA**

## 1. 注册 Apple ID

访问 [Apple Developer 网站](https://developer.apple.com/)，使用邮箱注册新的 Apple ID，或登录已有账号。注册后，确认已接受最新的开发者协议。

## 2. 获取证书和描述文件

由于免费账号使用 Apple 官方配置功能的权限有限，可以借助 **App Uploader** 工具创建所需的证书和描述文件：

- **下载 App Uploader**：[App Uploader 网站](https://www.applicationloader.net/)
- **使用 App Uploader**：
  - **登录**：在 App Uploader 中使用免费的 Apple ID 登录。
  - **生成证书**：在首页点击 `certificate`，创建并下载 `.p12` 文件，同时保存证书密码。
  - **创建 Bundle ID**：进入 `Identifiers`，创建 Bundle ID，通常采用 `com.yourcompanyname.appname` 的格式。
  - **添加测试设备**：在 `Test Devices` 中创建本地测试设备，输入名称和 UDID。连接设备后，可以在 Xcode 中找到 UDID。
  - **创建描述文件**：进入 `Profiles`，为其命名，然后下载生成的 `.mobileprovision` 文件。

## 3. 将证书添加到钥匙串并配置 Xcode

### 1. 将证书添加到“钥匙串访问”

1. 使用 Spotlight 搜索并打开 **钥匙串访问**。
2. 选择 `login` 钥匙串。
3. 将下载的 `.p12` 文件拖入 `login`，输入在 App Uploader 中设置的证书密码。

### 2. 配置 Xcode 签名

1. **登录 Apple ID**：在 Xcode 中登录与 App Uploader 中相同的 Apple ID。
2. **设置 Signing & Capabilities**：
   - 进入项目设置中的 `Signing & Capabilities`。
   - 在“Team”中选择自己的 Apple ID，并输入在 App Uploader 中创建的 Bundle ID。
   - Xcode 应当会自动匹配证书和描述文件。如果没有匹配，可以取消勾选“Automatically manage signing”，手动导入证书和描述文件。

## 4. 将应用部署到设备或生成 IPA

将应用部署到设备主要有两种方式：

- **直接通过 Xcode 部署**：这是最常见的调试方式，Xcode 会将应用直接安装到连接的 iPhone 或 iPad 上。
- **生成 IPA 文件，供手动安装或分享**：可以生成 IPA 文件，手动安装或在免费账号允许的范围内分享给他人。

### 直接通过 Xcode 部署

1. **连接设备**：将 iPhone 连接到 Mac，解锁手机；如果出现提示，在 iPhone 上选择“信任此电脑”。
2. **选择目标设备**：在 Xcode 左上角的设备选择器中，选择已连接的 iPhone。
3. **运行应用**：点击 `Run`，Xcode 会编译应用并将其部署到 iPhone。

### 生成 IPA 文件，供手动安装或分享

#### 1. 归档应用

1. **创建归档**：选择 `Product > Archive`，等待 Xcode 完成归档。
2. **打开 Organizer**：归档完成后，Xcode 会自动打开 Organizer 窗口，显示归档记录。

#### 2. 将 .app 文件转换为 IPA

1. **找到 .app 文件**：导出后，在 Organizer 中右键点击归档并选择 `Show in Finder`。进入归档中的 `Products > Applications`，找到 `.app` 文件。
2. **创建 Payload 文件夹**：复制 `.app` 文件，将其放入新建的 `Payload` 文件夹中。
3. **压缩为 IPA**：右键点击 `Payload` 文件夹，选择“压缩”，然后将生成的 `Payload.zip` 重命名为 `Payload.ipa`。现在就可以将它安装到 iOS 设备上。

#### 3. 安装 IPA 文件

获得 IPA 文件后，可以手动安装，也可以在免费 Apple 开发者账号允许的范围内分享给他人。
