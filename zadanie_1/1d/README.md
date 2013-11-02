### Treść
`Zadanie 1d`. Ściągnąć plik `text8.zip` ze strony `Matt Mahoney` (po rozpakowaniu 100MB):

`wget http://mattmahoney.net/dc/text8.zip -O text8.gz`

Zapisać wszystkie słowa w bazie `MongoDB`. Następnie zliczyć liczbę słów oraz liczbę różnych słów w tym pliku. Ile procent całego pliku stanowi:

   * `najczęściej występujące słowo w tym pliku`
   * `10, 100, 1000 najczęściej występujących słów w tym pliku`

Wskazówka: Zaczynamy od prostego EDA. Sprawdzamy, czy plik text8 zawiera wyłącznie znaki alfanumeryczne i białe:

```
tr --delete '[:alnum:][:blank:]' < text8 > deleted.txt
ls -l deleted.txt
  -rw-rw-r--. 1 wbzyl wbzyl 0 10-16 12:58 deleted.txt # rozmiar 0 -> OK
rm deleted.txt
```

Dopiero teraz wykonujemy te polecenia:

```
wc text8
  0         17005207 100000000 text8
tr --squeeze-repeats '[:blank:]' '\n' < text8 > text8.txt
wc text8.txt
  17005207  17005207 100000000 text8.txt  # powtórzone 17005207 -> OK
```

###Wyniki

Import bazy poleceniem `time mongoimport -d text -c text -type csv --fields 'word' --file text8.txt`

```
connected to: 127.0.0.1
Sat Nov  2 20:04:02.037 		Progress: 682283/100000000	0%
Sat Nov  2 20:04:02.037 			113100	37700/second
...
...
...
Sat Nov  2 20:10:15.007 		Progress: 99205240/100000000	99%
Sat Nov  2 20:10:15.007 			16870900	44869/second
Sat Nov  2 20:10:17.699 check 9 17005207
Sat Nov  2 20:10:17.823 imported 17005207 objects
```
Cała operacja trwała 6 minut i 18 sekund

```
real	6m18.181s
user	0m35.438s
sys	0m5.932s
```