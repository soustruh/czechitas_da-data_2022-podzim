# Poznámky k lekci Python Data z 20. 10. (web scraping)

Ahoj,
omlouvám se, že to tak trvalo, ale konečně jsem se dostal k sepsání slíbených poznámek.

## Vzorové řešení [úlohy č. 2](https://kodim.cz/kurzy/python-data-1/ziskavani-dat/webscraping/webscraping#excs%3Escraping-kodim.cz) z kodim.cz

### Zadání

Napište program, který vypíše na výstup všechny povinné a nepovinné domácí úložky z lekce První programy spolu s jejich obtížností.

### Jdeme na to! 💪

Na stránce v dolní části vidím bloky s jednotlivými úlohami, v každém bloku název úlohy a její obtížnost. Jako první mě napadne, že si projdu jednotlivé bloky a v každém z nich pak najdu ty názvy a obtížnosti. Jenže mi hned dojde, že některé z vás jako začátečnice nepřijímají několikeré zanoření úplně dobře, takže na to půjdu jinak: získám všechny názvy a všechny obtížnosti, uložím je do dvou seznamů a pak je postupně společně vypíšu, což mi dá možnost ukázat vám několik způsobů propojení stejně dlouhých seznamů.

### Nalezení správných CSS selektorů

Klikám pravým tlačítkem na jeden z nadpisů a vyberu `Prozkoumat prvek`, `Inspect element` nebo podobně znějící volbu. Všimnu si, že ačkoli jsem kliknul na nadpis (`<h3>`), ve skutečnosti se dívám na vlastnosti v něm vnořeného odkazu (`<a>`). Taky si všimnu, že pokud budu chtít pracovat s nadpisem, musím si dát pozor na další nadpisy na stránce. Nadpis místo odkazu volím proto, že název jeho třídy je kratší. 😇 Můj vybraný selektor pro výběr nadpisů je tedy `h3.exercise-assign__title` (element H3 třídy exercise-assign__title). Obdobně postupuji pro získání selektoru pro obtížnost: `div.demand-text`.

<img src="web_scraping.png" width="640">

### Vytvoření seznamů názvů úloh a obtížností

```py
from requests_html import HTMLSession  # importuji potřebný modul

session = HTMLSession()  # vytvářím nový objekt, pomocí kterého stránku stáhnu a zpracuju (parsuju)

print("## Úloha č. 2:")

# staženou a zpracovanou stránku uložím do proměnné stranka_2
stranka_2 = session.get("https://kodim.cz/kurzy/python-data/zaklady-programovani/prvni-programy/cteni-na-doma")  

nadpisy = []  # vytvořím si prázdný seznam pro všechny nadpisy
# projdu každý z nadpisů dané třídy, v rámci cyklu si ho uložím do proměnné `nadpis`…
for nadpis in stranka_2.html.find("h3.exercise-assign__title"):
    # …a text obsažený v tomto nadpisu připojím na konec seznamu `nadpisy`
    nadpisy.append(nadpis.text)

# opakuji předchozí 3 kroky pro obtížnosti
obtiznosti = []
for obtiznost in stranka_2.html.find("div.demand-text"):
    obtiznosti.append(obtiznost.text)
```

Nyní jsem tedy získal 2 seznamy – `nadpisy`:
```py
["Minuty", "Fahrnheit vs. Celsius", …]
```
a `obtiznosti`:
```py
["to dáš", "to dáš", …]
```

### Propojení obou seznamů

Propojení obou seznamů se dá udělat mnoha způsoby, a tak jsme si jich rovnou ukázali několik.

#### Pomocí funkce range() 🎯

```py
for n in range(0, len(nadpisy)):
    print(nadpisy[n] + " 👉 " + obtiznosti[n])
```

#### Pomocí čítače poprvé 🧮

```py
j = 0
while j < len(nadpisy):
    print(nadpisy[j] + " 👉 " + obtiznosti[j])
    j += 1
```

#### Pomocí čítače podruhé ♾️

```py
j = 0
while True:
    if j > len(nadpisy) - 1:
        break
    print(nadpisy[j] + " 👉 " + obtiznosti[j])
    j += 1
```

#### Pomocí zipu 🤐

```py
for nadpis, obtiznost in zip(nadpisy, obtiznosti):
    print(nadpis + " 👉 " + obtiznost)
```
