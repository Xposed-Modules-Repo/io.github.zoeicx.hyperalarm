<div align="center">
<img src="https://raw.githubusercontent.com/Zoeicx/HyperAlarm/main/imgs/icon.webp" width="140" height="140" alt="HyperAlarm" />

# HyperAlarm

**让小米时钟的闹钟触发 root 脚本 / Run a root script when a Mi Clock alarm fires**

</div>

## 它做什么

作用域只有一个：**小米时钟**（`com.android.deskclock`）。闹钟响的时候，模块在时钟进程里按闹钟**标签**去找同名脚本，以 root 身份执行。

```text
/data/adb/alarm_shell/scripts/valorant.sh   ← 闹钟标签写 "valorant.sh"
```

- 脚本以 root 执行，跑在系统主命名空间（`nsenter -t 1 -m`），能看到其它应用的数据目录
- 时钟自己的响铃、稍后提醒、重复规则全部照旧，脚本只是"顺带执行"
- 设了锁屏密码的话，静默任务会先解锁屏幕
- 每次执行留日志，应用内可看，并镜像到 `/sdcard/Android/data/io.github.zoeicx.hyperalarm/files/script_logs/<脚本名>/`
- 模块自带主页，1:1 复刻时钟闹钟列表，改标签不用进时钟

## 要求

| 项 | 要求 |
| --- | --- |
| 系统 | Android 16（SDK 36）/ HyperOS 3.x |
| 时钟 | 小米时钟 `18.28.0`（versionCode `1306002800`）—— 只在这个版本上验证过，其它版本需自行测试 |
| 框架 | LSPosed，Xposed API 102 |
| root | KernelSU / Magisk（仅用于执行脚本、读写脚本目录、同步日志） |

作用域只勾 **小米时钟**，不要多勾。

## 安装

1. 安装 APK，在 LSPosed 里启用模块，作用域只勾「小米时钟」
2. 强制停止时钟并重新打开，让模块注入时钟进程
3. 把脚本放进 `/data/adb/alarm_shell/scripts/`，给执行权限，确保 LF 换行
4. 新建一个闹钟，**标签写成脚本文件名**（例如 `valorant.sh`）

> 卸载重装（尤其换签名）会让 LSPosed 丢掉模块绑定：时钟进程里没有 hooks、主页列表空白。回 LSPosed 重新启用并勾作用域，再重启时钟即可。

## 已知限制

- 脚本依赖时钟内部实现，小时钟更新可能失效
- 没有 root 时模块不可用（脚本无法执行）
- 只验证过上面那一版时钟

## 链接

- 源码 / 更新日志：<https://github.com/Zoeicx/HyperAlarm>
- 协议与许可证：AGPL-3.0

---

## English

**Scope: Mi Clock (`com.android.deskclock`) only.**

When an alarm fires, the module matches the alarm **label** against a script file name and runs it as root from `/data/adb/alarm_shell/scripts/`. The clock keeps its own ringing/snooze/repeat behaviour; the script is a side effect. The module app mirrors the clock's alarm list so labels can be edited without opening the clock.

Requires Android 16 / HyperOS 3.x, Mi Clock `18.28.0` (verified only on that build), LSPosed with Xposed API 102, and root (KernelSU/Magisk) for script execution. Enable the module for **Mi Clock only**.

Source, releases and changelog: <https://github.com/Zoeicx/HyperAlarm>
