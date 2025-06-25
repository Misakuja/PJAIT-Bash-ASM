1. Napisz skrypt, który przeskanuje katalog Dokumenty i podkatalogi w poszukiwaniu plików .txt i wyświetli ich pełną listę
```bash
find "$HOME/Documents" -type f -name "*.txt"
```

2. Poprzedni skrypt rozszerz o pakowanie tego zestawu do skompresowanego pliku o nazwie “\[user]\_backup_\[data_wykonania]”. Takie backupy powinny być umieszczone w katalogu ~/.backups
```bash
#!/bin/bash
mkdir -p "$HOME/backups" # -p makes it not fail if it exists already

backup="$HOME/backups/$(whoami)_backup_$(date +"%Y-%m-%d_%H-%M-%S").tar.gz"

find "$HOME/Documents" -type f -name "*.txt" | tar -czf "$backup" "$HOME/Documents"/*.txt 
```

3. Stwórz zadanie, które cyklicznie (np. raz dziennie) wykona ten skrypt
```Bash
crontab -e
```

`i <- Insert Mode

```Bash
0 0 * * * /Documents/z2.sh
```

4. Napisz skrypt + crontaba, który cyklicznie sprawdzać będzie użycie przestrzeni dyskowej przez backupy w naszym katalogu i jeśli ta zużyta przestrzeń będzie większa niż X (do samodzielego ustalenia), to powiadomi użytkownika. Przydać się mogą polecenie `df` do sprawdzenia zajętego miejsca, `awk` do wyciągnięcia odpowiedniej wartości i pakiet `mailx`. 
```bash
#!/bin/bash 

backup_directory="$HOME/backups" total_space_kb=$(df -k "$backup_directory" | awk 'NR==2 {print $2}') # Total space in KB 

backup_usage_kb=$(du -sk "$backup_directory" | awk '{print $1}') # Backup directory size in KB 

usage_percentage=$(( 100 * backup_usage_kb / total_space_kb )) 

if [ "$usage_percentage" -gt 10 ]; then
    echo "Warning: The directory $backup_directory is using more than 10% of disk space. Current usage: $usage_percentage%" | mailx -s "Disk Space Usage Alert" "$USER"
fi
```

```Bash
crontab -e
```

`i <- Insert Mode

```Bash
0 * * * * /Documents/z4.sh
```
