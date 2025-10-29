# Maven 测试中文乱码解决方案

## 问题描述

在 Windows 系统运行 `mvn test` 时，中文日志显示乱码。

## 解决方案

### 1. PowerShell 配置

#### 步骤：

1. 打开 PowerShell，执行以下命令创建配置文件：
   ```powershell
   New-Item -Path $PROFILE -Type File -Force
   ```

2. 编辑配置文件：
   ```powershell
   notepad $PROFILE
   ```

3. 在打开的文件中添加以下内容：
   ```powershell
   # 设置 UTF-8 编码
   $OutputEncoding = [System.Text.Encoding]::UTF8
   [Console]::OutputEncoding = [System.Text.Encoding]::UTF8
   [Console]::InputEncoding = [System.Text.Encoding]::UTF8
   chcp 65001 > $null

   # 可选：显示提示信息（如果不想看到提示，可以注释掉下面这行）
   Write-Host "✓ 已自动设置 UTF-8 编码" -ForegroundColor Green
   ```

4. 保存文件并关闭

5. 重新打开 PowerShell，编码自动生效

#### 验证：
```powershell
chcp
# 应显示：Active code page: 65001
```

---

### 2. CMD 配置

#### 步骤：

1. 按 `Win + R`，输入 `regedit`，打开注册表编辑器

2. 导航到以下路径：
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor
   ```
   **说明**：`HKEY_LOCAL_MACHINE` 为全局配置，影响所有用户，需要管理员权限

3. 右键点击 `Command Processor`，选择 **新建** → **字符串值**

4. 名称输入：`AutoRun`

5. 双击 `AutoRun`，数值数据输入：`chcp 65001`

6. 点击确定，关闭注册表编辑器

#### 验证：

1. 打开新的 CMD 窗口
2. 会看到提示：`Active code page: 65001`
3. 输入 `chcp` 确认显示 `65001`

---

### 3. IntelliJ IDEA 配置

#### 步骤：

1. 打开 IDEA 设置：
   - **快捷键**：`Ctrl + Alt + S` (Windows/Linux) 或 `Cmd + ,` (Mac)
   - **菜单**：`File` → `Settings`

2. 导航到：
   ```
   Build, Execution, Deployment → Build Tools → Maven → Runner
   ```

3. 在 **VM Options** 输入框中添加：
   ```
   -Dfile.encoding=UTF-8
   ```

4. 点击 **OK** 保存

5. **无需重启** IDEA，立即生效

#### 验证：

在 IDEA 中右键项目运行 `Maven Test`，查看控制台输出，中文应正常显示。

---

## 配置总结

| 环境 | 配置位置 | 是否永久 | 是否需要重启 |
|------|---------|---------|-------------|
| **PowerShell** | `$PROFILE` 配置文件 | ✅ 永久 | ❌ 否（重新打开窗口） |
| **CMD** | 注册表 AutoRun | ✅ 永久 | ❌ 否（重新打开窗口） |
| **IDEA** | Maven Runner VM Options | ✅ 永久 | ❌ 否 |

---

## 常见问题

### Q1: PowerShell 提示无法加载配置文件？

**问题**：`无法加载文件 xxx.ps1，因为在此系统上禁止运行脚本`

**解决**：设置执行策略
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Q2: 如何查看 PowerShell 配置文件路径？

```powershell
$PROFILE
```

### Q3: 如何编辑 PowerShell 配置文件？

```powershell
notepad $PROFILE
```

### Q4: CMD 配置后如何验证注册表？

1. 按 `Win + R`，输入 `regedit`
2. 导航到：`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor`
3. 查看 `AutoRun` 的值是否为 `chcp 65001`

### Q5: PowerShell 脚本文件中的中文显示乱码？

**问题**：运行 PowerShell 脚本（.ps1 文件）时，脚本中的中文字符串显示乱码，但 Maven 输出正常。

**原因**：脚本文件本身的编码格式不正确。

**解决方法**：

1. 在 Cursor/VS Code 中打开脚本文件（如 `run-test.ps1`）
2. 查看右下角状态栏的编码显示
3. 如果显示 `UTF-8` 或 `GBK`，点击编码名称
4. 选择 **"Save with Encoding"**（通过编码保存）
5. 选择 **"UTF-8 with BOM"**
6. 保存文件

**为什么需要 BOM？**
- PowerShell 在 Windows 上需要 BOM（Byte Order Mark）来识别 UTF-8 文件
- 没有 BOM，PowerShell 会用系统默认编码（GBK）读取脚本
- 导致脚本中的中文字符串在加载时就被错误解析

**验证**：重新运行脚本，中文应该正常显示。

### Q6: IDEA 配置后在命令行运行还是乱码？

IDEA 的配置仅影响 IDEA 内部运行 Maven。
命令行需要单独配置 PowerShell 或 CMD。

---

## 完成！

配置完成后：
- ✅ PowerShell 中运行 `mvn test` 中文正常
- ✅ CMD 中运行 `mvn test` 中文正常  
- ✅ IDEA 中点击 Maven Test 中文正常

