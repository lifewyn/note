---
time: 2025-06-29
---


asus@asus-VirtualBox:/opt/lib60870/lib60870-C$ make dynlib gcc -lpthread -shared -o ./build/liblib60870.so ./build/src/common/linked_list.o ./build/src/iec60870/frame.o ./build/src/iec60870/lib60870_common.o ./build/src/iec60870/cs101/cs101_asdu.o ./build/src/iec60870/cs101/cs101_bcr.o ./build/src/iec60870/cs101/cs101_information_objects.o ./build/src/iec60870/cs101/cs101_master.o ./build/src/iec60870/cs101/cs101_master_connection.o ./build/src/iec60870/cs101/cs101_queue.o ./build/src/iec60870/cs101/cs101_slave.o ./build/src/iec60870/cs104/cs104_connection.o ./build/src/iec60870/cs104/cs104_frame.o ./build/src/iec60870/cs104/cs104_slave.o ./build/src/iec60870/link_layer/buffer_frame.o ./build/src/iec60870/link_layer/link_layer.o ./build/src/iec60870/link_layer/serial_transceiver_ft_1_2.o ./build/src/iec60870/apl/cpXXtime2a.o ./build/src/hal/socket/linux/socket_linux.o ./build/src/hal/thread/linux/thread_linux.o ./build/src/hal/time/unix/time.o ./build/src/hal/serial/linux/serial_port_linux.o ./build/src/hal/memory/lib_memory.o -lpthread /usr/bin/ld: ./build/src/iec60870/cs101/cs101_information_objects.o: warning: relocation against `doubleCommandVFT' in read-only section `.text' /usr/bin/ld: ./build/src/iec60870/cs101/cs101_asdu.o: relocation R_X86_64_PC32 against symbol `asduFrameVFT' can not be used when making a shared object; recompile with -fPIC /usr/bin/ld: final link failed: bad value collect2: error: ld returned 1 exit status make: *** [Makefile:149: build/liblib60870.so] Error 1 asus@asus-VirtualBox:/opt/lib60870/lib60870-C$


这个错误是因为编译动态库时需要位置无关代码（PIC），但当前的编译配置没有启用 -fPIC 标志。让我帮您解决这个问题。

Read Makefile

我看到问题了。Makefile中确实有 dynlib: CFLAGS += -fPIC 这一行，但是问题在于已经编译好的目标文件没有使用 -fPIC 标志。我们需要先清理之前的编译结果，然后重新编译。