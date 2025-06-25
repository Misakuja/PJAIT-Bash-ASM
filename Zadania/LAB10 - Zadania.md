1. Napisz program w języku C, który tworzy 5 wątków. Każdy wątek ma zwiększyć wspólną zmienną globalną o 1, 1000 razy. Wykorzystaj mutex, aby zapewnić, że dostęp do zmiennej jest zsynchronizowany i żaden wątek nie zmodyfikuje jej w tym samym czasie co inny wątek.
```C
#include <stdio.h>  
#include <pthread.h>  
  
pthread_mutex_t mutex;  
int counter = 0;  
  
void* threadFunction(void* arg) {  
  
    for (int i = 0; i < 1000; i++) {  
        pthread_mutex_lock(&mutex);  
        counter++;  
        pthread_mutex_unlock(&mutex);  
    }  
    return NULL;  
}  
  
int main() {  
    pthread_t thread1, thread2, thread3, thread4, thread5;  
  
    pthread_mutex_init(&mutex, NULL);  
  
    pthread_create(&thread1, NULL, threadFunction, NULL);  
    pthread_create(&thread2, NULL, threadFunction, NULL);  
    pthread_create(&thread3, NULL, threadFunction, NULL);  
    pthread_create(&thread4, NULL, threadFunction, NULL);  
    pthread_create(&thread5, NULL, threadFunction, NULL);  
  
    pthread_join(thread1, NULL);  
    pthread_join(thread2, NULL);  
    pthread_join(thread3, NULL);  
    pthread_join(thread4, NULL);  
    pthread_join(thread5, NULL);  
  
    pthread_mutex_destroy(&mutex);  
  
    printf("Wartość licznika: %d\n", counter);  
  
    return 0;  
}
```

2. Napisz program, który symuluje [problem producenta-konsumenta](https://pl.wikipedia.org/wiki/Problem_producenta_i_konsumenta) z jednym producentem i jednym konsumentem, korzystając z semaforów. Producent ma wypełniać bufor danymi (np. liczby całkowite), a konsument ma te dane pobierać i wyświetlać. Użyj semaforów do synchronizacji dostępu do bufora.
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
#include <semaphore.h>  
#include <pthread.h>  
#include <fcntl.h>  
  
#define BUFFER_SIZE 5  
  
int buffer[BUFFER_SIZE];  
int producerTracker = 0;  
int consumerTracker = 0;  
  
sem_t *empty;  
sem_t *full;  
pthread_mutex_t mutex;  
  
void *producer(void *arg) {  
    for (int i = 0; i < BUFFER_SIZE; ++i) {  
        sem_wait(empty);  
        pthread_mutex_lock(&mutex);  
  
        buffer[producerTracker] = i;  
        producerTracker = (producerTracker + 1) % BUFFER_SIZE; //if producerTracker > buffer size, return to index 0  
  
        pthread_mutex_unlock(&mutex);  
        sem_post(full);  
    }  
  
    pthread_exit(NULL);  
}  
  
void *consumer(void *arg) {  
    for (int i = 0; i < BUFFER_SIZE; ++i) {  
        sem_wait(full);  
        pthread_mutex_lock(&mutex);  
  
        const int value = buffer[consumerTracker];  
        consumerTracker = (consumerTracker + 1) % BUFFER_SIZE;  
  
        printf("Consumer: %d\n", value);  
  
        pthread_mutex_unlock(&mutex);  
        sem_post(empty);  
    }  
  
    pthread_exit(NULL);  
}  
  
  
int main() {  
    pthread_t producerThread, consumerThread;  
  
    empty = sem_open("/empty_sem", O_CREAT, 0644, BUFFER_SIZE);  
    if (empty == SEM_FAILED) {  
        perror("sem_open empty");  
        exit(EXIT_FAILURE); //MacOS alternative (Sem_init is not implemented)  
    }  
  
    full = sem_open("/full_sem", O_CREAT, 0644, 0);  
    if (full == SEM_FAILED) {  
        perror("sem_open full");  
        sem_unlink("/empty_sem");  
        exit(EXIT_FAILURE);  
    }  
  
    pthread_mutex_init(&mutex, NULL);  
  
    pthread_create(&producerThread, NULL, producer, NULL);  
    pthread_create(&consumerThread, NULL, consumer, NULL);  
  
    pthread_join(producerThread, NULL);  
    pthread_join(consumerThread, NULL);  
  
    sem_close(empty);  
    sem_close(full);  
    sem_unlink("/empty_sem");  
    sem_unlink("/full_sem");  
  
    return 0;  
}
```

3. Napisz program w języku C, który symuluje [problem czytelników i pisarzy](https://pl.wikipedia.org/wiki/Problem_czytelnik%C3%B3w_i_pisarzy_). Użyj semaforów do synchronizacji, aby wielu czytelników mogło jednocześnie czytać, ale tylko jeden pisarz mógł pisać w danym momencie. Gdy pisarz pisze, żaden czytelnik nie może czytać.
	   Third readers–writers problem (_no thread shall be allowed to starve_)
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
#include <semaphore.h>  
#include <pthread.h>  
#include <fcntl.h>  
  
#define READERS 5  
#define WRITERS 2  
  
int readCount = 0;  
  
sem_t *resource;  
sem_t *queue;  
pthread_mutex_t mutex;  
  
void *reader(void *arg) {  
    sem_wait(queue);  
    pthread_mutex_lock(&mutex);  
  
    readCount++;  
    if (readCount == 1)  
        sem_wait(resource);  
  
    sem_post(queue);  
    pthread_mutex_unlock(&mutex);  
  
    printf("Reader thread %p is reading\n", pthread_self());  
  
    pthread_mutex_lock(&mutex);  
    readCount--;  
    if (readCount == 0)  
        sem_post(resource);  
    pthread_mutex_unlock(&mutex);  
  
    pthread_exit(NULL);  
}  
  
void *writer(void *arg) {  
    sem_wait(queue);  
    sem_wait(resource);  
    sem_post(queue);  
  
    printf("Writer thread %p is writing\n", pthread_self());  
  
    sem_post(resource);  
  
    pthread_exit(NULL);  
}  
  
  
int main() {  
    pthread_t readers[READERS], writers[WRITERS];  
  
    resource = sem_open("/resource_sem", O_CREAT | O_EXCL, 0644, 1);  
    if (resource == SEM_FAILED) {  
        perror("sem_open resource");  
        exit(EXIT_FAILURE);  
    }  
  
    queue = sem_open("/queue_sem", O_CREAT | O_EXCL, 0644, 1);  
    if (queue == SEM_FAILED) {  
        perror("sem_open queue");  
        sem_unlink("/empty_sem");  
        exit(EXIT_FAILURE);  
    }  
  
    pthread_mutex_init(&mutex, NULL);  
  
    for (int i = 0; i < READERS; ++i) {  
        pthread_create(&readers[i], NULL, reader, NULL);  
    }  
  
    for (int i = 0; i < WRITERS; ++i) {  
        pthread_create(&writers[i], NULL, writer, NULL);  
    }  
  
    for (int i = 0; i < READERS; ++i) {  
        pthread_join(readers[i], NULL);  
    }  
  
    for (int i = 0; i < WRITERS; ++i) {  
        pthread_join(writers[i], NULL);  
    }  
  
    sem_close(resource);  
    sem_close(queue);  
    sem_unlink("/resource_sem");  
    sem_unlink("/queue_sem");  
  
    pthread_mutex_destroy(&mutex);  
  
    return 0;  
}
```

