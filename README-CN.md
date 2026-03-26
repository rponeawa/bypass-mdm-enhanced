# Bypass MDM Enhanced (rponeawa)

[English Version / 英文版](README.md)

本工具在 Assaf Dori 原始脚本的基础上进行了功能扩展。增强版本整合了通过对 **micaixin.cn** 商业工具进行逆向工程分析，以及对闲鱼 **@多啦快解** 脚本进行分析得出的核心绕过与持久化逻辑。

---

## 技术增强

本版本实现了通过二进制及脚本分析识别出的以下技术特性：

### 1. 源自 micaixin.cn 的分析
*   **系统守护进程抑制**：初始化系统标志位 `/var/db/.com.apple.mdmclient.daemon.forced_disable`，强制 `mdmclient` 进程在启动时终止。
*   **直接修改配置**：利用 `PlistBuddy` 在系统核心数据库中将 `CloudConfigRecordFound`、`CloudConfigHasActivationRecord` 以及 `CloudConfigProfileInstalled` 显式设置为 `false`。
*   **硬件级属性锁定**：对所有绕过标记和 Plist 配置文件应用 `uchg` (用户不可变) 标志，防止系统自动恢复。
*   **IPv6 连接屏蔽**：在 hosts 文件中包含 IPv6 (`::`) 条目，防止通过 IPv6 隧道进行 MDM 同步。

### 2. 源自 @多啦快解 的分析
*   **FileVault 磁盘解密**：包含检测并解锁受 FileVault 保护的 APFS 卷的逻辑，确保能够访问系统数据库。
*   **扩展服务抑制**：实现了针对 `cloudconfigurationd` 及其他管理代理的显式 `launchctl` 禁用指令，作为额外的防御层。

---

## 安装与使用说明

请按照以下步骤在全新安装 macOS 过程中绕过 MDM 注册：

**1. 关机**
执行 Mac 的强制关机操作。

**2. 进入恢复模式**
*   Apple Silicon (M系列芯片)：按住电源键直至出现启动选项。
*   Intel 处理器：在启动过程中按住 Command + R。

**3. 网络激活**
连接 Wi-Fi 网络以确保 Mac 已激活。

**4. 终端初始化**
从顶部菜单栏选择“实用工具”，并打开“终端”。

**5. 执行脚本**
运行以下命令：
```bash
curl -L https://raw.githubusercontent.com/rponeawa/bypass-mdm-enhanced/main/bypass-mdm-enhanced.sh -o bypass-mdm.sh && chmod +x ./bypass-mdm.sh && ./bypass-mdm.sh
```

**6. 绕过选项**
选择选项 1: "Bypass MDM from Recovery"。

**7. 账户配置**
配置临时管理员账户或使用默认值。

**8. 完成操作**
等待提示：“Bypass Completed Successfully”。

**9. 重启设备**
退出终端并重启 Mac。

---

## 安装后后续步骤

**10. 身份验证**
使用临时账户登录 (默认值: Apple / 1234)。

**11. 设置助手**
跳过所有初始提示 (Apple ID、Siri、Touch ID、定位服务)。

**12. 创建正式账户**
前往“系统设置 > 用户与群组”，创建一个永久的管理员账户。

**13. 系统清理**
在“系统设置”中删除临时的管理员账户。

---

## 故障排除

### 卷检测失败
确认设备处于恢复模式，并且目标磁盘上已存在有效的 macOS 安装。

### 权限被拒绝
确保脚本具有执行权限：`chmod +x bypass-mdm.sh`。

---

**免责声明**: 本工具仅供教育与研究使用。
