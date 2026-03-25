# CyberYuan RealDrive

> **CyberYuan RealDrive** 是工业机器人硬件实时驱动模块，实现仿真环境与真实机器人控制器之间的数据通信，支持实时关节驱动、回溯控制和角度校准。

## 前置依赖

| 依赖仓库 | 说明 | 链接 |
|----------|------|------|
| **BaseToolkit** (必需) | 基础框架层，提供 IEngine、Persistence、DBManager 等模块及 MySQL 插件 | [BaseToolkit](https://github.com/Yuan5520/BaseToolkit) |

> **请先导入 BaseToolkit 仓库，再导入本仓库。** RealDrive 依赖 Base 提供的引擎抽象层 (IEngine)、数据持久化 (Persistence)、数据库管理 (DBManager) 以及 MySQL/JSON 序列化插件。

## 关联产品

| 仓库 | 说明 |
|------|------|
| [RobotMatrix](https://github.com/Yuan5520/RobotMatrix) | 机器人仿真系统，配合 RealDrive 可实现仿真-实机数字孪生联动 |

---

## 安装

1. 确保已导入 [BaseToolkit](https://github.com/Yuan5520/BaseToolkit)
2. 将本仓库克隆或下载到 Unity 项目的 `Assets/` 目录下
3. 确保以下目录结构正确放置：
   - `Assets/RealDrive5/`
   - `Assets/Plugins/DriveLicense/`
4. Unity 将自动识别 Assembly Definition 并完成编译
5. 首次使用需要激活许可证：菜单栏 `CyberYuan > Drive License > Activate`

## 系统要求

- Unity 2021.3 LTS 或更高版本
- .NET Standard 2.1 / .NET Framework 4.x
- 已安装 CyberYuan-Base
- 网络连接（用于与机器人控制器通信）

---

## 模块概览

### Core -- 驱动核心

实时硬件通信与关节驱动控制。

- `RealTimeDriveController` -- 实时驱动控制器，负责与机器人控制器建立连接并同步关节数据
- `BackTrackDriveController` -- 回溯驱动控制器，支持运动轨迹的回放与追踪
- `IDriveController` -- 驱动控制器接口，统一不同驱动模式的调用方式
- `AngleCorrectionUtil` -- 角度校正工具，处理关节角度的零位偏移和方向映射
- `DataRateMonitor` -- 数据传输速率监测，用于诊断通信质量
- `JointMovementState` -- 关节运动状态追踪

### Data -- 数据定义

驱动系统所需的数据模型。

- `DriveMode` -- 驱动模式枚举（实时驱动 / 回溯驱动）
- `JointAngleData` -- 关节角度数据包
- `JointAngleInfo` -- 关节角度详细信息
- `JointCalibrationPreset` -- 关节校准预设配置（零位偏移、方向系数等）
- `DriveLogRecord` -- 驱动日志记录
- `ProcessLogFormat` -- 处理日志格式定义

### Editor -- 编辑器扩展

- `RealDriveEditor` -- 自定义 Inspector，提供可视化驱动参数配置
- `DriveLicenseEditorWindow` -- 许可证激活窗口
- `DriveLicenseMenuItems` -- 菜单项注册

### Runtime -- 运行时组件

- `RealDriveBehaviour` -- 主 MonoBehaviour 组件，挂载到 Unity 场景中使用

---

## 产品功能

- **实时关节驱动**：与真实机器人控制器实时通信，将控制器返回的关节数据同步到 Unity 场景中的数字孪生模型
- **回溯驱动模式**：支持录制的运动轨迹回放，可用于离线分析和轨迹验证
- **角度校准系统**：内置关节零位校准和方向映射，适配不同品牌/型号的机器人
- **数据率监控**：实时监测通信数据传输速率，快速定位网络延迟和丢包问题
- **日志系统**：完整的驱动过程日志记录，便于故障排查

## 产品亮点

1. **数字孪生核心** -- 实现虚拟模型与真实机器人的实时同步，延迟低至毫秒级
2. **多模式驱动** -- 实时驱动与回溯驱动灵活切换，满足在线/离线不同场景需求
3. **即插即用** -- 一个 MonoBehaviour 组件即可完成集成，Inspector 可视化配置所有参数
4. **校准预设** -- 支持保存和切换关节校准预设，快速适配不同机器人型号
5. **通信诊断** -- 内置数据率监控，实时反馈通信质量，方便现场调试
6. **许可证保护** -- RSA-2048 + AES-256-CBC 加密的许可证系统

---

## 快速开始

1. 在 Unity 场景中选择机器人 GameObject
2. 添加 `RealDriveBehaviour` 组件：`Add Component > RealDrive5`
3. 在 Inspector 中配置：
   - 通信地址和端口
   - 关节映射关系
   - 校准预设参数
4. 点击 Play 进入运行模式
5. 确保机器人控制器已开启数据服务
6. RealDrive 将自动建立连接并开始同步关节数据

## 许可证激活

1. 菜单栏选择 `CyberYuan > Drive License > Activate`
2. 在弹出窗口中输入邮箱并发送申请
3. 收到许可证密钥后，输入密钥完成激活
4. 可通过 `CyberYuan > Drive License > Check Status` 查看许可证状态

## 与 RobotMatrix 配合使用

当同时安装 RobotMatrix 和 RealDrive 时，可以实现：

- **仿真验证** -- 先在 RobotMatrix 中规划和验证运动轨迹
- **实机执行** -- 通过 RealDrive 将验证后的轨迹发送到真实机器人
- **实时监控** -- RealDrive 采集的实时关节数据可通过 RobotMatrix 的碰撞检测系统进行安全监控
- **数据闭环** -- 仿真数据与实际数据均可上传至数据库进行对比分析

## 技术支持

如有问题请联系 CyberYuan 技术支持团队。
