# STM32 串口通信作业 — 提交说明

本目录是 STM32 串口通信课程作业的完整交付物，包含实验报告、STM32 工程源码与提交指引。

## 目录结构

```
STM32串口通信作业/
├── 实验报告.md          # 完整实验报告（理论 + 代码 + 步骤 + 结果分析）
├── README.md            # 本文件：使用与提交说明
└── code/
    ├── 任务一_轮询/main.c    # USART1 轮询发送 + #/* 启停控制
    ├── 任务二_中断/main.c    # 中断方式重做 #/* 启停控制
    ├── 任务二_DMA/main.c     # DMA 循环方式连续发送
    ├── usart.c / usart.h     # USART1 初始化（CubeMX 生成版，供参考）
```

## 如何使用代码（重要）

这些 `.c/.h` 文件需要配合 **STM32CubeMX 生成的完整工程** 使用，因为工程还包含 `stm32f1xx_hal_msp.c`、`gpio.c`、`dma.c`、`main.h`、启动文件、HAL 库等（由 CubeMX 自动生成）。

**正确流程：**

1. 打开 STM32CubeMX，新建工程，芯片选 `STM32F103C8Tx`。
2. 按 `实验报告.md` 第 4.2 节的步骤配置（SYS=Serial Wire、RCC=HSE 晶振、USART1=Asynchronous 115200-8-N-1、时钟 72MHz）。
   - 任务二(3) DMA：额外在 USART1 → DMA Settings 添加 `USART1_TX`（DMA1_Channel4，Memory→Peripheral，Circular，Byte）。
   - 任务二(1) 中断：额外在 USART1 → NVIC Settings 勾选 `USART1 global interrupt`。
3. Project Manager → Toolchain 选 `MDK-ARM V5`，生成代码。
4. 用 Keil 打开生成的工程，把对应任务目录下的 `main.c` **覆盖**工程里的 `main.c`（或将其中 `USER CODE` 段落复制进去）。
5. 编译、下载、用串口助手 sscom 观察。

> 注意：`usart.c/h` 只是参考，直接用 CubeMX 生成的即可（本目录提供的是「带 DMA + 中断」的完整版本，是各任务的超集）。

## 实验报告导出 PDF

`实验报告.md` 转 PDF，任选其一：

- **Typora**：打开 md → 文件 → 导出 → PDF。
- **VS Code**：安装 `Markdown PDF` 插件 → 打开 md → 右键 → Markdown PDF: Export (pdf)。
- **Pandoc**（若已安装）：
  ```bash
  pandoc 实验报告.md -o 实验报告.pdf --pdf-engine=xelatex -V CJKmainfont="SimSun"
  ```

## 打包提交

```bash
# 在项目父目录下执行，把整个目录打成 zip
cd C:\Users\Lenovo
# 用资源管理器右键 "STM32串口通信作业" → 压缩为 zip，或：
powershell Compress-Archive -Path "STM32串口通信作业" -DestinationPath "STM32串口通信作业.zip"
```

学习通提交内容：
- [ ] 项目代码 zip 包
- [ ] 实验报告 `实验报告.md`
- [ ] 实验报告 `实验报告.pdf`
- [ ] 博客地址（发布到 CSDN / 博客园）
- [ ] GitHub 仓库地址

## 实测部分说明

报告中的实测项已全部用**理论计算值/预估结果**填充（传输时间表、逻辑分析仪位周期 8.68 µs 等），并保留了操作步骤。拿到硬件后按步骤实测、把真实数据替换进去即可，重点关注：

1. 基础2：文件传输真实秒表时间（与理论值对比）。
2. 基础2：不接 GND 时的真实现象（理论预期：乱码/收不到）。
3. 任务一(1)：不加延时 vs 加延时的真实接收现象。
4. 任务三：逻辑分析仪实测位周期（理论值 8.68 µs @115200）。

## 建议的博客/GitHub 结构

- **博客**：把 `实验报告.md` 的内容发布，标题如《STM32 串口通信实验（USART/中断/DMA + RS232/RS485 电平）》。
- **GitHub 仓库**：`STM32-USART-Experiments`，上传 `code/` 下源码 + 报告，写 README 说明。

## 提交前自查清单

- [ ] 芯片型号、波特率参数（115200-8-N-1）与任务书一致
- [ ] 三个任务的 `main.c` 能编译通过
- [ ] 报告中的姓名/学号/班级已填写
- [ ] md 与 PDF 内容一致
- [ ] zip、md、PDF、博客、GitHub 五项齐全
