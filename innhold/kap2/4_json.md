JSON: Et dataformat (Per Edvard)
===================

JSON står for "JavaScript Object Notation" og er et tekstformat som er relativt leselig for mennesker, og enda enklere for maskiner å lese. Det er basert på JavaScript, men er ellers språkuavhengig.

To strukturer danner oppbyggingen til JSON:
- En samling av par basert på nøkkel og verdi
- En hierarkisk liste med verdier

Disse strukturene universelle og så å si alle moderne programmeringsspråk støtter disse i en eller annen form. Derfor passer JSON spesielt godt til å overføre data mellom forskjellige tjenester som kan være bygget med forskjellige programmeringsspråk.


Et eksempel på JSON kan for eksempel være:
```json
{
    "id": "12975534035527",
    "product_code": "DVSF65100022",
    "title": "Der ingen skulle tru at nokon kunne bu",
}
```
Et JSON-objekt kan ha ingen, ett, eller flere "nøkkel-verdi-par", hvor kolon skiller mellom nøkkel og verdi, og komma skiller mellom hvert par. Selve objektet er omgitt av klammeparenteser.
Eksemplet over viser ett JSON-objekt med tre nøkkel-verdi-par. 

Vi kan utvide eksemplet til innholde et underobjekt med egne nøkler og verdier:
```json
{
    "id": "12975534035527",
    "product_code": "DVSF65100022",
    "title": "Der ingen skulle tru at nokon kunne bu",
    "additional_metadata": {
        "category": "3.8.7 Personlig / Livsstil / Familie",
        "presentation_form": "2.1.4.99 Dokumentarserie",
        "age_restriction": "A",
        "production_year": 2022
    }
}
```
I eksemplet over ser vi også at flere datatyper enn strenger støttes for verdier. Datatypene som støttes er streng, nummer, objekt, liste, boolean, og null.

Videre kan vi f.eks utvide eksemplet til å innholde en liste med verdier:
```json
{
    "id": "12975534035527",
    "product_code": "DVSF65100022",
    "title": "Der ingen skulle tru at nokon kunne bu",
    "additional_metadata": {
        "category": "3.8.7 Personlig / Livsstil / Familie",
        "presentation_form": "2.1.4.99 Dokumentarserie",
        "age_restriction": "A",
        "production_year": 2022
    },
    "spoken_languages": ["norsk", "sunnmørsk"]
}
```

Og helt til sist, et eksempel med en liste av objekter:
```json
{
    "id": "12975534035527",
    "product_code": "DVSF65100022",
    "title": "Der ingen skulle tru at nokon kunne bu",
    "additional_metadata": {
        "category": "3.8.7 Personlig / Livsstil / Familie",
        "presentation_form": "2.1.4.99 Dokumentarserie",
        "age_restriction": "A",
        "production_year": 2022
    },
    "spoken_languages": ["norsk", "sunnmørsk"],
    "episodes" : [
        {
            "id": "12975764507527",
            "title" : "Øvre Brekkebakkane 3",
            "episode_number" : 1,
            "ready_for_broadcast": true,
        },
        {
            "id": "12975764519527",
            "title" : "Øvre Brekkebakkane 4",
            "episode_number" : 2,
            "ready_for_broadcast": false,
        },
    ]
}
```

I Python får man støtte for JSON gjennom standardbiblioteket `json`. 
Det er to funksjoner i dette biblioteket som du kommer til å benytte mye når du arbeider med JSON i Python, nemlig `loads()` og `dumps()`.
Disse funksjonene konverterer henholdsvis JSON fra og til et Python objekt (oppslagstabell).

Det første eksemplet vårt kan for eksempel se slikt ut når man konverterer en JSON-streng til en Python-oppslagstabell:
```python
import json

product_json = '{"id": "12975534035527", "product_code": "DVSF65100022", "title": "Der ingen skulle tru at nokon kunne bu"}'

product = json.loads(product_json)
print(product["title"])
```

Og omvendt når man konverterer fra en Python oppslags-tabell til en JSON-string:
```python
import json

product = {
    "id": "12975534035527", 
    "product_code": "DVSF65100022", 
    "title": "Der ingen skulle tru at nokon kunne bu"
}

json_string = json.dumps(product)
print(json_string)
```

I de aller fleste tilfeller hvor man arbeider med JSON kommer man ikke til å belage seg på at dataen er hardkodet i scriptet sitt.
Man ønsker nok heller å hente dataen fra et eksternt sted, f.eks. en fil eller et API. 

Vi har tidligere sett på hvordan man kan lese innhold fra en fil, så nå skal vi se videre på hvordan man kan lese JSON fra en fil for deretter å benytte det innebygde json-biblioteket i Python til å konvertere innholdet i fila til en oppslagstabell.

Hvis f.eks. fila inneholder kun ett JSON-objekt
```json
{
    "id": "12975534035527",
    "product_code": "DVSF65100022",
    "title": "Der ingen skulle tru at nokon kunne bu",
}
```

Så kan man lese hele innholdet av fila som beskrevet i kapittel 2.1, for deretter å benytte det innebygde json-biblioteket i Python for å konvertere slik at det kan legges i en oppslagstabell:
```python
import json

with open("data.json", "r", encoding="utf-8") as fil:
    json_data = fil.read()

data_dict = json.loads(json_data)
```

Dersom fila med JSON-data inneholder flere objekter per linje, f.eks. slik:
```json
{ "id": "12975534035527", "product_code": "DVSF65100022", "title": "Der ingen skulle tru at nokon kunne bu"},
{ "id": "13158833767527", "product_code": "NNFA21000023", "title": "Dagsrevyen 21"},
{ "id": "13272583632527", "product_code": "DVFJ20000023", "title": "Norge i dag"}
```

Så kan man lese en og en linje ved hjelp av metoden du lærte i kapittel 2.1, og deretter konvertere hver linje til en oppslagstabell som man legger i en liste:
```python
import json

with open("data.json", "r", encoding="utf-8") as fil:
    linjer = fil.readlines()

json_liste = []
for linje in linjer:
    json_liste.append(json.loads(linje))
```
Lista kalt `json_liste` vil da inneholde JSON-objektene du leste inn fra fila, da konvertert til oppslagstabeller.



TODO:
- Skrive data til en fil som JSON.