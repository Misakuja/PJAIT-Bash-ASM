### Time

UNIX time - seconds since midnight of 1st Jan 1970.

`date` - returns current date


```bash
date +"%d.%m.%y"
```
- %d - dzień miesiąca
- %m - miesiąc
- %y - ostatnie dwie cyfry roku
- %Y - pełny rok
- %H - godzina (24h)
- %M - minuta
and many more... 

---
### Backups
`tar`
- c - tworzy archiwum
- x - rozpakowuje archiwum
- u - zaktualizuje archiwum o podane pliki/katalogi
- f - służy do określenia nazwy i ścieżki do pliku, do którego ma być zapisane lub z którego ma być odczytane archiwum.
- z - obsłuży kompresję przy użyciu gzipa **(to make the archive smaller)**
- v - _verbose_ - standardowa opcja, zwracająca informacje o tym, co akurat robi program w trakcie wykonywania

```bassh
tar -cf archive.tar file1.txt file2.sh file3.sh Documents/Downloads/
```

---
### Scheduler

`crontab -e` - shows the file where you'd put the scheduled task. For example every minute, or every day u can run a script.

Checks if any 

 \* \* \* \* \*
 minute | hour | day(month) | month | day(week)

https://crontab.guru/


 ---

---

### [[LAB04 - Processes, Signals|Następny LAB]]
### [[LAB03 - Zadania|Zadania]]