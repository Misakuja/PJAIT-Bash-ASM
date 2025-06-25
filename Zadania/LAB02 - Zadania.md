[[LAB02 - Bash Continued]]


1. Napisz wyrażenia regularne, które sprawdzą:
    a. Czy podany tekst jest adresem e-mail
	`^[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$`

    b. Czy podany tekst jest prawidłowym imieniem (zaczyna się od wielkiej litery i zawiera tylko litery)
	`^[A-Z]{1}[a-z]+ [A-Z]{1}[a-z]+$`

    c. Czy podany teks jest prawidłowym polskim kodem pocztowym (69-420)
	`^\d{2}-\d{3}$`

2. Napisz skrypt, który sprawdzi, czy podano do niego dokładnie jeden argument (przypomnę o $#, $@) oraz czy jest to plik, a następnie wyszuka w tym pliku wszystkie wystąpienia ciągu “s\[5cyfr]”
```bash
#!/bin/bash

if [ $# -eq 1 ]; then
    if [ -f "$1" ]; then
        grep -E 's\d{5}\b' "$1"
    fi
fi
```

3. **NA DODATKOWY PUNKT** Napisz skrypt, który wspomoże biednego wykładowcę w sprawdzaniu kolokwiów. W katalogu znajdują się 32 pliki cpp, nazywające się zgodnie ze wzorem - \[nrindeksu]\_z1.cpp. Zadanie, które będzie sprawdzane poległo na wczytaniu od użytkownika liczby <25 i wydrukowaniu jej silni. Biedny wykładowca chciałby aby:
    - Wyświetlony na górze ekranu został nr indeksu studenta bez rozszerzenia o raz numeru zadania
    - Poniżej wyniki wywołania sprawdzanego zadania na wartościach 5, 10 i 30
    - Skrypt umożliwił mu wpisanie liczby punktów za zadanie i umieszczenie jej wraz z **SAMYM** nr indeksu w pliku tekstowym lub wyświetlenie kodu zadania w razie wątpliwości
    - Po wpisaniu oceny powinien przejść automatycznie do następnego studenta.
```bash
#!/bin/bash

results_file="results.txt"
echo "RESULTS" > "$results_file"

cpp_files=$(ls | egrep "s[[:digit:]]{5}\_z1.cpp")

for file in $cpp_files; do
        index="${file%%_*}" # %%_* expression that removes everything after _
        echo "$index" >> "$results_file"
		
        clang++ "$file" -o program.out
                if [ $? -ne 0 ]; then # if exit status isnt 0
                echo "Program doesn't compile" >> "$results_file"
                echo "Program doesn't compile"
        fi
		
        for num in 5 10 30; do
                echo "For input: $num" >> "$results_file"
                echo "For input: $num"  
                ./program.out <<< "$num" >> "$results_file"
                ./program.out <<< "$num"
        done
		
        # -p for prompt
        read -p "Enter grade for $index or 'c' to see code " grade
        if [ "$grade" == "c" ]; then
                cat "$file"   
                read -p "Enter grade for $index: " grade 
        fi
                
        echo "GRADE: $grade" >> "$results_file"
        echo "-----" >> "$results_file"
done            

rm -f program.out #remove france (the .out)
```


![[s31232_z1 1.cpp]]