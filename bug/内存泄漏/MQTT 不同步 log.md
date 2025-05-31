

: 设备信息: name=502开关柜, id=SK0002, slave_addr=4
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建points数组
May 16 17:19:09 localhost sc_cfg_man[12933]: 处理测温点 1/1
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建测温点对象
May 16 17:19:09 localhost sc_cfg_man[12933]: 测温点信息: location=动触头上A相, frequency=1
May 16 17:19:09 localhost sc_cfg_man[12933]: 处理设备 3/3
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建设备对象
May 16 17:19:09 localhost sc_cfg_man[12933]: 设备信息: name=:#0111,#012#011#011#011#011#011"points":#011[{#012#011#011#011#011#011#011#011"location":#011"动触头上A相",#012#011#011#011#011#011#011#011"frequency":#0111#012#011#011#011#011#011#011}, {#012#011#011#011#011#011#011#011"location":#011"动▒▒#003, id=#011#011#011#011#011"frequency":#0111#012#011#011#011#011#011#011}, {#012#011#011#011#011#011#011#011"location":#011"动▒▒#003, slave_addr=151587081
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建points数组
May 16 17:19:09 localhost sc_cfg_man[12933]: 处理测温点 1/1814169865
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建测温点对象
May 16 17:19:10 localhost mosquitto[333]: 1747387150: Socket error on client cfg_man, disconnecting.
^C
[1]   Done                    sleep 8 && xrandr --output HDMI-1 --same-as DSI-1 --auto
[2]   Exit 127                sleep 9 && ./usr/bin/auto_qu.sh
[3]   Done                    sleep 6 && echo "RK809 sound set route ."
[4]-  Done                    sleep 6 && amixer -D hw:rockchiprk809co sset "Playback Path" "HP"
[5]+  Done                    sleep 6 && amixer -D hw:rockchiprk809co sset "Capture MIC Path" "Main Mic"
root@linux:~# tail -f /var/log/syslog
May 16 17:19:09 localhost sc_cfg_man[12933]: 处理测温点 1/1
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建测温点对象
May 16 17:19:09 localhost sc_cfg_man[12933]: 测温点信息: location=动触头上A相, frequency=1
May 16 17:19:09 localhost sc_cfg_man[12933]: 处理设备 3/3
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建设备对象
May 16 17:19:09 localhost sc_cfg_man[12933]: 设备信息: name=:#0111,#012#011#011#011#011#011"points":#011[{#012#011#011#011#011#011#011#011"location":#011"动触头上A相",#012#011#011#011#011#011#011#011"frequency":#0111#012#011#011#011#011#011#011}, {#012#011#011#011#011#011#011#011"location":#011"动▒▒#003, id=#011#011#011#011#011"frequency":#0111#012#011#011#011#011#011#011}, {#012#011#011#011#011#011#011#011"location":#011"动▒▒#003, slave_addr=151587081
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建points数组
May 16 17:19:09 localhost sc_cfg_man[12933]: 处理测温点 1/1814169865
May 16 17:19:09 localhost sc_cfg_man[12933]: 成功创建测温点对象
May 16 17:19:10 localhost mosquitto[333]: 1747387150: Socket error on client cfg_man, disconnecting.
May 16 17:22:39 localhost tempc_moniter[14288]: 温度监控系统启动...
May 16 17:22:39 localhost tempc_moniter[14288]: 打开温度数据库...
May 16 17:22:39 localhost tempc_moniter[14288]: 成功打开温度数据库
May 16 17:22:39 localhost tempc_moniter[14288]: 打开放电数据库...
May 16 17:22:39 localhost tempc_moniter[14288]: 成功打开放电数据库
May 16 17:22:39 localhost tempc_moniter[14288]: 打开历史数据库...
