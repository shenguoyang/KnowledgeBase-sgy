# Specular Vision — 代码库目录结构索引

本文件是 Specular Vision 代码库的目录结构"单一事实来源"。其他项目文档（CLAUDE.md、skill 文件等）通过引用此文件获取路径信息，避免内容重复。

---

## 1. 顶层目录布局

```
D:\code\products\auto\inspect\specular_vision\2.x\software\branches\hotfix_2.0.14\
├── applications/       # 应用层（6 个进程级服务 / 桌面应用）
├── components/         # 组件 / 业务层（DLL + RPC 基础设施）
├── tools/              # 辅助 Qt 工具
├── script/             # 构建 / CI / 版本脚本
├── bin/                # 构建产物（Release / Debug 二进制）
├── lib/                # 构建产物（.lib）
├── environment/        # 环境准备与依赖检出脚本
└── .claude/            # Claude Code 配置、skill、codebase-index
```

---

## 2. 应用层

| 应用 | 路径 | 角色 |
|------|------|------|
| `inspection_client` | `applications/inspection_client/` | 操作员 UI —— 检测控制、可视化、用户管理 |
| `inspection_server` | `applications/inspection_server/` | 中央调度 —— 传感器、机器人、自动控制、数据集成 |
| `specular_sensor_server` | `applications/specular_sensor_server/` | 传感器网关 —— 桥接 inspection_server 与 FPGA 硬件 v3 |
| `worker_screen_client` | `applications/worker_screen_client/` | 车间大屏展示（Qt5 + Vue 前端） |
| `data_docking_service` | `applications/data_docking_service/` | 数据同步与对接 |
| `defect_data_tool` | `applications/defect_data_tool/` | NIO 定制数据对接工具（从 data_docking_service 复制） |
| `feite_data_display` | `applications/feite_data_display/` | feite定制功能|
应用层代码位于各应用的 `src/` 目录下，包含 Qt UI、app main、视图层代码。

---

## 3. 组件 / 业务层

### 3.1 核心组件总览

| 组件 | 根路径 | 输出 | 职责 |
|------|--------|------|------|
| `specular_vision_gen` (SVG) | `components/business/specular_vision_gen/` | `specular_vision_gen.dll` | 核心检测编排 —— PLC 信号、自动控制、车辆跟踪、Teach/Inspect 回调 |
| `specular_vision_sensor` | `components/business/specular_vision_sensor/` | `specular_vision_sensor.dll` | FPGA 驱动 —— 图像采集、PMD 检测、Teach/标定 |
| `auto_control` | `components/business/auto_control/` | DLL | 基于 PLC 的线体自动控制 |
| `robot_control` | `components/business/robot_control/` | DLL | 机器人通信接口 |

### 3.2 组件标准子目录结构

各业务组件内部遵循一致的目录布局：

| 路径模式 | 内容 |
|---------|------|
| `src/` | 业务逻辑、DAO、管理器、状态机、信号生成 |
| `src/data/` | 持久化层（DAO 实现、DatabaseManager） |
| `src/data/config_db_mgr/` | 配置表 DAO |
| `src/data/defect_db_mgr/` | 缺陷 / 检测记录 DAO |
| `src/data/<功能>_db_mgr/` | 其他功能表 DAO（如 `robot_db_mgr`） |
| `src/auto/` | 状态机、流程控制、自动信号生成 |
| `src/fpga/` | FPGA 适配器与控制（仅 specular_vision_sensor） |
| `include/internal/` | 内部头文件（非公开 API） |
| `include/internal/version.h` | 版本号定义 |
| `project/` | MSBuild 工程文件（`.vcxproj`, `.sln`） |
| `test/ut/` | 单元测试 |
| `test/sit/` | 系统 / 集成测试 |

---

## 4. `specular_vision_gen` 核心算子

位于 `components/business/specular_vision_gen/src/`，公共 API 算子对应 `*_svg.cpp` 文件：

| 算子类 | 文件模式 | 职责 |
|-------|---------|------|
| `InspectOperatorSvg` | `inspect_operator_svg.cpp` | 在线检测流程（`enter`, `posStart`, `leave` 回调） |
| `TeachOperatorSvg` | `teach_operator_svg.cpp` | Teach / 标定 |
| `ImageStreamOperatorSvg` | `image_stream_operator_svg.cpp` | 图像推流 |
| `DeviceControlSvg` | `device_control_svg.cpp` | 硬件设备控制 |
| `SystemManagerSvg` | `system_manager_svg.cpp` | 系统配置 |
| `VehicleManagerSvg` | `vehicle_manager_svg.cpp` | 车型跟踪 |
| `FinishLineManagerSvg` | `finish_line_manager_svg.cpp` | EOL（End of Line）处理 |
| `PLCSignalOperatorSvg` | `plc_signal_operator_svg.cpp` | PLC 信号 I/O |

---

## 5. RPC 层

| 路径 | 内容 |
|------|------|
| `components/rpc_common/` | 共享请求/响应定义（宏生成的参数块） |
| `components/rpc_common/specular_vision_gen/` | SVG RPC 协议定义 |
| `components/rpc_common/specular_vision_gen/specular_vision_gen_rpc_json.h` | SVG 关键协议头文件 |
| `components/rpc_common/specular_vision_sensor/` | Sensor RPC 协议定义 |
| `components/rpc_common/specular_vision_sensor/specular_vision_sensor_rpc_json.h` | Sensor 关键协议头文件 |
| `components/rpc_common/pillar_lamp_control/` | 灯柱控制 RPC 协议定义 |
| `components/rpc_client/` | RPC 客户端代理实现 |
| `components/rpc_client/specular_vision_gen/` | SVG 客户端 stubs |
| `components/rpc_client/specular_vision_sensor/` | Sensor 客户端 stubs |

