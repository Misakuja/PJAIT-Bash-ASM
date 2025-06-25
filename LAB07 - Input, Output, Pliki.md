```C
FILE *fopen( const char * filename, const char * mode );
```

- r - Otwiera plik w trybie do odczytu
- w - Otwiera plik w trybie do zapisu. Jeśli plik nie istnieje, jest tworzony, jeśli istnieje i ma zawartość, zostanie ona usunięta
- a - Otwiera plik w trybie „append”. Dane będą dopisywane na końcu, . Operacje pozycjonowania (fseek, fsetpos) są ignorowane
- r+ - Otwiera plik w trybie odczytu/zapisu. Plik musi istnieć
- w+ - Otwiera plik w trybie odczytu/zapisu. Jeśli plik nie istnieje, jest tworzony, jeśli istnieje i ma zawartość, zostanie ona usunięta
- a+ - Otwiera plik w trybie odczytu/zapisu. Dane będą dopisywane na końcu. Operacje pozycjonowania (fseek, fsetpos) działają

```C
#include <stdio.h>

int main() {
    FILE *plik;
    char slowo[100];

    plik = fopen("nazwa_pliku.txt", "r"); // otwieranie pliku do odczytu

    if (plik == NULL) {
        printf("Nie udalo sie otworzyc pliku\n");
        return 1;
    }

    while (fscanf(plik, "%s", slowo) != EOF) { // odczyt slowo po slowie
        printf("%s ", slowo); // wyswietlenie
    }

    fclose(plik); // zamkniecie pliku
    return 0;
}
```

```C
#include <stdio.h>

int main() {
    FILE *plik;
    char slowo[100] = "Hello, world!";

    plik = fopen("nazwa_pliku.txt", "w"); // otwieranie pliku do zapisu

    if (plik == NULL) {
        printf("Nie udalo sie otworzyc pliku\n");
        return 1;
    }

    fprintf(plik, "%s", slowo); // zapisywanie slowa do pliku

    fclose(plik); // zamkniecie pliku
    return 0;
}
```

---
## Odczyt i zapis stringów

`fgets()` i `fputs()` to dwie z podstawowych funkcji wejścia/wyjścia w języku C, które służą do odczytu i zapisu danych z/do plików tekstowych.

Funkcja `fgets()` służy do odczytu wiersza tekstu z pliku. Jej sygnatura wygląda następująco:

```C
char *fgets(char *str, int num, FILE *stream);
```

Argument `str` wskazuje na bufor, do którego zostanie zapisany odczytany wiersz tekstu. Argument `num` określa maksymalną ilość znaków, jakie mogą być odczytane (uwzględniając miejsce na znak końca wiersza). Argument `stream` to wskaźnik na plik, z którego ma być odczytany tekst.

Funkcja `fputs()` służy do zapisu wiersza tekstu do pliku. Jej sygnatura wygląda następująco:

```C
int fputs(const char *str, FILE *stream);
```

Argument `str` wskazuje na napis, który ma być zapisany do pliku. Argument `stream` to wskaźnik na plik, do którego ma być zapisany tekst.

Obie funkcje zwracają wartość różną od zera w przypadku sukcesu, a zero w przypadku błędu.


---

### [[LAB08 - Signals|Następny LAB]]
### [[LAB08 - Zadania|Zadania]]