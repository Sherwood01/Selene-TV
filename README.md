# Selene-TV

Android TV（Leanback）端 MoonTV 客户端的**发布与构建仓库**。源码位于私有仓库
[`MoonTechLab/Selene-TV-source`](https://github.com/MoonTechLab/Selene-TV-source)。

本仓库只承载 CI：推送 `v*` 形式的 tag 时，GitHub Actions 会拉取源码仓库、构建
release APK（按 ABI 分包），并把产物发布到本仓库的 Release。

## 发布流程

```bash
# 在本仓库（Selene-TV）打 tag 即可触发，版本号随意（建议与源码 versionName 一致）
git tag v1.0.0
git push origin v1.0.0
```

构建完成后，Release 会附带两个安装包：

| 安装包 | 架构 | 适用设备 |
|--------|------|----------|
| `SeleneTV-<ver>-arm64-v8a.apk` | armv8 | 64 位 ARM，主流 Android TV / 电视盒子 |
| `SeleneTV-<ver>-armeabi-v7a.apk` | armv7a | 32 位 ARM，较老设备 |

也可在 Actions 页面手动 `workflow_dispatch` 触发（不创建 Release，仅产出 artifact）。

## 所需 Secrets

在本仓库 **Settings → Secrets and variables → Actions** 配置：

| Secret | 必需 | 说明 |
|--------|------|------|
| `PULL_TOKEN` | ✅ | 可读取私有源码仓库的 PAT（fine-grained，授予 `Selene-TV-source` 的 Contents: Read） |
| `KEY_STORE_PASSWORD` | ✅ | keystore 密码 |
| `KEY_PASSWORD` | ✅ | key 密码 |
| `ALIAS` | ✅ | key 别名 |
| `SIGNING_KEY` | ⬜ | 可选。base64 编码的 keystore，用于覆盖源码仓库内已提交的 `key.jks` |

> 签名稳定性：源码仓库已提交 `key.jks`，CI 默认用它签名，因此每次 Release 使用同一签名，
> 满足 Android 应用覆盖升级要求。仅当需要更换签名时才设置 `SIGNING_KEY`。