RPC 框架为自研（非 gRPC），参数块通过宏自动生成 JSON 序列化代码。

---

## 6. FPGA / 硬件层

| 路径 | 内容 |
|------|------|
| `components/business/specular_vision_sensor/src/fpga/` | FPGA 适配器（`fpga_adapter`）、寄存器控制（`fpga_control`）、超时定时器 |
| `fpga_adapter.h / fpga_adapter.cpp` | FPGA 抽象适配层 |
| `fpga_control.h / fpga_control.cpp` | 寄存器级 I/O |

---

## 7. 工具项目

工具位于 `tools/`，均为 Qt 辅助应用，自带构建工程。

| 工具 | 路径 | 用途 |
|------|------|------|
| `specular_vision_tool` | `tools/specular_vision_tool/` | 相机与屏幕标定 |
| `specular_trainning_tool` | `tools/specular_trainning_tool/` | 深度学习训练 |
| `sensor_config_tool` | `tools/sensor_config_tool/` | 传感器调试 / 固件升级 |
| `plc_monitor` | `tools/plc_monitor/` | PLC 信号位可视化 |

完整工具地图见 `tools/工具地图.txt`。

---

## 8. 构建系统

### 8.1 工程文件位置

- 各应用/组件的 `.vcxproj` 位于 `<模块>/project/platform0.0/vs2017/`
- 例外：`sensor_v3` 使用 `<模块>/project/vs2017/`
- Sensor server 同时存在 `vs2017`（v3 sensor）和 `vs2017_2.x`（v2 sensor）目录

### 8.2 CI 构建

CI 脚本与工程列表：

| 路径 | 内容 |
|------|------|
| `script/ci/ci.bat` | CI 构建入口 |
| `script/ci/projects/all.txt` / `all_3.0.txt` | 全量构建 |
| `script/ci/projects/client.txt` | 客户端构建 |
| `script/ci/projects/sensor.txt` | 传感器构建 |
| `script/ci/projects/tools.txt` | 工具构建 |
| `script/ci/projects/simulator.txt` | 模拟器构建 |
| `script/ci/projects/test.txt` | 测试工程构建 |

构建依赖顺序：`specular_vision_sensor` → `specular_vision_gen` → 应用层。

### 8.3 输出目录

| 配置 | 二进制 | 库文件 |
|------|--------|--------|
| Release | `bin/win64_v141_Release/` | `lib/win64_v141_release/` |
| Debug | `bin/win64_v141_Debug/` | `lib/win64_v141_debug/` |

---

## 9. 测试

每个主要组件均有 `test/` 目录：

| 路径模式 | 内容 |
|---------|------|
| `<组件>/test/ut/` | 单元测试（VS 测试工程） |
| `<组件>/test/sit/` | 系统 / 集成测试 |

示例：
- `components/business/specular_vision_gen/test/ut/database/ut.vcxproj`
- `tools/sv_aging_test_tool/test/ut/task/task.vcxproj`

无统一测试运行器，按工程分别构建并执行。

---

## 10. 数据库层

- **数据库引擎**：SQLite，采用 DAO 模式 + 连接池（`DatabaseManager`）
- **表名定义**：DAO 头文件中通过 `#define *_DATA_BASE_NAME` 宏定义
- **字段定义**：`DATA_BASE_*_FIELD_*` 宏

| 路径模式 | 内容 |
|---------|------|
| `components/business/*/src/data/` | 持久化层根目录 |
| `components/business/*/src/data/config_db_mgr/` | 配置表 DAO |
| `components/business/*/src/data/defect_db_mgr/` | 缺陷记录 DAO |
| `components/business/*/src/data/*_db_mgr/` | 其他功能表 DAO |

---

## 11. 基础设施库

代码库中有自研基础设施库（位于 `components/` 或第三方检出目录），在很多组件中替代标准 STL 使用：

| 库 | 用途 |
|----|------|
| `infrad` | 通用基础设施 |
| `netd` | 网络 |
| `logd` | 日志 |
| `databased` | 数据库封装 |
| `imaged` | 图像处理 |
| `jsond` | JSON 处理 |

---

## 12. 路径约定速查表

按分析需求索引到对应路径：

| 分析目标 | 查找路径 |
|---------|---------|
| 应用层代码 | `applications/*/src/` |
| 业务逻辑 / DAO / 状态机 | `components/business/*/src/` |
| 内部头文件 | `components/business/*/include/internal/` |
| RPC 协议定义 | `components/rpc_common/*/` |
| RPC 客户端代理 | `components/rpc_client/*/` |
| FPGA 适配器与寄存器 | `components/business/specular_vision_sensor/src/fpga/` |
| 数据库管理器 | `components/business/*/src/data/*_db_mgr/` |
| 配置表 DAO | `components/business/*/src/data/config_db_mgr/` |
| 缺陷记录 DAO | `components/business/*/src/data/defect_db_mgr/` |
| 状态机 / 流程控制 | `components/business/*/src/auto/` |
| 版本号定义 | `components/business/*/include/internal/version.h` |
| MSBuild 工程文件 | `components/business/*/project/**/*.vcxproj` |
| CI 工程列表 | `script/ci/projects/` |
| 测试 | `<模块>/test/ut/`, `<模块>/test/sit/` |
