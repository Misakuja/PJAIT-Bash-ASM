```Bash
cd /proc
```
folders with all processes

1. `ps` **- wyświetla listę procesów działających na systemie, wraz z ich identyfikatorami PID, stanem, priorytetem, czasem działania i innymi informacjami.**

2. `top` - wyświetla dynamiczną listę procesów działających na systemie wraz z ich aktualnymi wartościami zasobów, takimi jak zużycie procesora, pamięci RAM, dysku i innych.
	`htop` - 'better' version.
3. `kill` - pozwala na wysłanie sygnału do procesu, co może spowodować zakończenie jego działania. Można użyć identyfikatora PID lub nazwy procesu.
4. `killall` - pozwala na wysłanie sygnału do wszystkich procesów o określonej nazwie. Może to spowodować zakończenie działania wszystkich procesów o tej samej nazwie.
5. `nice` - pozwala na ustawienie priorytetu procesu. Domyślnie wszystkie procesy mają taki sam priorytet, ale można zmienić priorytet dla wybranych procesów.
6. `renice` - pozwala na zmianę priorytetu już uruchomionego procesu.
7. `bg` - pozwala na uruchomienie zatrzymanego procesu w tle.
8. `fg` - pozwala na przywrócenie procesu do działania w trybie interaktywnym.
9. `jobs` - wyświetla listę procesów działających w tle i zatrzymanych procesów, wraz z ich identyfikatorami i stanami.
10. `pstree` - wyświetla drzewo procesów, pokazując relacje między procesami i ich rodzicami.


---

Aby uruchomić proces “w tle” należy przy wywołującym poleceniu dopisać znak `&`

```Bash
openttd &
```

```Bash
jobs
```
Aktualnie wykonywane prace przez terminal.

```Bash
fg 1
```
Make the sleeping job come to foreground (1 is job id). Makes terminal unusable.

```Bash
bg 1
```
Make it go back to background. Allows use of terminal.

---
### Signals
- SIGINT (sygnał przerwania) - wysyłany do procesu, gdy użytkownik wciśnie klawisze Ctrl+C. Powoduje to przerwanie bieżącej operacji procesu.
- SIGKILL (sygnał zabójstwa) - wysyłany do procesu, aby natychmiastowo zakończyć jego działanie. Ten sygnał nie daje procesowi czasu na czyszczenie swojego stanu, a zamyka go natychmiastowo.
- SIGTERM (sygnał zakończenia) - wysyłany do procesu, aby poprosić go o zakończenie swojego działania. Ten sygnał pozwala procesowi na przeprowadzenie niezbędnych czynności przed zakończeniem.
- SIGHUP (sygnał hangup) - wysyłany do procesu, gdy użytkownik zamknie terminal. Powoduje to zakończenie procesu.
- SIGUSR1 i SIGUSR2 (sygnały użytkownika) - sygnały, które mogą być zdefiniowane i wysłane przez użytkownika w celu wywołania określonej funkcji w procesie.
- SIGPIPE (sygnał potoku) - wysyłany do procesu, gdy proces próbuje pisać do potoku, który został już zamknięty. Powoduje to zakończenie procesu.

```Bash
kill -1 6890
```


---

### [[LAB05 - NBP API + gnuplot|Następny LAB]]
### [[LAB04 - Zadania|Zadania]]