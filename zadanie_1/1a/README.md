Zadanie 1a polega na zaimportowaniu, do systemów baz danych uruchomionych na swoim komputerze, danych z pliku `Train.csv` bazy:

   * `MongoDB`
   * `PostgreSQL` – opcjonalnie dla znających fanów SQL

### Poprawienie pliku

Aby poprawnie zaimportowac plik nalezy najpierw usunąć z niego znaki nowej lini `\n` co mozna zrobić w nastepujący sposób:

`cat Train.csv | tr "\n" " " | tr "\r" "\n" > Train_prepared.csv`

Po tej operacji plik posiada 6034197 lini, a więc o jedną za dużo. Na końcu pliku znajduje się pusta linia która nalezy usunąć. Teraz mozna zrobić import.

### Import

Czas importu mierzymy poleceniem time
`time mongoimport -d train -c train --type csv --headerline --file Train.csv` 

```
connected to: 127.0.0.1
Wed Oct 30 12:48:02.078 		Progress: 57919742/7253917399	0%
Wed Oct 30 12:48:02.078 			48200	12050/second
...
...
...
Wed Oct 30 12:54:27.145 		Progress: 7188208479/7253917399	99%
Wed Oct 30 12:54:27.145 			5979500	15371/second
Wed Oct 30 12:54:29.540 check 9 6034196
Wed Oct 30 12:54:29.571 imported 6034195 objects
```
