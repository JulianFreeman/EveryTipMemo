# EveryTipMemo

## 安装 KUbuntu 时不能创建加密分区的解决办法

安装前先选择 Try KUbuntu，然后在此试用系统中安装 ubiquity-frontend-gtk，然后在终端运行命令 `ubiquity gtk_ui`，注意后面是下划线不是中划线。然后在弹出的安装界面中就可以正常添加加密分区、设置密码了。

> 加密分区指 physical volume for encryption，分区设置完密码后会出现一个 `/dev/mapper/?`，在这下面点击添加根分区。
> 注意，留 500M 给 `/boot` 分区，**不要加密**

## Linux 挂载 U 盘后里面的中文文件名乱码的解决办法

```shell
sudo mount -o iocharset=utf8 /dev/sdb1 /mnt/some
```

## 在 Linux 补充缺失的字体

将字体文件复制到 `/home/user/.local/share/fonts/` 下，然后运行命令 `fc-cache -fv` 生成字体缓存，之后就可以用了。

## 使用 Batch 创建文件

```batch
type nul>*.*
```

## Linux 输入法设置

有时候安装了输入法也用不了，这时候主要是缺少设置，需要添加几行代码。可以在家目录的 `.bashrc` 文件内添加，或者为了所有用户都能用，在 `/etc/profile` 里添加也行。

```shell
export GTK_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export QT_IM_MODULE=fcitx
```

如果是 ibus，就把 `fcitx` 改成 `ibus`。然后注销，再登录。

## 检测 Batch 文件是否以管理员启动

```batch
@echo off

rem check if it is run as admin
if not "%1" == "am_admin" (
    rem you'd better keep the following line as it is.
    powershell start -verb runas '%0' am_admin & exit /b
)
```

## 检测 Shell 文件是否以管理员启动

```shell
#!/bin/bash

if [ "$UID" -ne "0" ]; then
    echo "Permission Denied. Try to run as root."
    exit
fi

# 以上条件也可以换为 "$(id -u)" -ne "0"
```

## 在 Windows 中创建快捷方式时不添加“-快捷方式”字样

找到注册表位置 `\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer`。点击 Explorer，在右侧找到 link，默认值为 `1e000000`，修改为 `00000000`，不需要重启。

## 通过注册表修改默认应用

打开注册表，进入 `HKEY_CLASSES_ROOT`，找到自己要修改的后缀名文件，查看其数据，比如 `.blend` 的数据是 blendfile，继续向下找 blendfile，找到 `shell/open/command`，在数据中修改执行文件路径即可。

