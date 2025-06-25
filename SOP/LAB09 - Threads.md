# Create Thread
```C
pthread_create(&threads[i], NULL, thread_function, (void *)&thread_args[i]);
```
(thread ID, NULL, function the thread will do, void * type of argument(s))

# Exit from Thread
```C
pthread_exit(NULL);
```
kills thread, argument is what it'll return

# Make parent wait for the threads to end before proceeding
```C
pthread_join(threads[i], NULL);
```


# Quick Thread Example
```C
#include <stdio.h>
#include <pthread.h>

#define NUM_THREADS 5

// Funkcja, która będzie wykonywana przez wątek
void *thread_function(void *thread_arg) {
    int thread_id = *(int *)thread_arg;
    printf("Wątek %d: Witaj, świecie!\n", thread_id);
    pthread_exit(NULL);
}

int main() {
    pthread_t threads[NUM_THREADS];
    int thread_args[NUM_THREADS];
    int i;

    // Tworzenie wątków
    for (i = 0; i < NUM_THREADS; i++) {
        thread_args[i] = i;
        pthread_create(&threads[i], NULL, thread_function, (void *)&thread_args[i]);
    }

    // Oczekiwanie na zakończenie wątków
    for (i = 0; i < NUM_THREADS; i++) {
        pthread_join(threads[i], NULL);
    }

    printf("Wszystkie wątki zakończyły pracę. Program kończy działanie.\n");

    return 0;
}
```

---

### [[LAB10 - Mutex, Semaphores|Następny LAB]]
### [[LAB09 - Zadania|Zadania]]