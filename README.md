# World Stabilizer（世界稳定器）— KSP 1.12.5 本地编译版

修复 KSP 载具载入时的「地面碰撞爆炸」问题（官方 Bug [#16159](https://bugs.kerbalspaceprogram.com/issues/16159)）：
游戏有时会把载具放到地面以下再启动物理，结果载具被强力弹开。本 mod 在物理启动前「接住」载具，
尽量把它放到地面之上并平缓释放。

本仓库是 [whale2/WorldStabilizer](https://github.com/whale2/WorldStabilizer)（commit `ecd91a4`）的
**KSP 1.12.5 本地编译版**：源码未改动，使用 KSP 1.12.5 的程序集重新编译，并附带已编译好的 DLL。

## 目录结构

| 路径 | 说明 |
|---|---|
| `src/` | C# 源码（WorldStabilizer 本体 + KAS 鱼叉/挂架重新连接模块） |
| `GameData/WorldStabilizer/` | 成品：复制进 KSP 的 `GameData/` 即可 |
| `GameData/WorldStabilizer/Plugins/WorldStabilizer.dll` | 已编译插件（适配 KSP 1.12.5） |
| `GameData/WorldStabilizer/WorldStabilizer.cfg` | 配置文件（稳定时长、贴地、鱼叉重连等，附注释） |

## 安装

把 `GameData/WorldStabilizer` 整个文件夹复制到 KSP 安装目录的 `GameData/` 下，重启游戏即可。

## 主要功能

- **载具载入/起飞稳定**：物理启动前接住载具 → 贴合地面 → 平缓释放，防止弹跳/爆炸
- **KAS 重连**：着陆后自动重新挂接 KAS 鱼叉（`harpoonReiforcedForce` / `harpoonReattachTimeout`）
- **排除列表**：`excludeVessels`（按载具名）、`excludePartModules`（发射夹、地面实验件等默认排除）

## 编译（可选）

需要 KSP 1.12.5 的 `KSP_x64_Data/Managed` 程序集。已验证命令（Visual Studio 2022 BuildTools 的 Roslyn csc）：

```
csc -target:library -out:WorldStabilizer.dll ^
    -r:"<KSP>/KSP_x64_Data/Managed/Assembly-CSharp.dll" ^
    -r:"<KSP>/KSP_x64_Data/Managed/UnityEngine.dll" ^
    -r:"<KSP>/KSP_x64_Data/Managed/UnityEngine.CoreModule.dll" ^
    -r:"<KSP>/KSP_x64_Data/Managed/UnityEngine.PhysicsModule.dll" ^
    src/WorldStabilizer.cs src/GearHarpoonReconnector.cs src/GenericReconnector.cs ^
    src/HangarReconnector.cs src/HarpoonReconnector.cs src/KASAPI.cs ^
    src/KASPylonReconnector.cs src/Properties/AssemblyInfo.cs
```

⚠️ 源码含 C# 6 字符串插值（`$"..."`），必须用 Roslyn 编译器；旧版 .NET Framework 的 csc 会报 `CS1056`。

## License

MIT — 见 [LICENSE](LICENSE)。原作者 [whale2](https://github.com/whale2)；本版本为其 1.12.5 重编译版。

## English

A locally recompiled KSP 1.12.5 build of [whale2/WorldStabilizer](https://github.com/whale2/WorldStabilizer)
(fixes the ground-collision explosion when a vessel is loaded — bug #16159). Source unchanged; rebuilt
against KSP 1.12.5 assemblies. MIT licensed; original by whale2.
