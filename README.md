# ICPPrePost - 有限元前后处理软件

## 📋 目录

- [简介](#简介)
- [功能特性](#功能特性)
- [系统要求](#系统要求)
- [安装指南](#安装指南)
- [快速开始](#快速开始)
- [开发指南](#开发指南)
- [API文档](#api文档)
- [插件开发](#插件开发)
- [常见问题](#常见问题)
- [贡献指南](#贡献指南)

## 🎯 简介

ICPPrePost 是一个功能完整的有限元分析前后处理软件，基于Qt、VTK、OpenCascade和Gmsh等开源技术构建。它提供了直观的用户界面，强大的几何建模能力，高质量的网格生成，以及丰富的后处理功能。

### 核心特点

- **跨平台支持**：Windows、Linux、macOS
- **模块化架构**：易于扩展和维护
- **插件系统**：支持Python脚本和C++插件
- **多求解器支持**：Calculix、Elmer、Code_Aster等
- **现代化UI**：基于Qt6的原生界面

## ✨ 功能特性

### 前处理功能

#### 几何建模
- ✅ 基本几何创建（立方体、圆柱、球体、圆锥）
- ✅ 布尔操作（并集、差集、交集）
- ✅ STEP/IGES导入导出
- ✅ 几何修复和简化
- ✅ 参数化建模支持

#### 网格生成
- ✅ 四面体/六面体网格
- ✅ 自适应网格细化
- ✅ 网格质量控制
- ✅ 边界层网格
- ✅ 并行网格生成

#### 材料和边界条件
- ✅ 材料库管理
- ✅ 各向同性/正交各向异性材料
- ✅ 多种边界条件类型
- ✅ 载荷工况管理
- ✅ 接触定义

### 求解器接口
- ✅ Calculix集成
- ✅ 作业队列管理
- ✅ 并行计算支持
- ✅ 远程求解支持
- ✅ 实时进度监控

### 后处理功能
- ✅ 云图显示
- ✅ 矢量场可视化
- ✅ 变形动画
- ✅ 等值面/线提取
- ✅ 数据曲线提取
- ✅ 自定义报告生成

## 💻 系统要求

### 最低配置
- **操作系统**：Windows 10/11, Ubuntu 20.04+, macOS 10.15+
- **处理器**：双核 2.0 GHz
- **内存**：4 GB RAM
- **显卡**：支持OpenGL 3.3
- **硬盘**：2 GB可用空间

### 推荐配置
- **处理器**：四核 3.0 GHz+
- **内存**：16 GB RAM
- **显卡**：独立显卡，2GB+ VRAM
- **硬盘**：SSD，10 GB可用空间

## 📥 安装指南

### Windows安装


### Linux安装

#### Ubuntu/Debian


## 🚀 快速开始



## 🔧 开发指南

### 项目结构

```
ICPPrePost/
├── src/                    # 源代码
│   ├── core/              # 核心模块
│   ├── gui/               # 用户界面
│   ├── geometry/          # 几何引擎
│   ├── meshing/           # 网格模块
│   ├── preprocessing/     # 前处理
│   ├── postprocessing/    # 后处理
│   ├── solver/            # 求解器接口
│   └── plugin/            # 插件系统
├── plugins/               # 插件目录
├── tests/                 # 单元测试
├── docs/                  # 文档
└── resources/             # 资源文件
```

### 编码规范

```cpp
// 文件命名：PascalCase
// MyClassName.h, MyClassName.cpp

// 类命名：PascalCase
class GeometryEngine {
public:
    // 公共方法：camelCase
    void generateMesh();
    
    // 获取器/设置器
    int getId() const { return m_id; }
    void setId(int id) { m_id = id; }

private:
    // 私有成员：m_前缀
    int m_id;
    std::string m_name;
};

// 命名空间：小写
namespace utils {
    // 自由函数：camelCase
    double calculateVolume(const Shape& shape);
}

// 常量：大写下划线
const int MAX_ITERATIONS = 100;
```

### 构建系统

项目使用CMake作为构建系统：

```cmake
# 添加新模块
add_library(my_module
    MyClass.cpp
    MyClass.h
)

target_link_libraries(my_module
    PUBLIC
        Qt6::Core
        geometry_lib
    PRIVATE
        ${VTK_LIBRARIES}
)

# 添加测试
add_test(NAME MyModuleTest 
         COMMAND test_my_module)
```

## 📚 API文档

### 几何模块

```cpp
// 创建基本几何
auto engine = std::make_unique<GeometryEngine>();
auto box = engine->createBox(10, 20, 30);
auto cylinder = engine->createCylinder(5, 15);

// 布尔操作
auto result = engine->fuse(box.get(), cylinder.get());

// 导入/导出
std::vector<std::shared_ptr<GeometryItem>> items;
engine->importSTEP("model.step", items);
engine->exportSTEP("output.step", items);
```

### 网格模块

```cpp
// 初始化网格引擎
MeshEngine meshEngine;
meshEngine.initialize();

// 设置参数
MeshParameters params;
params.elementType = MeshParameters::Tetrahedral;
params.globalSize = 1.0;
params.optimizationSteps = 3;

// 生成网格
auto mesh = meshEngine.generateMesh(geometry, params);

// 检查质量
auto report = meshEngine.checkMeshQuality(mesh.get());
std::cout << "平均质量: " << report.avgQuality << std::endl;
```

### 求解器接口

```cpp
// 创建求解器
auto solver = std::make_unique<CalculixInterface>();
solver->setExecutablePath("/usr/bin/ccx");
solver->setNumProcessors(4);

// 生成输入文件
solver->generateInputFile(project, loadCase, "input.inp");

// 运行求解
connect(solver.get(), &SolverInterface::progressUpdated,
        [](double progress) {
            std::cout << "进度: " << progress << "%" << std::endl;
        });

solver->start();
```

## 🔌 插件开发

### 创建简单插件

```cpp
// MyPlugin.h
#pragma once

#include "plugin/PluginInterface.h"
#include <QObject>

class MyPlugin : public QObject, public IPlugin
{
    Q_OBJECT
    Q_PLUGIN_METADATA(IID PluginInterface_iid FILE "myplugin.json")
    Q_INTERFACES(IPlugin)

public:
    // IPlugin接口
    PluginMetadata getMetadata() const override;
    bool initialize() override;
    void shutdown() override;
    void setupMenu(QMenu* menu) override;
};

// MyPlugin.cpp
#include "MyPlugin.h"
#include <QMenu>
#include <QAction>
#include <QMessageBox>

PluginMetadata MyPlugin::getMetadata() const
{
    PluginMetadata meta;
    meta.name = "我的插件";
    meta.description = "插件示例";
    meta.version = "1.0.0";
    meta.author = "开发者";
    meta.category = "Tools";
    return meta;
}

bool MyPlugin::initialize()
{
    // 初始化插件资源
    return true;
}

void MyPlugin::shutdown()
{
    // 清理资源
}

void MyPlugin::setupMenu(QMenu* menu)
{
    QAction* action = menu->addAction("我的功能");
    connect(action, &QAction::triggered, []() {
        QMessageBox::information(nullptr, "插件", "Hello from plugin!");
    });
}
```

### 插件配置文件

```json
{
    "name": "MyPlugin",
    "version": "1.0.0",
    "compatibleVersions": ["1.0.0", "1.1.0"],
    "dependencies": [],
    "autoLoad": true
}
```

## ❓ 常见问题

### Q: 如何提高大模型的显示性能？

A: 可以尝试以下方法：
1. 启用LOD（细节层次）：视图 → 性能设置 → 启用LOD
2. 减少显示的单元边：视图 → 显示选项 → 关闭边显示
3. 使用简化显示模式：在大模型时自动切换到点云模式

### Q: 网格生成失败怎么办？

A: 常见原因和解决方法：
1. **几何错误**：使用几何 → 检查和修复工具
2. **尺寸不匹配**：调整网格尺寸参数
3. **内存不足**：减小网格密度或使用64位版本

### Q: 如何添加自定义材料？

A: 两种方法：
1. **GUI方式**：材料库 → 新建 → 输入属性
2. **配置文件**：编辑 `~/.icpprepost/materials.xml`

### Q: 支持哪些求解器？

A: 当前支持：
- ✅ Calculix (完全支持)
- ✅ Elmer (基本支持)
- 🚧 Code_Aster (开发中)
- 🚧 OpenFOAM (计划中)

## 🤝 贡献指南

我们欢迎所有形式的贡献！

### 报告问题
1. 检查是否已有相似issue
2. 提供详细的复现步骤
3. 包含系统信息和错误日志

### 提交代码
1. Fork项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建Pull Request

### 开发环境设置



## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 🙏 致谢

- Qt Project - 跨平台UI框架
- VTK - 可视化工具包  
- OpenCascade - CAD内核
- Gmsh - 网格生成器
- 所有贡献者和用户

## 📞 联系方式

- 官网：
- 邮箱：
- 论坛：
- QQ群：

---

© 2025 MyCompany. All rights reserved.
