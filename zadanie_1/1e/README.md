## Treść


`zadanie 1e`. Wyszukać w sieci dane zawierające obiekty [`GeoJSON`](http://geojson.org/geojson-spec.html#examples). Zapisać dane w bazie `MongoDB`.

Dla zapisanych danych przygotować 6–9 różnych [`Geospatial Queries`](http://docs.mongodb.org/manual/applications/geospatial-indexes/) (co najmniej po jednym dla obiektów `Point`, `LineString` i `Polygon`). W przykładach należy użyć każdego z tych operatorów: `$geoWithin`, `$geoIntersect`, `$near`.

##Dane

Do rozwiązania zadania użyłem danych ze strony [`U.S. Geological Survey`](http://www.usgs.gov/) z działu [`United States Board on Geographic Names`](http://geonames.usgs.gov/) pt. [`Domestic and Antarctic Names`](http://geonames.usgs.gov/domestic/download_data.htm) dla stanu `Alabama`.

Źródło danych: [link](http://geonames.usgs.gov/docs/stategaz/AL_Features_20131020.zip)

###Dane

Przed wgraniem pliku do bazy należy najpierw go naprawić zamieniając wszystkie "|" na ",".

`cat AL_Features_20131020.txt | tr "|" "," > AL_Prepared.txt`

Importujemy do bazy poleceniem `time mongoimport -d geoal -c geoal --type csv --headerline --file AL_Prepared.txt`

## Wyniki 

```
connected to: 127.0.0.1
Sat Nov  2 20:40:04.886 check 9 62348
Sat Nov  2 20:40:04.957 imported 62347 objects
```

Wgrywanie bazy trwało niespełna 2 sekundy

```
real	0m1.907s
user	0m0.752s
sys	0m0.040s
```

##Zapytania

### $Near

```js
var punkt = { "type" : "Point", 
			"coordinates" : [ -73.6605406,  40.9844661 ] };
```

```js
db.geoal.find({ loc: {$near: {$geometry: punkt}, $maxDistance: 300} }).toArray()
```

```json
[
	{
		"_id" : ObjectId("52758765c95267aa51542c47"),
		"id" : 123910,
		"name" : "New Town",
		"loc" : {
			"type" : "Point",
			"coordinates" : [
				-85.8174748,
				34.8734153
			]
		}
	},
	{
		"_id" : ObjectId("52758765c95267aa5154675f"),
		"id" : 139265,
		"name" : "Rosenwald School (historical)",
		"loc" : {
			"type" : "Point",
			"coordinates" : [
				-85.8160859,
				34.8756376
			]
		}
	}
]
```
