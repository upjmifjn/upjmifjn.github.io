---
title: Linux禁用觸摸屏-鼠標亂點
date: 2018-12-03 08:47:47
tags: [禁用觸摸屏,鼠標亂點]
description: Debian 9.5禁用觸摸屏
---

1.安裝输入设备列表查看命令xinput
sudo apt-get install xinput

2.終端輸入xinput或xinput list，在列出的輸入設備列表中查找觸摸屏對應的名稱Atmel Atmel maXTouch Digitizer和id 12(註意：id每次插入新的硬件例如鼠標鍵盤等，可能會導致id號發生變化)
<!--more-->

3.設置觸摸屏的enabled屬性為0，禁用觸摸屏
xinput set-prop 12 'Device Enabled' 0
或
xinput set-prop 'Atmel Atmel maXTouch Digitizer' 'Device Enabled' 0

3、查看觸摸屏屬性列表
xinput list-props 12


重啓後會恢復默認屬性。所以用腳本/添加開機自啓來禁用觸摸屏比較好


--------------------腳本

#!/bin/bash
#sudo sh Touchscreen.sh off（注意后面的参数）
if [ $1 = 'on' ]
then
     xinput set-prop 'Atmel Atmel maXTouch Digitizer' 'Device Enabled' 1
    echo "觸摸屏開啓成功！"
elif [ $1 = 'off' ]
then
    xinput set-prop 'Atmel Atmel maXTouch Digitizer' 'Device Enabled' 0
    echo "觸摸屏關閉成功！"
else
    echo "請輸入參數：on/off"
    echo "開啓觸摸屏：TouchDigitizerEnabled on"
    echo "禁用觸摸屏：TouchDigitizerEnabled off"
fi

-----------------------------開機自動禁用觸摸屏

[Desktop Entry]
#開機自動禁用觸摸屏。在~/.config/autostart/下創建一個啟動器xinput.desktop文件，內容如下
Type=Application
Exec=xinput set-prop 'Atmel Atmel maXTouch Digitizer' 'Device Enabled' 0
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name[zh_TW]=touch digitizer enable
Name=touch digitizer enable
Comment[zh_TW]=禁用觸摸屏
Comment=禁用觸摸屏

--------------------------------------
