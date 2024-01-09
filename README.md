# 暨南大学Linux锐捷

## 常规下载并执行脚本

这个仓库主要记录了如何在ubuntu22.04系统下创建锐捷启动的快捷方式，并且无窗口后台运行。

如何在ubuntu22.04系统下启动锐捷联网就不赘述了，主要流程如下

1. 在这个[链接](https://mynet.jnu.edu.cn/app/customer/fileHandle/downloadFile?fileName=Ruijie_Supplicant(Linux_student).zip)下载zip压缩包并解压。
2. 按照里面的readme文件如下操作
        
        chmod +x ./rjsupplicant.sh
        sudo ./rjsupplicant.sh -a 1 -d 1 -u 你的学号 -p 你的密码

## 创建桌面快捷无后台执行

这里需要创建3个文件
1. **ruijie.sh**脚本文件来后台运行**rjsupplicant.sh**。
2. **ruijie_wrapper.sh**脚本文件来封装**ruijie.sh**(直接执行**ruijie.sh**会无法正常后台执行，原因未知)。
3. **ruijie.desktop**桌面快捷来执行**ruijie_wrapper.sh**，实现双击桌面快捷即可。


**ruijie.desktop**文件内容如下

    [Desktop Entry]
    Version=1.0
    Exec= <ruijie_wrapper.sh文件绝对路径>
    Name=Ruijie
    GenericName=SSH Server
    Comment=Connect to My Server
    Encoding=UTF-8
    Terminal=true
    Type=Application
    Icon= <这个仓库附带logo的绝对路径(不需要可以删除这行)>
    Categories=Application;Network;

**ruijie.wrapper.sh**文件内容如下

    #!/bin/bash
    cd <ruijie.sh文件绝对路径>
    sudo ./ruijie.sh

**ruijie.sh**文件内容如下

    #!/bin/bash
    cd <rjsupplicant.sh文件绝对路径>

    echo "Turn on/off Ruijie? [On/off]"
    read Operation

    if [[ -n $Operation && ${Operation,,} = 'off' ]]
    then
        sudo ./rjsupplicant.sh -q
    else
    (sudo ./rjsupplicant.sh -a 1 -d 1 -u 你的学号 -p 你的密码 &)
    fi

以上步骤完成之后直接双击桌面**ruijie.desktop**文件即可，这里双击后会需要输入密码，然后选择是开启还是关闭锐捷。
