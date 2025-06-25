Dzisiejsze zadanie polega na stworzeniu skryptu (albo kilku poleceń niezależnych od siebie) generującego wykres kursu USD do złotego w całym 2024 roku. Dodatkowo, jeśli skrypt będzie uruchomiony z 3 argumentami, to:

- pierwszy powinien określać walutę (USD, EUR, CHF), której wykres do złotówki będziemy wyświetlać;
- drugi powinien określać datę początkową
- trzeci powinien określać datę końcową

```Bash
#!/bin/bash

if [ $# -eq 0 ]; then
        currency="USD"
        start_date="2024-01-01"
        end_date="2024-12-30"
elif [ $# -eq 3 ]; then
        currency=$1
        start_date=$2
        end_date=$3
else
        echo "Wrong amount of arguments" 
        exit 1
fi

curl --silent "https://api.nbp.pl/api/exchangerates/rates/A/$currency/$start_date/$end_date/" | jq -r '.rates[] | "\(.effectiveDate) \(.mid)"' > "$currency.data"

echo "set title '$currency to PLN'
set xlabel 'Date'  
set ylabel 'Exchange Rate'
set grid
set xdata time
set timefmt '%Y-%m-%d'
set format x '%b %y'
plot '$currency.data' using 1:2 with lines title '$currency'" > plot.gp

gnuplot -persist plot.gp
```












