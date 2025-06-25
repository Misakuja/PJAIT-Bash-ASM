1. Obsługę sygnałów możemy bez problemu przerejestrować. Napisz program, który przy pierwszym wywołaniu SIGINT wejdzie do funkcji, jak w przykładzie z zajęć, ale przy następnym wywołaniu już wywoła SIG_DFL.
```C
#include <stdio.h>  
#include <signal.h>  
#include <unistd.h>  
  
void sig_handler(int signum){  
    printf("I'm inside the handler!");  
    signal(SIGINT, SIG_DFL);  
}  
  
int main(){  
    signal(SIGINT, sig_handler);  
    sleep(10);  
}
return 0;
```

2. Napisz programik, który po otrzymaniu od użytkownika jakiegoś inputu o długości 5 (np. stringa ‘jajco’) wyśle sygnał USR1 do handlera, który zapisze go do pliku. W przeciwnym wypadku główna funkcja zapisze input do innego pliku.
```C
#include <stdio.h>  
#include <signal.h>  
#include <unistd.h>  
#include <string.h>  
  
char string[100];  
  
void sig_handler(int signum){  
    FILE *file = fopen("LAB08z2-1.txt", "a");  
    fprintf(file, "%s\n", string);  
    fclose(file);  
}  
  
int main(){  
    scanf("%s", string);  
    if (strlen(string) == 5) {  
        signal(SIGUSR1, sig_handler);  
        raise(SIGUSR1);  
    } else {  
        FILE *file = fopen("LAB08z2-2.txt", "a");  
        fprintf(file, "%s\n", string);  
        fclose(file);  
    }  
}
return 0;
```

3. Napisz program, który zlicza, ile razy otrzymał sygnał SIGINT, a następnie wypisuje tę wartość na ekranie po otrzymaniu sygnału SIGTERM.
```C
#include <stdio.h>  
#include <signal.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
int counter;  
  
void sig_handler(int signum) {  
  if (signum == SIGINT) {  
    printf("Got a SIGINT!\n");  
    counter++;  
  }  
  else if (signum == SIGTERM) {  
    printf("SIGINT Counter: %d\n", counter);  
    exit(0);  
  }  
}  
  
int main() {  
  counter = 0;  
  signal(SIGINT, sig_handler);  
  signal(SIGTERM, sig_handler);  
  
  sleep(10);  
  raise(SIGTERM);  
  return 0;  
}
```

4. Napisz programik, który będzie się wykonywał w pętli, a w momencie otrzymania SIGUSR1 wydrukuje na terminalu potwierdzenie i zabije się
```C
#include <stdio.h>  
#include <signal.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
void sig_handler(int signum) {  
  printf("Received SIGUSR1 signal.\n");  
  exit(0);  
}  
  
int main() {  
  signal(SIGUSR1, sig_handler);  
  printf("Program started. PID: %d\n", getpid());  
  
  while(1) {  
    sleep(1);  
  }  
  return 0;  
}
```

5. Tu użyjemy wiadomości z kilku wcześniejszych zajęć. Napisz program, który się sforkuje, a następnie rodzic zabije dziecko. Przyda się polecenie kill() – w nim możemy podać konkretny PID, który chcemy ubić.
```C
#include <stdio.h>  
#include <signal.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
int main() {  
  pid_t child_pid = fork();  
  
  if (child_pid < 0) {  
    exit(1);  
  }  
  
  else if (child_pid == 0) {  
    while (1) {  
      sleep(1);  
    }  
    return 0;  
  } else {  
    printf("Parent process. PID: %d\n", getpid());  
    printf("Child process has PID: %d\n", child_pid);  
  
    if (kill(child_pid, SIGTERM) < 0) {  
      exit(1);  
    }  
  
     int status;  
     waitpid(child_pid, &status, 0);  
  
    if (WIFSIGNALED(status)) {  
      printf("Child killed by signal number: %d\n", WTERMSIG(status));  
    }  
  }  
  return 0;  
}
```