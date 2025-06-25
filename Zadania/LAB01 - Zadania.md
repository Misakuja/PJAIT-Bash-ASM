[[LAB01 - Zadania]]

1. W katalogu Dokumenty (Documents) utwórz poniższą hierarchię katalogów:

    ```
     Example
     ├── JAZ
     ├── Przepisy
     │   ├── Kolacje
     │   ├── Obiady
     │   └── Sniadania
     ├── SOP
     └── PRI
    ```

    Proszę o przesłanie rozwiązania jako wycinka historii terminala (polecenie `history`)
```bash
mkdir -p ~/Documents/Example/{JAZ,Przepisy/{Kolacje,Obiady,Sniadania},SOP,PRI}
```

2. Napisz skrypt, który pobierze plik [mhyla.com/repo/cars.txt](https://mhyla.com/repo/cars.txt), posortuje go malejąco, ponumeruje linie i zapisze pierwsze 20 do pliku lista.txt
```bash
#!/bin/bash

curl https://mhyla.com/repo/cars.txt > cars.txt

sort -r cars.txt | nl | head -n 20 > lista.txt
```

3. Napisz skrypt, który przyjmie od użytkownika liczbę 1-20 i wyświetli samochód z pliku lista.txt o indeksie równym tej liczbie
```bash
#!/bin/bash

echo "Input a number between 1 and 20"

read num

head -n "$num" lista.txt | tail -n 1
```
