# Mutex ("Singular Semaphore")

```C
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t mutex; // Deklaracja mutexu

int counter = 0; // Globalny licznik

void* threadFunction(void* arg) {

    // Sekcja krytyczna
    for (int i = 0; i < 1000000; i++) {
        pthread_mutex_lock(&mutex); // Blokowanie mutexu przed dostępem do sekcji krytycznej
        counter++;
	    thread_mutex_unlock(&mutex); // Odblokowanie mutexu po zakończeniu sekcji krytycznej
    }
    return NULL;
}

int main() {
    pthread_t thread1, thread2;

    pthread_mutex_init(&mutex, NULL); // Inicjalizacja mutexu

    // Tworzenie wątków
    pthread_create(&thread1, NULL, threadFunction, NULL);
    pthread_create(&thread2, NULL, threadFunction, NULL);

    // Oczekiwanie na zakończenie wątków
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    pthread_mutex_destroy(&mutex); // Zniszczenie mutexu

    printf("Wartość licznika: %d\n", counter);

    return 0;
}
```

The mutex doesn't allow two threads to try and change the counter at the same time.
Without it the counter wouldn't reach a value of 2000000, but a random value between 1000000-2000000.

# Semaphores
```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <semaphore.h>
#include <pthread.h>

#define NUM_THREADS 3

int global_variable = 0;
sem_t sem;

void *thread_function(void *arg) {
    int *thread_num = (int *) arg;

    // Wątek oczekuje na semaforze
    sem_wait(&sem);

    // Sekcja krytyczna
    global_variable++;
    printf("Wątek %d: zwiększam globalną zmienną na %d\n", *thread_num, global_variable);

    // Wątek zwalnia semafor
    sem_post(&sem);

    pthread_exit(NULL);
}

int main() {
    pthread_t threads[NUM_THREADS];
    int thread_args[NUM_THREADS];

    // Inicjalizacja semafora
    if (sem_init(&sem, 0, 1) == -1) {
        perror("Nie udało się zainicjować semafora");
        exit(EXIT_FAILURE);
    }

    // Tworzenie wątków
    for (int i = 0; i < NUM_THREADS; i++) {
        thread_args[i] = i;
        if (pthread_create(&threads[i], NULL, thread_function, &thread_args[i]) != 0) {
            perror("Błąd przy tworzeniu wątku");
            exit(EXIT_FAILURE);
        }
    }

    // Oczekiwanie na zakończenie wątków
    for (int i = 0; i < NUM_THREADS; i++) {
        if (pthread_join(threads[i], NULL) != 0) {
            perror("Błąd przy oczekiwaniu na zakończenie wątku");
            exit(EXIT_FAILURE);
        }
    }

    // Usuwanie semafora
    sem_destroy(&sem);

    return 0;
}
```

---
### [[LAB10 - Zadania|Zadania]]