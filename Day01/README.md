````markdown
# 📝 Day01 - 运行最最简单的操作系统

## 1️⃣ 运行环境
- **操作系统**：Windows  
- **虚拟机**：VMware / QEMU  

---

## 2️⃣ 编译生成镜像
### 命令示例
```bash
..\z_tools\nask.exe helloos.nas helloos.img
````

### 命令解析

* `..\z_tools\nask.exe`：汇编器程序路径
* `helloos.nas`：汇编源代码
* `helloos.img`：生成的操作系统二进制映像文件

> ⚠️ 生成的 `.img` 文件是原始二进制映像（Raw Binary Image），无法直接在 Windows 资源管理器或虚拟光驱软件中打开。

### 为什么不是标准光盘映像？

1. **原始二进制映像**：直接生成软盘镜像，通常只有 1440 KB 大小。
2. **引导扇区**：前 512 字节为引导程序（Boot Sector），计算机 BIOS 或虚拟机直接加载执行。
3. **无标准文件系统**：虽然可能包含 FAT12 字符串，但没有真正的目录表或文件元数据。

---

## 3️⃣ 运行方法

### 方法一：在 VMware 上运行

1. 新建一个虚拟机（任意操作系统类型均可）
2. 添加新的软盘，导入 `helloos.img` 或由 `asm.bat` 生成的镜像
3. 启动虚拟机，镜像内的引导程序将自动执行

### 方法二：在 QEMU 模拟器运行

#### 步骤 1：复制镜像

```bash
copy helloos.img ..\z_tools\qemu\fdimage0.bin
```

* `copy`：Windows 文件复制命令
* `..\z_tools\qemu\fdimage0.bin`：QEMU 期望的软盘文件位置和名称

#### 步骤 2：启动模拟器

```bash
..\z_tools\make.exe -C ../z_tools/qemu
```

* `make.exe`：构建工具，读取 `Makefile` 自动启动 QEMU
* `-C ../z_tools/qemu`：切换到 QEMU 目录执行构建和启动命令

> ⚠️ 注：过去可以用 `..\z_tools\imgtol.com w a: helloos.img` 写入物理软盘，但在现代 Windows 系统上不再推荐使用。

---

## 4️⃣ 开发循环（Development Loop）

1. **编辑源代码** (`zsos.nas`)

   * 使用编辑器（VS Code、Notepad++ 等）修改汇编代码
2. **编译镜像** (`asm2.bat`)

   ```bash
   ..\z_tools\nask.exe zsos.nas zsos.img
   ```

   * 将汇编代码翻译成机器码，生成软盘镜像
3. **运行操作系统** (`run2.bat`)

   ```bash
   copy zsos.img ..\z_tools\qemu\fdimage0.bin
   ..\z_tools\make.exe -C ../z_tools/qemu
   ```

   * 自动复制镜像到 QEMU 指定位置并启动虚拟机
   * 你写在 `zsos.nas` 中的程序将在虚拟机中运行

---

## 5️⃣ 注意事项

* `.img` 文件是原始二进制，非 ISO 格式
* 每次修改源代码后，需要重新编译生成镜像
* 在 QEMU 或 VMware 中运行时，确保镜像路径正确
* 具体内容--见word！！

```
---
---

