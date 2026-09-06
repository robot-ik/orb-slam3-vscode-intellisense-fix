
# ORB‑SLAM3 VSCode IntelliSense Fix
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> 💡 Problem: ORB‑SLAM3 compile successfully, but VSCode show lots of header file red squiggles.
> 适配系统：**Ubuntu 24.04** | ORB‑SLAM3 | VS Code C/C++ Extension

## 📝 现象描述
直接用 VSCode 打开 ORB‑SLAM3 源码：
- ✅ CMake + make 编译完全正常，项目可以正常运行
- ❌ VSCode 编辑器大量头文件报红
`#include "System.h"`、`#include "opencv2/opencv.hpp"` 提示找不到头文件

### 根本原因
> **编译通过 ≠ VSCode IntelliSense 可以识别头文件**

1. CMake/g++ 在编译阶段通过 `-I` 参数传入头文件搜索路径，编译器可以正确找到头文件；
2. VSCode 的 IntelliSense 是一套独立代码解析器，**不会自动读取 CMake 的编译参数**；
3. 没有告诉 VSCode 项目、第三方库头文件所在位置，因此出现大量红色报错波浪线。

## 🚀 完整修复流程

### 1. CMake 开启编译数据库导出
进入 ORB‑SLAM3 的 build 目录，执行 cmake，务必带上参数 `-DCMAKE_EXPORT_COMPILE_COMMANDS=ON`
```bash
cd build
cmake .. -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
make -j$(nproc)
