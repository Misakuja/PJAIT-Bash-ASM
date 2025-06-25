1. Napisz program, który:
- Tworzy **trzy procesy potomne** (`fork()`).
- Każdy proces potomny powinien:
    - wypisać swój `PID` i `PPID`,
    - zakończyć działanie kodem `exit(i)` (gdzie `i` to numer procesu).
- Proces rodzica powinien:
    - poczekać na **każdego potomka osobno** (`waitpid()`),
    - wypisać informacje o zakończeniu dzieci: `PID` + kod wyjścia (`WEXITSTATUS()`).
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
int main(int argc, char *argv[]) {  
    pid_t pidArr[3];  
  
    for (int i = 0; i < 3; i++) {  
       pidArr[i] = fork();  
  
       if (pidArr[i] == 0) {  
          printf("Child %d has PID: %d, and its parent's PID is: %d\n", i, getpid(), getppid());  
          exit(i);  
       }  
       sleep(1);  
    }  
  
    for (int i = 0; i < 3; i++) {  
       int status;  
  
       if (waitpid(pidArr[i], &status, 0) > 0) {  
          if (WIFEXITED(status)) {  
             printf("Parent: Child with PID %d exited with code %d\n", pidArr[i], WEXITSTATUS(status));  
          } else {  
             printf("Parent: Child didn't exit normally\n");  
          }  
       }  
    }  
    return 0;  
}
```

2. Napisz program, który:
- Tworzy jeden proces potomny.
- Dziecko:
    - wypisuje swój `PID` i `PPID`,
    - śpi 2 sekundy,
    - sprawdza ponownie `PPID` (czy zmienił się na 1),
    - kończy działanie.
- Rodzic kończy się **od razu**, bez `wait()`. Czy rodzic się zmienia? Jeśli tak, to kto jest nowym rodzicem?
```C
#include <stdio.h>  
#include <unistd.h>  
  
int main(int argc, char *argv[]) {  
    if (fork() == 0) {  
        printf("Child has PID: %d, and its parent PID is: %d\n", getpid(), getppid());  
        sleep(2);  
        printf("Child's parent PID is: %d\n", getppid());  
    } else {  
        sleep(1);  
    }  
    return 0;  
}
```
	Rodzic się zmienia; nowym rodzicem jest proces '1', jest to rodzic wszystkich innych procesów, pierwszy proces jaki wstaje wraz z linuxem.

3. Napisz program, który:
- Tworzy dwa procesy potomne.
- Proces 1:
    - wypisuje “Jestem procesem 1”,
    - kończy się po 1 sekundzie.
- Proces 2:
    - czeka 2 sekundy,
    - wypisuje “Jestem procesem 2”,
    - kończy się.

- Rodzic powinien użyć `waitpid()` i wypisać, **który proces zakończył się jako pierwszy i drugi**.
- Możesz dodać timestamp (`time()` lub `gettimeofday()`), by precyzyjnie zmierzyć czas zakończenia.
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
#include <time.h>  
  
int main(int argc, char *argv[]) {  
  time_t start, end;  
  time(&start);  
  
  const pid_t pid1 = fork();  
  if (pid1 == 0) {  
    sleep(1);  
    printf("I'm process 1, I finished after %.f seconds\n", difftime(time(&end), start));  
    exit(0);  
  }  
  
  const pid_t pid2 = fork();  
  if (pid2 == 0) {  
    sleep(2);  
    printf("I'm process 2, I finished after %.f seconds\n", difftime(time(&end), start));  
    exit(0);  
  }  
  
  int status1;  
  if (waitpid(pid1, &status1, 0)) {  
    printf("Process 1 finished first\n");  
  }  
  
  int status2;  
  if (waitpid(pid2, &status2, 0)) {  
    printf("Process 2 finished second\n");  
  }  
  
  return 0;  
}
```

4. Napisz program, który:
- Tworzy dziecko.
- Dziecko tworzy **swoje dziecko** (czyli wnuka).
- Proces wnuka:
    - wypisuje “Wnuk działa”, śpi 2 sekundy, kończy się `exit(5)`.
- Proces dziecko:
    - czeka na zakończenie wnuka (`wait()`),
    - wypisuje `PID` zakończonego procesu i `WEXITSTATUS`.
- Proces rodzic:
    - nie czeka na nikogo i kończy się od razu.
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
int main(int argc, char *argv[]) {  
    const pid_t child_pid = fork();  
  
    if (child_pid == 0) {  
        const pid_t grandchild_pid = fork();  
        if (grandchild_pid == 0) {  
            printf("Grandchild works\n");  
            sleep(2);  
            exit(5);  
        }  
        int status;  
        wait(&status);  
        printf("Process with PID %d finished with code %d\n", grandchild_pid, WEXITSTATUS(status));  
    }  
    return 0;  
}
```

5. Stwórz program, który buduje strukturę:

```
Rodzic
├── Dziecko 1
│   └── Wnuk 1
└── Dziecko 2
    └── Wnuk 2
```

Każdy proces powinien wypisać swoje:
- `PID`, `PPID`, poziom w drzewie
- nazwę roli (np. “Dziecko 1”)
```C
#include <stdio.h>  
#include <stdlib.h>  
#include <unistd.h>  
  
int main(int argc, char *argv[]) {  
    const pid_t child_pid1 = fork();  
    if (child_pid1 == 0) {  
       printf("Child 1, PID: %d, PPID: %d\n", getpid(), getppid());  
       const pid_t grandchild_pid1 = fork();  
       if (grandchild_pid1 == 0) {  
          printf("Grandchild 1, PID %d, PPID %d\n", getpid(), getppid());  
          exit(0);  
       }  
       int status;  
       wait(&status);  
       exit(0);  
    }  
  
    const pid_t child_pid2 = fork();  
    if (child_pid2 == 0) {  
       printf("Child 2, PID: %d, PPID: %d\n", getpid(), getppid());  
       const pid_t grandchild_pid2 = fork();  
       if (grandchild_pid2 == 0) {  
          printf("Grandchild 2, PID %d, PPID %d\n", getpid(), getppid());  
          exit(0);  
       }  
       int status;  
       wait(&status);  
       exit(0);  
    }  
    int status1, status2;  
    wait(&status1);  
    wait(&status2);  
    return 0;  
}
```