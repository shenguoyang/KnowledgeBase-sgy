# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **Specular Vision** industrial inspection system, a C++/Qt5 machine vision platform for automotive surface defect detection using structured light (PMD) via FPGA hardware. It follows a client-server architecture on Windows x64.

- **Language**: C++17 (`stdcpplatest`), Qt5 for client UI
- **Build**: Visual Studio 2017 (v141 toolset) + MSBuild
- **Platform**: Windows x64 (Win10 SDK 17763)
- **CodeGraph**: MCP server available; see `.codegraph/codegraph.md`

## Build Commands

### Prerequisites

Ensure `MS_BUILD_2017` env var points to `MSBuild.exe`, then run once:

```bash
environment/prepare.bat
```

This calls `checkout.bat`, which checks out third-party dependencies (platform, SDKs, Boost, libtorch, etc.) from internal SVN repos.

### Single Project Build

All `.vcxproj` files live under `project/platform0.0/vs2017/` within each application/component (`sensor_v3` uses `project/vs2017/`).

```bash
cd applications/inspection_server/project/platform0.0/vs2017
"%MS_BUILD_2017%" specular_vision_gen_server.vcxproj /t:Rebuild /p:Platform=x64 /p:Configuration=Release /p:VisualStudioVersion=15

devenv specular_vision_gen_server.sln /build "Release|x64"
```

### CI / Full Build

```bash
cd script/ci
ci.bat <project-list> <version> <software-name> <svn-path> [customized-name]
```

Project list files under `script/ci/projects/`:
- `all.txt` / `all_3.0.txt` — full build
- `client.txt` / `sensor.txt` — by service
- `tools.txt` — tools only
- `simulator.txt` — simulation build
- `test.txt` — test projects

### Output Directories

- `bin/win64_v141_Release/` — release binaries
- `bin/win64_v141_Debug/` — debug binaries
- `lib/win64_v141_release/` / `lib/win64_v141_debug/` — libraries

## High-Level Architecture

The project follows a 4-layer client-server architecture:

```
Application Layer  →  Component/Business Layer  →  RPC Layer  →  FPGA/Hardware Layer
```

For a complete directory layout, component map, operator list, path conventions, and more, see **[.claude/codebase-index.md](.claude/codebase-index.md)**.

### Key Design Patterns

`inspection_server` loads `specular_vision_gen.dll` and exposes its operators via RPC server. Each operator (`ImageStream`, `Inspect`, `Teach`, `DeviceControl`, etc.) has a corresponding `*_rpc_server.cpp` in `inspection_server/src/` that forwards RPC calls to the SVG implementation.

`specular_sensor_server` loads `specular_vision_sensor.dll` and similarly exposes sensor operators over RPC. The server itself is thin; heavy logic lives inside the sensor component.


## Testing
Each major component has a `test/` directory:

- `test/ut/` — unit tests (Visual Studio test projects)
- `test/sit/` — system/integration tests

Run by building the specific `.vcxproj` and executing the output binary. There is no unified test runner.

Examples:
- `components/business/specular_vision_gen/test/ut/database/ut.vcxproj`
- `tools/sv_aging_test_tool/test/ut/task/task.vcxproj`

## Tool Projects

Tools under `tools/` are auxiliary Qt-based utilities with their own build projects. For the complete tool list and paths, see [.claude/codebase-index.md](.claude/codebase-index.md#7-工具项目). A full tool map is also available at `tools/工具地图.txt`.

## Development Notes

- **Build dependency order**: `specular_vision_sensor` → `specular_vision_gen` → applications. CI enforces this via `projects/*.txt` ordering.
- **Custom infrastructure libraries** are preferred over direct STL in many components (`infrad`, `netd`, `logd`, `databased`, `imaged`, `jsond`).
- **Database**: SQLite with DAO pattern and connection pool (`DatabaseManager`).
- **Qt client**: Uses `GlobalApplication` instead of standard `QApplication`; licensed via HASP hardware dongle.
- **Sensor server variants**: `vs2017` (v3 sensor) and `vs2017_2.x` (legacy v2 sensor) directories coexist under `specular_sensor_server/project/`.
- **Version control**: `environment/set_version.bat` defines versions; CI calls per-project `modify_version_h.bat` to inject `version.h` before building.

## Coding Guidelines

Behavioral guidelines for code changes in this repository, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls. These bias toward caution over speed; for trivial tasks, use judgment.

### 1. Think Before Coding

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

### 3. Surgical Changes

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans (imports, variables, functions), remove them. Don't remove pre-existing dead code unless asked.

### 4. Goal-Driven Execution

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
### 5. 验证，而非假设
当修改一个跨层（数据结构 → RPC → 数据库 → 用户界面）的数据结构或行为时，不要止步于找到第一个匹配项。一个头文件中的结构体改动，往往需要在序列化、持久化存储和用户界面这三个层面进行相应的修改——每一层使用的机制都不同。在断定修改已完成之前，请追踪数据的完整生命周期。
```
