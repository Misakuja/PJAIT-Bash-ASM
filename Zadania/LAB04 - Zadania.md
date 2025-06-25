1. Uruchom program (np. openttd) w tle za pomocą polecenia nohup i zweryfikuj jego działanie za pomocą polecenia `ps`. Zmień jego priorytet za pomocą polecenia renice, aby porównać wpływ na wydajność programu. Następnie użyj polecenia kill, aby zabić proces.
```Bash
nohup openttd &
```
```Bash
ps
```
```Bash
sudo renice -2 59801
```
```Bash
ps -o pid,nice
```
```Bash
kill 59801
```

2. Uruchom program w tle za pomocą polecenia nohup i przekieruj jego wynik do pliku tekstowego.
```Bash
nohup openttd > output.txt &
```

3. Użyj polecenia top lub htop, aby monitorować wykorzystanie zasobów przez różne procesy na twoim systemie. Zidentyfikuj procesy, które zużywają najwięcej pamięci lub mocy obliczeniowej, i zmień ich priorytet za pomocą polecenia renice.
```Bash
htop
```
```Bash
sudo renice -10 -p 59899
```
```Bash
sudo renice -10 -p 1371
```
```Bash
sudo renice -10 -p 59911
```

4. Zastosuj polecenie nice do uruchomienia programu z niższym priorytetem niż standardowy. Następnie wykonaj inne zadanie na komputerze i zaobserwuj, jak program działa w tle z niższym priorytetem.
```Bash
nice -n 20 openttd &
```
openttd zabiera mniej CPU w porównaniu do innych programów / openttd otwartego z nice = 0. Będzie zatem działał wolniej na koszt innych programów.

5. Uruchom dowolny proces, a następnie wstrzymaj jego wykonywanie za pomocą polecenia kill -STOP. Następnie wznów jego wykonywanie za pomocą polecenia kill -CONT.
```Bash
nohup openttd &

kill -STOP 60162

kill -CONT 60162
```

6. Utwórz skrypt, który automatycznie uruchamia program po starcie systemu. Następnie zrestartuj system i upewnij się, że program uruchamia się automatycznie - wujek google albo prowadzący podpowiedzą, w razie czego.
### Script:
```bash
#!/bin/bash

openttd &
```
#### Crontab solution 
It should work on both Mac and Linux, but for some reason doesn't on my Mac.
Apple also depreciated `Crontab` in favour of `launchd`
```bash
@reboot sh ~/PJATK/4-Semestr/SOP/"Lesson 04 - Work"/Zad06.sh
```
#### Launchd solution
1. Place the `.plist` in the `~Library/LaunchDaemons/` directory for system-wide tasks, regardless of which user logs in.
2. Ensure `chown root:wheel` and `chmod 644`.
3. Make sure to run:
```bash
sudo launchctl load com.sop.lab04.zad06.plist
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.system.openttd</string>

    <key>ProgramArguments</key>
    <array>
        <string>/Users/ateoyu/PJATK/4-Semestr/SOP/Lesson 04 - Work/zad06.sh</string>
    </array>

    <key>RunAtLoad</key>
    <true/>

    <key>KeepAlive</key>
    <true/>

    <key>UserName</key>
    <string> </string>
</dict>
</plist>
```


