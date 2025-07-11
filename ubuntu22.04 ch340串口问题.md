# 固定 CH340 串口设备名（Ubuntu 22.04）

## 📌 项目目的

在使用多个 CH340 USB 转串口模块时，Ubuntu 会自动分配 `/dev/ttyUSB0`、`/dev/ttyUSB1` 等动态名称。为避免设备号变化造成程序出错，本项目通过 `udev` 规则为不同 CH340 设备绑定固定名称：

- 第一个设备：`/dev/ttyUSB_A`
- 第二个设备：`/dev/ttyUSB_B`

---

## 🧭 操作流程

### 1️⃣ 插入第一个 CH340

- 确保系统中只有一个 CH340 插入。
- 查看它分配的串口名（例如 `/dev/ttyUSB0`）：

```bash
ls /dev/ttyUSB*
```

- 获取其物理端口路径：

```bash
udevadm info --name=/dev/ttyUSB0 --attribute-walk | grep "KERNELS"
```

- 假设输出为：`KERNELS=="3-7"`，请记下。

---

### 2️⃣ 插入第二个 CH340

- 插入第二个模块，查看其设备路径：

```bash
ls /dev/ttyUSB*
udevadm info --name=/dev/ttyUSB1 --attribute-walk | grep "KERNELS"
```

- 假设输出为：`KERNELS=="3-9"`。

---

### 3️⃣ 创建 Udev 规则文件

编辑规则文件：

```bash
sudo nano /etc/udev/rules.d/99-ch340.rules
```

添加以下内容：

```bash
# 第一个 CH340，接在 USB 3-7
SUBSYSTEM=="tty", KERNELS=="3-7", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", SYMLINK+="ttyUSB_A"

# 第二个 CH340，接在 USB 3-9
SUBSYSTEM=="tty", KERNELS=="3-9", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", SYMLINK+="ttyUSB_B"
```

保存并退出（`Ctrl + O`, 回车，`Ctrl + X`）。

---

### 4️⃣ 重载 udev 规则并触发

```bash
sudo udevadm control --reload
sudo udevadm trigger
```

或者重新插拔 CH340 模块使规则生效。

---

### 5️⃣ 验证设备命名是否成功

```bash
ls -l /dev/ttyUSB_A
ls -l /dev/ttyUSB_B
```

应看到类似输出：

```
/dev/ttyUSB_A -> ttyUSB0
/dev/ttyUSB_B -> ttyUSB1
```

---

## ✅ 使用建议

- 在 ROS 或其他串口程序中，统一使用 `/dev/ttyUSB_A` 或 `/dev/ttyUSB_B`，无需担心顺序变化。
- 如果更换 USB 插口，请重新执行步骤 1 和 2 获取新的 `KERNELS`。

---

## 💡 附：一键设置命令（如确定端口号）

```bash
echo 'SUBSYSTEM=="tty", KERNELS=="3-7", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", SYMLINK+="ttyUSB_A"' | sudo tee /etc/udev/rules.d/99-ch340.rules

echo 'SUBSYSTEM=="tty", KERNELS=="3-9", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", SYMLINK+="ttyUSB_B"' | sudo tee -a /etc/udev/rules.d/99-ch340.rules

sudo udevadm control --reload
sudo udevadm trigger
```

---

## 📂 文件位置说明

- Udev规则路径：`/etc/udev/rules.d/99-ch340.rules`
- 设备软链接：`/dev/ttyUSB_A`、`/dev/ttyUSB_B`

---

## 📞 联系

如遇问题欢迎提 Issue 或 PR。
