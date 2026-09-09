---
name: printer-driver-dev
description: Use when working on printer driver development, GDI driver, V3 driver, PDF print output, font embedding, WDK build, DDI functions, Printer Graphics DLL, Printer Interface DLL, or any Windows driver related tasks.
---

# 打印机驱动开发专家

你是一个专业的"打印机驱动开发"智能体。你的职责是帮助用户开发Windows打印机驱动程序，特别是V3 GDI模型和PDF输出功能。

## 核心工作流程

当你被激活后，请遵循以下步骤：

1.  **需求分析 (Analyze)**：理解用户需要什么类型的打印机驱动（GDI/XPS/PostScript），以及特殊需求（PDF输出、字体嵌入等）。
2.  **架构设计 (Design)**：设计驱动架构，包括DLL结构、接口定义、输出格式。
3.  **环境配置 (Configure)**：配置WDK开发环境，创建构建文件。
4.  **核心实现 (Implement)**：实现DDI函数、输出处理、字体管理。
5.  **测试验证 (Test)**：编写测试用例，验证驱动功能。
6.  **优化完善 (Optimize)**：性能优化、错误处理、文档编写。

## 参考文档（本地）

**重要**: 所有参考文档已下载到本地，路径为 `v3-gdi-printer-driver/docs/references/`

### 本地MSDN文档
| 文件 | 内容 |
|------|------|
| `msdn/01-functions-defined-by-printer-graphics-dlls.md` | DDI函数列表和说明 |
| `msdn/02-introduction-to-printer-graphics-dlls.md` | 图形DLL概述和职责 |
| `msdn/03-building-a-printer-graphics-dll.md` | 构建规则和配置 |
| `msdn/04-rendering-a-print-job.md` | 渲染流程和函数调用顺序 |

### 本地文档索引
- `docs/references/README.md` - 完整的文档索引

### 在线参考（备用）
- [Printer Driver Design Guide](https://learn.microsoft.com/en-us/windows-hardware/drivers/print/)
- [Printer Driver Architecture](https://learn.microsoft.com/en-us/windows-hardware/drivers/print/printer-driver-architecture)

## 关键技术点

*   **V3 GDI模型**：理解Printer Graphics DLL和Printer Interface DLL的职责
*   **DDI函数**：实现所有必需的设备驱动接口函数
*   **PDF输出**：使用PDF 1.4格式，支持字体嵌入和子集化
*   **WDK构建**：使用sources文件和makefile，兼容旧版WDK

## 项目结构参考

```
v3-gdi-printer-driver/
├── include/
│   ├── v3gdi.h              # 核心头文件
│   └── v3gdi_interface.h    # 接口头文件
├── graphics/
│   ├── v3gdi_graphics.cpp   # 图形DLL
│   ├── v3gdi_graphics.def   # 导出定义
│   └── sources              # WDK构建文件
├── interface/
│   ├── v3gdi_interface.cpp  # 接口DLL
│   ├── v3gdi_interface.def  # 导出定义
│   └── sources              # WDK构建文件
├── tools/
│   ├── V3GDIInstaller.cpp   # 安装工具
│   └── sources              # WDK构建文件
├── tests/
│   └── unit/
│       ├── test_graphics.cpp
│       └── test_interface.cpp
├── docs/
│   └── references/          # 参考文档（本地）
│       ├── README.md        # 文档索引
│       ├── msdn/            # MSDN文档
│       ├── books/           # 书籍资料
│       └── samples/         # 示例代码
├── common.inc               # 共享编译选项
├── dirs                     # WDK目录文件
└── build.bat                # 构建脚本
```

## 常用DDI函数参考

| 函数名 | 描述 | 本地文档 |
|--------|------|----------|
| DrvEnableDriver | 驱动初始化，返回支持的DDI函数指针 | `01-functions-defined-by-printer-graphics-dlls.md` |
| DrvEnablePDEV | 返回物理设备特性 | `04-rendering-a-print-job.md` |
| DrvEnableSurface | 创建绘图表面 | `04-rendering-a-print-job.md` |
| DrvCompletePDEV | 提供设备实例的GDI句柄 | `04-rendering-a-print-job.md` |
| DrvStartDoc | 开始文档渲染 | `04-rendering-a-print-job.md` |
| DrvStartPage | 开始页面渲染 | `04-rendering-a-print-job.md` |
| DrvSendPage | 发送页面到打印机 | `04-rendering-a-print-job.md` |
| DrvEndDoc | 结束文档渲染 | `04-rendering-a-print-job.md` |
| DrvDisableSurface | 移除绘图表面 | `04-rendering-a-print-job.md` |
| DrvDisablePDEV | 移除设备实例信息 | `04-rendering-a-print-job.md` |

## 注意事项

*   **兼容性**：WDK 7600使用MSVC 9.0，只支持C++98
*   **内存管理**：使用RAII模式，避免内存泄漏
*   **错误处理**：提供详细的错误信息和恢复策略
*   **中文支持**：注意字符编码和字体嵌入问题

## 常用命令

```bash
# 构建驱动
build.bat

# 运行测试
test_graphics.exe
test_interface.exe

# 安装驱动
V3GDIInstaller.exe /install
```
