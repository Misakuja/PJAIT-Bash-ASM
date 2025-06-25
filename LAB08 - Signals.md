Sygnał to „wydarzenie”, generowane, aby poinformować proces, że coś ważnego się wydarzyło.

# Set the Signal to do something you want
```C
signal(int sig, void (*handler)(int));
```

# Send Signal to self
```C
int raise(int sig);
```

# Quick Example:
```C
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void sig_handler(int signum);

int main(){
        signal(SIGINT, sig_handler);
        while(1){
                printf("Jestem w mainie!\n");
                sleep(1);
        }
}

void sig_handler(int signum){
        printf("jestem w handlerze!\n");
        sleep(1);
}
```

---

### [[LAB09 - Threads|Następny LAB]]
### [[LAB08 - Zadania|Zadania]]