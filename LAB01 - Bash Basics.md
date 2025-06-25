### Bash Commands

`history` - History of commands

***Zmienne lokalne:***
- `zmienna=jajco`
- `zmienna=10`

***Print:***
`echo $zmienna`

"" vs '' in echo
```
zmienna=10

echo 'Pożycz no $zmienna złotych'
Pożycz no $zmienna złotych

echo "Pożycz no $zmienna złotych"
Pożycz no 10 złotych
```

***Skrypty:***
`nano skrypt.sh`

`./skrypt.sh`

`chmod -v ug+x skrypt.sh`

`chmod 764 skrypt.sh`

4  2  1  4  2  0   4  0  0
`r w x` `r w -` `r - -`
   user    group     all  

#### Strumień
`fortune | cowsay` <- przekierowanie output'u 

`fortune > jajco.txt` <- overwrite file with output
`fortune >> jajco.txt` <- append to file

#### Save file
`wget https://mhyla.com/repo/cars.txt`

`curl https://mhyla.com/repo/cars.txt > file.txt` <- curl copies


#### TXT stuff
`sort` - sortowanie wierszy
`nl` -  numerowanie wierszy
`wc` - zliczanie wierszy, słów i znaków
`head` - wyświetlenie pierwszych _n_ linii pliku
`tail` - wyświetlenie ostatnich _n_ linii pliku (tail -3 cars.txt)
`grep` - wyszukiwanie patternów (regex)

---

### [[LAB02 - Bash Continued|Następny LAB]]
### [[LAB01 - Zadania|Zadania]]