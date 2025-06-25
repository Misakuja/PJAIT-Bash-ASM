1. Napisz programik, który w nieskończonej pętli będzie równolegle drukował w wątku „jestem wątkiem” a w mainie „jestem mainem”. Zaobserwuj co się dzieje. Potem dopisz do tych pętli usleep() (uśpienie w μs) o różnych wartościach, np. 500 i 2500. Co się zmieniło?
```C
#include <stdio.h>  
#include <pthread.h>  
#include <unistd.h>  
  
void *thread_function(void *thread_arg) {  
    usleep(500);  
    while(1) {  
        printf("I'm the thread!\n");  
    }  
}  
  
int main() {  
    pthread_t thread;  
     pthread_create(&thread, NULL, thread_function, NULL);  
  
    usleep(2500);  
    while(1) {  
        printf("I'm the main!\n");  
    }  
}
```

2. Napisz programik, który współbieżnie wykona 10 razy printa w jednym wątku i 10 razy printa w mainie, a gdy wszystko się zakończy wydrukuje informację że się zakończyło.
```C
#include <stdio.h>  
#include <pthread.h>  
  
void *thread_function(void *thread_arg) {  
    for (int i = 0; i <= 10; ++i) {  
        printf("I'm in thread!\n");  
    }  
    pthread_exit(NULL);  
}  
  
int main() {  
    pthread_t thread;  
    pthread_create(&thread, NULL, thread_function, NULL);  
  
    for (int i = 0; i <= 10; ++i) {  
        printf("I'm in main!\n");  
    }  
  
    pthread_join(thread, NULL);  
  
    printf("Done with everything.");  
    return 0;  
}
```

4. \[OSTROŻNIE] Napisz program, który będzie działał jako prosty licznik czasu. Główny wątek będzie odpowiedzialny za obsługę interakcji z użytkownikiem i oczekiwanie na polecenie rozpoczęcia zliczania czasu. Po otrzymaniu takiego polecenia, program utworzy wątek, który będzie zliczał czas w tle. Główny wątek będzie nadal aktywny, pozwalając użytkownikowi na wydawanie poleceń, takich jak zatrzymanie lub zresetowanie licznika czasu.
```C
#include <stdio.h>  
#include <pthread.h>  
#include <unistd.h>  
#include <stdbool.h>  
#include <time.h>  
#include <string.h>  
  
bool counting = false;  
bool death = false;  
time_t start_time = 0;  
int elapsed_time = 0;  
  
void *thread_function(void *thread_arg) {  
    while(1) {  
        if (counting) {  
            time_t end = time(NULL);  
            elapsed_time = (int)(end - start_time);  
        }  
        if (death) break;  
    }  
    pthread_exit(NULL);  
}  
  
int main() {  
    pthread_t thread;  
    pthread_create(&thread, NULL, thread_function, NULL);  
  
    while (1) {  
        char input[20];  
        printf("Type in 'start', 'stop', 'reset' or 'exit'\n");  
        scanf("%19s", input);  
  
        if (strcmp(input, "start") == 0) {  
            if (!counting) {  
                start_time = time(NULL) - elapsed_time;  
                counting = true;  
            }  
        } else if (strcmp(input, "stop") == 0) {  
            if (counting) {  
                elapsed_time = (int)(time(NULL) - start_time);  
                counting = false;  
                printf("Stopped. Time elapsed: %d seconds\n", elapsed_time);  
            }  
        } else if (strcmp(input, "reset") == 0) {  
            elapsed_time = 0;  
            if (counting) {  
                start_time = time(NULL);  
                printf("Reset. Time elapsed: %d seconds\n", elapsed_time);  
  
            }  
        } else if (strcmp(input, "exit") == 0) {  
            death = true;  
            pthread_join(thread, NULL);  
            break;  
        }  
    }  
    return 0;  
}
```