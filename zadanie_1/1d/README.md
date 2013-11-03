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

## Słowa

### Najczęściej występujące słowo

```json
{ "result" : [ { "_id" : "the", "count" : 1061396 } ], "ok" : 1 }
```

```js
`the` występuje 1061396 razy co stanowi 6.241594118789616% wszystkich słów
```
###Czas

```
real	0m9.030s
user	0m0.040s
sys	    0m0.008s
```

### 10 najczęściej występujących słów

```json
	"result" : [
		{
			"_id" : "the",
			"count" : 1061396
		},
		{
			"_id" : "of",
			"count" : 593677
		},
		{
			"_id" : "and",
			"count" : 416629
		},
		{
			"_id" : "one",
			"count" : 411764
		},
		{
			"_id" : "in",
			"count" : 372201
		},
		{
			"_id" : "a",
			"count" : 325873
		},
		{
			"_id" : "to",
			"count" : 316376
		},
		{
			"_id" : "zero",
			"count" : 264975
		},
		{
			"_id" : "nine",
			"count" : 250430
		},
		{
			"_id" : "two",
			"count" : 192644
		}
	],
	"ok" : 1
}
```

```js
te słowa występują w sumie 4205965 razy co stanowi 24.733394894869555% wszystkich słów
```
###Czas

```
real	0m8.906s
user	0m0.032s
sys	    0m0.016s
```

### 100 najczęściej występujących słów

```json
	"result" : [
		{
			"_id" : "the",
			"count" : 1061396
		},
		{
			"_id" : "of",
			"count" : 593677
		},
		{
			"_id" : "and",
			"count" : 416629
		},
		{
			"_id" : "one",
			"count" : 411764
		},
		{
			"_id" : "in",
			"count" : 372201
		},
		...
		...
		...
		{
			"_id" : "history",
			"count" : 12623
		},
		{
			"_id" : "will",
			"count" : 12560
		},
		{
			"_id" : "up",
			"count" : 12445
		},
		{
			"_id" : "while",
			"count" : 12363
		},
		{
			"_id" : "where",
			"count" : 12347
		}
	],
	"ok" : 1
}

```

```js
te słowa występują w sumie 7998978 razy co stanowi 47.03840417820259% wszystkich słów
```
###Czas

```
real	0m8.951s
user	0m0.036s
sys	    0m0.016s
```

### 1000 najczęściej wystepujących słów

```json
	"result" : [
		{
			"_id" : "the",
			"count" : 1061396
		},
		{
			"_id" : "of",
			"count" : 593677
		},
		{
			"_id" : "and",
			"count" : 416629
		},
		{
			"_id" : "one",
			"count" : 411764
		},
		{
			"_id" : "in",
			"count" : 372201
		},
		...
		...
		...
		{
			"_id" : "child",
			"count" : 1789
		},
		{
			"_id" : "element",
			"count" : 1787
		},
		{
			"_id" : "appears",
			"count" : 1786
		},
		{
			"_id" : "takes",
			"count" : 1783
		},
		{
			"_id" : "fall",
			"count" : 1783
		}
	],
	"ok" : 1
}

```

```js
te słowa występują w sumie 11433354 razy co stanowi 67.23443001899359% wszystkich słów
```
###Czas

```
real	0m8.899s
user	0m0.084s
sys	    0m0.012s
```

### Wszystkie słowa
W sumie w bazie znajduje się 253854 słów

### Czas

```
real	0m10.123s
user	0m0.688s
sys	    0m0.080s
```