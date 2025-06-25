`geany` - text editor for C

`gcc` - compilator for c

---

C Main:
```C
int main(int argc, char *argv) {
}
```
`argc` jest intem, w którym znajdować się będzie liczba argumentów przekazanych do programu. `Argv[]` natomiast jest tablicą o rozmiarze takim jak _argc_, która zawierać będzie kolejno wszystkie przekazane do programu argumenty.

---

```C
#include <stdio.h>
```
printf library ^

Printf wildcards:
- %c – char
- %s – string
- %d – signed int
- %f – float
- %u – unsigned int

```C
#include <unistd.h>
```
fork library ^


```C
getpid(); - get pid of the process
getppid(); - get pid of the parent process
```


---


```C
#include <sys/wait.h>
```
wait library ^

---

### [[LAB07 - Input, Output, Pliki|Następny LAB]]
### [[LAB06 - Zadania|Zadania]]