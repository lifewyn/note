---
time: 2025-02-12
tags:
  - C
---

Yes, there are several alternative synchronization solutions that might be more appropriate for this use case. Here are some options:

1. **Read-Write Lock (rwlock)**
```c:buildroot/buildroot-201902/scom/check_power/check_power.c
// Declare at global scope
pthread_rwlock_t power_rwlock = PTHREAD_RWLOCK_INITIALIZER;

// In battery_routine_check (writer)
void *battery_routine_check(void *arg)
{
    while(1) {
        power_type power_data_local;
        
        // Get data without lock first
        if (get_adc_voltage(&power_data_local.adc_voltage) != 0) {
            continue;
        }
        if (get_power_voltage(power_data_local.adc_voltage/1000, &power_data_local.power_voltage) != 0) {
            continue;
        }
        if (get_power_surplus_capacity(power_data_local.power_voltage, &power_data_local.battery) != 0) {
            continue;
        }

        // Only lock when updating shared data
        if (pthread_rwlock_wrlock(&power_rwlock) == 0) {
            memcpy(&shared_power_data, &power_data_local, sizeof(power_type));
            pthread_rwlock_unlock(&power_rwlock);
        }
        
        sleep(1);
    }
}

// In exposure_battery_check (reader)
void *exposure_battery_check(void *arg)
{
    while(1) {
        power_type power_data_local;
        
        // Quick read of shared data
        if (pthread_rwlock_rdlock(&power_rwlock) == 0) {
            memcpy(&power_data_local, &shared_power_data, sizeof(power_type));
            pthread_rwlock_unlock(&power_rwlock);
        }
        
        // Process data without holding lock
        // ...
    }
}
```

2. **Atomic Operations with Memory Barriers**
```c:buildroot/buildroot-201902/scom/check_power/check_power.c
#include <stdatomic.h>

typedef struct {
    atomic_int version;
    float adc_voltage;
    float power_voltage;
    float battery;
} atomic_power_type;

static atomic_power_type shared_power_data = {0};

void *battery_routine_check(void *arg)
{
    while(1) {
        power_type new_data;
        
        // Collect new data
        if (get_adc_voltage(&new_data.adc_voltage) != 0) {
            continue;
        }
        // ... other measurements ...

        // Atomic update
        int version = atomic_load(&shared_power_data.version);
        atomic_store(&shared_power_data.adc_voltage, new_data.adc_voltage);
        atomic_store(&shared_power_data.power_voltage, new_data.power_voltage);
        atomic_store(&shared_power_data.battery, new_data.battery);
        atomic_store(&shared_power_data.version, version + 1);
        
        sleep(1);
    }
}
```

3. **Lock-free Ring Buffer**
```c:buildroot/buildroot-201902/scom/check_power/check_power.c
#define POWER_BUFFER_SIZE 4

typedef struct {
    power_type buffer[POWER_BUFFER_SIZE];
    atomic_int write_index;
    atomic_int read_index;
} power_ring_buffer;

static power_ring_buffer power_buffer = {0};

void *battery_routine_check(void *arg)
{
    while(1) {
        power_type new_data;
        // Collect new data...

        int write_idx = atomic_load(&power_buffer.write_index);
        int next_write = (write_idx + 1) % POWER_BUFFER_SIZE;
        
        power_buffer.buffer[write_idx] = new_data;
        atomic_store(&power_buffer.write_index, next_write);
        
        sleep(1);
    }
}
```

4. **Message Queue Based Solution**
```c:buildroot/buildroot-201902/scom/check_power/check_power.c
#define POWER_QUEUE_SIZE 10

typedef struct {
    power_type data[POWER_QUEUE_SIZE];
    sem_t empty;
    sem_t full;
    sem_t mutex;
    int head;
    int tail;
} power_queue_t;

static power_queue_t power_queue;

void init_power_queue() {
    sem_init(&power_queue.empty, 0, POWER_QUEUE_SIZE);
    sem_init(&power_queue.full, 0, 0);
    sem_init(&power_queue.mutex, 0, 1);
    power_queue.head = power_queue.tail = 0;
}

void enqueue_power_data(power_type *data) {
    sem_wait(&power_queue.empty);
    sem_wait(&power_queue.mutex);
    
    power_queue.data[power_queue.tail] = *data;
    power_queue.tail = (power_queue.tail + 1) % POWER_QUEUE_SIZE;
    
    sem_post(&power_queue.mutex);
    sem_post(&power_queue.full);
}
```

Each solution has its advantages:

1. **Read-Write Lock**:
- Better for multiple readers, single writer
- More efficient than mutex when reads are more frequent than writes
- Still provides mutual exclusion when needed

2. **Atomic Operations**:
- No locking overhead
- Very efficient for simple operations
- Good for single producer/consumer scenarios
- Best performance but more complex to implement correctly

3. **Lock-free Ring Buffer**:
- No blocking
- Good for high-throughput scenarios
- Works well when producer and consumer run at different speeds
- Complex to implement correctly

4. **Message Queue**:
- Natural producer/consumer pattern
- Built-in synchronization
- Good for decoupling components
- Easy to understand and maintain

For your specific case, I would recommend either:

1. The Read-Write Lock solution if you need to maintain the current code structure but want better performance
2. The Message Queue solution if you're willing to restructure the code for better separation of concerns

The choice depends on your specific requirements:
- If low latency is critical: Use atomic operations
- If you have multiple readers: Use rwlock
- If you want to decouple components: Use message queue
- If you need to handle bursts of data: Use ring buffer

Would you like me to elaborate on any of these solutions?
