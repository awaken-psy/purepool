# PurePool

[![C++17](https://img.shields.io/badge/C++-17-blue.svg)](https://en.wikipedia.org/wiki/C%2B%2B17) [![OpenGL](https://img.shields.io/badge/OpenGL-4.1-green.svg)](https://www.opengl.org/) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> **一个基于现代 OpenGL 的实时渲染项目，聚焦水体渲染、多 Pass 管线与物理一致的光照模型** 🌊  
> 项目涵盖水面反射 / 折射、PBR 材质、IBL 环境光照与阴影映射，面向图形学实践与课程项目场景。

![image-20251212060818079](./README.assets/screenshot.png)

动态效果展示:

![pure](./README.assets/pure.gif)

## ✨ 核心亮点

### 🌊 水体渲染（Water Rendering）

- **多 Pass 反射 / 折射管线**  
  基于 FBO 的多 Pass 渲染流程，分别生成水面反射与折射结果，并在最终 Pass 中进行视角相关混合。
- **Fresnel 菲涅尔效应**  
  使用 Schlick 近似，根据观察角度动态调节反射与折射权重。
- **DUDV 扰动与动态波动**  
  通过 DUDV 贴图与时间参数模拟水面流动与扰动。
- **基于深度的水体吸收**  
  从折射 Pass 的深度纹理估计水体厚度，并采用 Beer–Lambert 定律模拟深水变暗。

### 🎨 物理基础渲染（PBR）

- **Cook–Torrance BRDF（GGX）**  
  采用金属度 / 粗糙度工作流，实现符合能量守恒的表面反射模型。
- **法线贴图与 TBN 空间**  
  在世界空间中构建 TBN 矩阵，支持非一致缩放下的正确法线扰动。
- **多光源模型**  
  支持方向光、点光与聚光灯，并统一纳入 PBR 光照计算。

### 🌅 IBL 环境光照

- **HDR → Cubemap 转换**  
  在 GPU 上将经纬度 HDR 环境贴图转换为立方体贴图。
- **Irradiance / Prefilter / BRDF LUT 预计算**  
  完整实现基于 IBL 的漫反射与高光反射支持。
- **与直接光照协同**  
  IBL 提供环境能量分布，直接光照提供局部高频细节。

### 🌑 阴影映射

- **方向光 Shadow Map**  
  基于正交投影的深度贴图生成实时阴影。
- **PCF 软阴影**  
  通过多重采样平滑阴影边缘。
- **自适应光空间范围**  
  根据场景 AABB 自动调整阴影覆盖范围。

### 🧱 工程与架构

- **glTF 2.0 资产管线**  
  统一加载网格、材质、纹理、相机与灯光数据。
- **模块化渲染结构**  
  渲染、光照、阴影、水面等功能解耦实现。
- **跨平台支持**  
  macOS / Windows / Linux（OpenGL 4.1+）。

## 🚀 快速开始

```bash
# 克隆项目
git clone --recurse-submodules https://github.com/Yanyilucas/PurePool.git
cd PurePool

# 构建（推荐 out-of-source）
mkdir -p build && cd build
cmake .. && cmake --build .

# 运行
./PurePool
```

> ⚠️ 说明：当前资源路径基于相对路径 `../` 解析，默认建议在 `build/` 目录中运行程序。

## 🛠️ 技术栈

| 组件 | 版本 | 用途 |
|------|------|------|
| **C++** | 17+ | 核心语言 |
| **OpenGL** | 4.1+ | 图形渲染API |
| **CMake** | 3.10+ | 构建系统 |
| **GLFW** | 3.3+ | 窗口管理 |
| **GLM** | - | 数学库 |
| **tinygltf** | - | 3D模型加载 |
| **stb_image** | - | 纹理加载 |

## 📂 项目架构

```
PurePool/
├── code/                  # 源代码
│   ├── Inc/               # 头文件
│   ├── Src/               # 实现文件
│   └── Shaders/           # GLSL着色器
├── resources/             # 资源文件
│   ├── blender/           # 3D模型
│   ├── water/             # 水面贴图
│   ├── hdr/               # HDR环境光照贴图
│   └── skybox/            # 天空盒
└── external/              # 第三方库
```

## 🚧 未来计划

- [ ] 更高质量的水体效果（改进的波浪模型、屏幕空间反射）
- [ ] 点光 / 聚光阴影支持
- [ ] 实验性光线追踪或混合渲染
- [ ] 渲染调试与可视化工具


## 📄 开源协议

MIT License - 自由使用，无需署名

---

_PurePool 是一个面向实时图形学实践的 OpenGL 项目，适合作为课程设计、渲染实验或水体效果研究的基础工程 🎓_