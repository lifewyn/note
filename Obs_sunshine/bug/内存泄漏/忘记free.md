---
time: 2025-05-20
---

Read file: /opt/tempc_moniter_system/source/sc_cfg_man/cfg_man_lib.c
你的怀疑**是有道理的**，并且代码确实存在**内存泄漏和潜在的悬挂指针问题**，这很可能导致你日志中出现的乱码、异常负数等现象。

### 详细分析

#### 1. `struct tempc_set current_tempc_set` 的分配与释放

- 在 `sync_data_tempc` 函数中，`current_tempc_set` 是**栈变量**，但其内部的 `tempc_node_node` 和每个 `sensor_unit_node` 都是通过 `malloc` 动态分配的。
- 但是**整个函数结束后，并没有对这些动态分配的内存进行释放**！

#### 2. 代码片段举例

```c
struct tempc_set current_tempc_set;
memset(&current_tempc_set, '\0', sizeof(struct tempc_set));
// ...
ret = sync_tempc_buffer(&current_tempc_set);
// ...
return 0; // 没有释放current_tempc_set内部的malloc内存
```

在 `sync_tempc_buffer` 里有如下分配：

```c
current_tempc_set->tempc_node_node = malloc(...);
for (每个设备) {
    current_tempc_set->tempc_node_node[device_count].sensor_unit_node = malloc(...);
}
```

#### 3. 后果

- **内存泄漏**：每次调用 `sync_data_tempc`，都会分配新内存，但永远不会释放，长时间运行后会耗尽内存。
- **悬挂指针/脏数据**：如果 `current_tempc_set` 被多次复用，未释放的旧指针可能被覆盖或误用，导致数据错乱、乱码、异常负数等。

---
![[Pasted image 20250520135501.png]]

### 你的问题回答

> 和在sync_data_tempc(mqtt_handle_t *mqtt_handle)中没有释放struct tempc_set current_tempc_set的指针有关系吗？

**是的，非常有关系！**
- 如果不释放 `tempc_node_node` 及其内部的 `sensor_unit_node`，会导致内存泄漏和数据错乱，进而出现你日志中的乱码、异常负数等现象。

---

### 建议修复方法

在 `sync_data_tempc` 结尾**增加如下释放代码**：

```c
for (int i = 0; i < current_tempc_set.device_num; i++) {
    if (current_tempc_set.tempc_node_node[i].sensor_unit_node != NULL) {
        free(current_tempc_set.tempc_node_node[i].sensor_unit_node);
    }
}
if (current_tempc_set.tempc_node_node != NULL) {
    free(current_tempc_set.tempc_node_node);
}
```

**务必保证每次malloc都能被free！**

---

### 总结

- 你的问题和`current_tempc_set`内存未释放高度相关。
- 建议立即修复，防止更多内存泄漏和数据异常。
- 其他类似结构体也要检查是否有类似问题。

如需帮你补充完整释放代码，请告知！
