


![[Pasted image 20250427155539.png]]


ocean@ocean-VirtualBox:/opt/tempc_moniter_system/tools/paho.mqtt.c$ mkdir build
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/tools/paho.mqtt.c$ cd build
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/tools/paho.mqtt.c/build$ ls
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/tools/paho.mqtt.c/build$ cmake -DCMAKE_INSTALL_PREFIX=/opt/tempc_moniter_system/sc_lib/MQTT_pacho \
      -DCMAKE_C_COMPILER=/opt/tempc_moniter_system/tools/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc \
      -DCMAKE_CXX_COMPILER=/opt/tempc_moniter_system/tools/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-g++ \
      ..
-- The C compiler identification is GNU 7.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /opt/tempc_moniter_system/tools/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- CMake version: 3.22.1
-- CMake system name: Linux
-- Timestamp is 2025-04-27T08:09:25Z
-- Configuring done
-- Generating done
CMake Warning:
  Manually-specified variables were not used by the project:

    CMAKE_CXX_COMPILER


-- Build files have been written to: /opt/tempc_moniter_system/tools/paho.mqtt.c/build
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/tools/paho.mqtt.c/build$ make
[  1%] Building C object src/CMakeFiles/common_obj.dir/MQTTTime.c.o
[  3%] Building C object src/CMakeFiles/common_obj.dir/MQTTProtocolClient.c.o
[  4%] Building C object src/CMakeFiles/common_obj.dir/Clients.c.o
[  6%] Building C object src/CMakeFiles/common_obj.dir/utf-8.c.o
[  8%] Building C object src/CMakeFiles/common_obj.dir/MQTTPacket.c.o
[  9%] Building C object src/CMakeFiles/common_obj.dir/MQTTPacketOut.c.o
[ 11%] Building C object src/CMakeFiles/common_obj.dir/Messages.c.o
[ 12%] Building C object src/CMakeFiles/common_obj.dir/Tree.c.o
[ 14%] Building C object src/CMakeFiles/common_obj.dir/Socket.c.o
[ 16%] Building C object src/CMakeFiles/common_obj.dir/Log.c.o
[ 17%] Building C object src/CMakeFiles/common_obj.dir/MQTTPersistence.c.o
[ 19%] Building C object src/CMakeFiles/common_obj.dir/Thread.c.o
[ 20%] Building C object src/CMakeFiles/common_obj.dir/MQTTProtocolOut.c.o
[ 22%] Building C object src/CMakeFiles/common_obj.dir/MQTTPersistenceDefault.c.o
[ 24%] Building C object src/CMakeFiles/common_obj.dir/SocketBuffer.c.o
[ 25%] Building C object src/CMakeFiles/common_obj.dir/LinkedList.c.o
[ 27%] Building C object src/CMakeFiles/common_obj.dir/MQTTProperties.c.o
[ 29%] Building C object src/CMakeFiles/common_obj.dir/MQTTReasonCodes.c.o
[ 30%] Building C object src/CMakeFiles/common_obj.dir/Base64.c.o
[ 32%] Building C object src/CMakeFiles/common_obj.dir/SHA1.c.o
[ 33%] Building C object src/CMakeFiles/common_obj.dir/WebSocket.c.o
[ 35%] Building C object src/CMakeFiles/common_obj.dir/Proxy.c.o
[ 37%] Building C object src/CMakeFiles/common_obj.dir/StackTrace.c.o
[ 38%] Building C object src/CMakeFiles/common_obj.dir/Heap.c.o
[ 38%] Built target common_obj
[ 40%] Building C object src/CMakeFiles/paho-mqtt3a.dir/MQTTAsync.c.o
[ 41%] Building C object src/CMakeFiles/paho-mqtt3a.dir/MQTTAsyncUtils.c.o
[ 43%] Linking C shared library libpaho-mqtt3a.so
[ 43%] Built target paho-mqtt3a
[ 45%] Building C object src/CMakeFiles/paho-mqtt3c.dir/MQTTClient.c.o
[ 46%] Linking C shared library libpaho-mqtt3c.so
[ 46%] Built target paho-mqtt3c
[ 48%] Building C object src/CMakeFiles/MQTTVersion.dir/MQTTVersion.c.o
[ 50%] Linking C executable MQTTVersion
[ 50%] Built target MQTTVersion
[ 51%] Building C object test/CMakeFiles/thread.dir/thread.c.o
[ 53%] Building C object test/CMakeFiles/thread.dir/__/src/Thread.c.o
[ 54%] Linking C executable thread
[ 54%] Built target thread
[ 56%] Building C object test/CMakeFiles/test1.dir/test1.c.o
[ 58%] Linking C executable test1
[ 58%] Built target test1
[ 59%] Building C object test/CMakeFiles/test15.dir/test15.c.o
[ 61%] Linking C executable test15
[ 61%] Built target test15
[ 62%] Building C object test/CMakeFiles/test2.dir/test2.c.o
[ 64%] Linking C executable test2
[ 64%] Built target test2
[ 66%] Building C object test/CMakeFiles/test4.dir/test4.c.o
[ 67%] Linking C executable test4
[ 67%] Built target test4
[ 69%] Building C object test/CMakeFiles/test45.dir/test45.c.o
[ 70%] Linking C executable test45
[ 70%] Built target test45
[ 72%] Building C object test/CMakeFiles/test6.dir/test6.c.o
[ 74%] Linking C executable test6
[ 74%] Built target test6
[ 75%] Building C object test/CMakeFiles/test8.dir/test8.c.o
[ 77%] Linking C executable test8
[ 77%] Built target test8
[ 79%] Building C object test/CMakeFiles/test9.dir/test9.c.o
[ 80%] Linking C executable test9
[ 80%] Built target test9
[ 82%] Building C object test/CMakeFiles/test95.dir/test95.c.o
[ 83%] Linking C executable test95
[ 83%] Built target test95
[ 85%] Building C object test/CMakeFiles/test10.dir/test10.c.o
[ 87%] Linking C executable test10
[ 87%] Built target test10
[ 88%] Building C object test/CMakeFiles/test11.dir/test11.c.o
[ 90%] Linking C executable test11
[ 90%] Built target test11
[ 91%] Building C object test/CMakeFiles/test_issue373.dir/test_issue373.c.o
[ 93%] Linking C executable test_issue373
[ 93%] Built target test_issue373
[ 95%] Building C object test/CMakeFiles/test_sync_session_present.dir/test_sync_session_present.c.o
[ 96%] Linking C executable test_sync_session_present
[ 96%] Built target test_sync_session_present
[ 98%] Building C object test/CMakeFiles/test_connect_destroy.dir/test_connect_destroy.c.o
[100%] Linking C executable test_connect_destroy
[100%] Built target test_connect_destroy
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/tools/paho.mqtt.c/build$ make install
Consolidate compiler generated dependencies of target common_obj
[ 38%] Built target common_obj
Consolidate compiler generated dependencies of target paho-mqtt3a
[ 43%] Built target paho-mqtt3a
Consolidate compiler generated dependencies of target paho-mqtt3c
[ 46%] Built target paho-mqtt3c
Consolidate compiler generated dependencies of target MQTTVersion
[ 50%] Built target MQTTVersion
Consolidate compiler generated dependencies of target thread
[ 54%] Built target thread
Consolidate compiler generated dependencies of target test1
[ 58%] Built target test1
Consolidate compiler generated dependencies of target test15
[ 61%] Built target test15
Consolidate compiler generated dependencies of target test2
[ 64%] Built target test2
Consolidate compiler generated dependencies of target test4
[ 67%] Built target test4
Consolidate compiler generated dependencies of target test45
[ 70%] Built target test45
Consolidate compiler generated dependencies of target test6
[ 74%] Built target test6
Consolidate compiler generated dependencies of target test8
[ 77%] Built target test8
Consolidate compiler generated dependencies of target test9
[ 80%] Built target test9
Consolidate compiler generated dependencies of target test95
[ 83%] Built target test95
Consolidate compiler generated dependencies of target test10
[ 87%] Built target test10
Consolidate compiler generated dependencies of target test11
[ 90%] Built target test11
Consolidate compiler generated dependencies of target test_issue373
[ 93%] Built target test_issue373
Consolidate compiler generated dependencies of target test_sync_session_present
[ 96%] Built target test_sync_session_present
Consolidate compiler generated dependencies of target test_connect_destroy
[100%] Built target test_connect_destroy
Install the project...
-- Install configuration: ""
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/MQTTAsync_publish.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/MQTTAsync_publish_time.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/MQTTAsync_subscribe.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/MQTTClient_publish.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/MQTTClient_publish_async.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/MQTTClient_subscribe.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/paho_c_pub.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/paho_c_sub.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/paho_cs_pub.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/paho_cs_sub.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/samples/pubsub_opts.c
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/CONTRIBUTING.md
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/epl-v20
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/edl-v10
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/README.md
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/share/doc/Eclipse Paho C/notice.html
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/libpaho-mqtt3c.so.1.3.14
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/libpaho-mqtt3c.so.1
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/libpaho-mqtt3c.so
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/libpaho-mqtt3a.so.1.3.14
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/libpaho-mqtt3a.so.1
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/libpaho-mqtt3a.so
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/bin/MQTTVersion
-- Set runtime path of "/opt/tempc_moniter_system/sc_lib/MQTT_pacho/bin/MQTTVersion" to ""
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/include/MQTTAsync.h
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/include/MQTTClient.h
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/include/MQTTClientPersistence.h
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/include/MQTTProperties.h
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/include/MQTTReasonCodes.h
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/include/MQTTSubscribeOpts.h
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/include/MQTTExportDeclarations.h
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/cmake/eclipse-paho-mqtt-c/eclipse-paho-mqtt-cConfig.cmake
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/cmake/eclipse-paho-mqtt-c/eclipse-paho-mqtt-cConfig-noconfig.cmake
-- Installing: /opt/tempc_moniter_system/sc_lib/MQTT_pacho/lib/cmake/eclipse-paho-mqtt-c/eclipse-paho-mqtt-cConfigVersion.cmake
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/tools/paho.mqtt.c/build$
