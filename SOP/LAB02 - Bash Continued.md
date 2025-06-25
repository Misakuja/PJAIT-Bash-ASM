[[LAB02 - Zadania]]

#### Exit Codes
`$?` - zmienna nadpisywana przez ostatni exit code
`echo $?` outputs:
0 - True
1 - False

---
### If statement

```bash
if [ statement ]
then
    do_something
else
    do_something_else
fi
```

\[ statement ] - in reality it's just a `test` command.

`test` can do a lot; compare, check if smth exits, then returns 0 or 1

---
### While loop
Also similar to `if`, executes until the test doesn't pass.

---
### grep - Regex
grep - search for provided argument
ex.
```bash
grep 'Lidl' zakupy.txt
```

REGEX:
- . – pojedynczy dowolny znak
- ? – poprzedni znak pojawia się 0 lub 1 raz
- * - poprzedni znak pojawia się 0 lub więcej razy
- + - poprzedni znak pojawia się 1 lub więcej razy
- {n} – poprzedni znak pojawia się n lub więcej razy
- {n,m} – poprzedni znak pojawia się od n do m razy
- \[abc] – znak jest jednym z wymienionych w nawiasie
- \[^abc] – znak nie jest jednym z wymienionych w nawiasie
- () – pozwala na grupowanie znaków
- | - operacja logiczna OR
- ^ - początek linii
- $ - koniec linii (za koniec przyjmujemy tutaj znacznik LF, windowsowe CRLF może nie zostać wykryte)

- \d - any number
- \w - all numbers and letters

Searching for two things with REGEX:
```bash
grep -P "ford.*2005\t" cars.txt
```

---
### sed - very powerful tool for a lot of things
!Doesn't automatically save, it just outputs it. Use `>>` to save it to a file.

Search and replace:

search for something and replace it with another thing. 
ex.
```bash
sed 's/UKOS/SOP/' przedmioty.txt
```
This command replaces all "UKOS" to "SOP".

---

### [[LAB03 - Time, Backups|Następny LAB]]
### [[LAB02 - Zadania|Zadania]]