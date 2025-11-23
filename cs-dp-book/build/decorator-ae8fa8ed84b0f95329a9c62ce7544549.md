---
title: Decorator - dekorator
---

# Przeznaczenie wzorca Decorator

- Pozwala na dynamiczne przydzielanie danemu obiektowi nowych zachowań.
- Zapewnia elastyczną alternatywę dla tworzenia nowych klas pochodnych w
  celu rozszerzania funkcjonalności.

# Kontekst/Problem

- Chcemy rozszerzyć funkcjonalność obiektu w sposób przezroczysty dla
  klienta.
- Nowe zachowanie lub nowy stan powinny być dodawane dynamicznie w
  trakcie działania programu.

Chcemy rozszerzyć funkcjonalność obiektu Photo i dodać do zdjęcia ramkę
oraz dwa tagi reprezentujące opis zdjęcia.

```{figure} img/decorated-coffee.*
:width: 60%
:align: center
```

**Możliwe rozwiązania**

1.  Dodanie nowego zobowiązania przez zastosowanie dziedziczenia.
    Rozwiązanie mało elastyczne (statyczne).

```{figure} img/Decorator_1.*
:width: 80%
:align: center
```

2.  Umieszczenie komponentu w innym obiekcie, który dodaje ramkę.
    - Obiekt będący otoczką komponentu nazywa się dekoratorem.
    - Dekorator dostosowuje się do interfejsu ozdabianego obiektu,
      dzięki czemu staje się przezroczysty dla klientów.
    - Dekorator przekazuje żądania do komponentu i może wykonywać
      dodatkowe akcje.

```{figure} img/Decorator_2.*
:width: 80%
:align: center
```

Wzorzec dekoratora umożliwia składanie obiektu `Photo` z dekoratorami
`BorderedPhoto` i `TaggedPhoto`.

```{figure} img/Decorator_3.*
:width: 80%
:align: center
```

Dziedziczenie jest jedną z form rozszerzenia funkcjonalności klasy, ale
niekoniecznie musi być najlepszym sposobem na osiągnięcie w pełni
elastycznych projektów aplikacji. Tworząc projekt aplikacji, należy go
tak skonstruować, aby możliwe było rozszerzanie zachowań poszczególnych
elementów bez konieczności modyfikowania istniejącego kodu.
Wykorzystując kompozycję oraz delegację, można dodawać nowe zachowania
podczas działania programu. Wzorzec Dekorator posługuje się zbiorem klas
dekorujących (dekoratorów), które są wykorzystywane do dekorowania
poszczególnych obiektów (składników).

# Stosowalność

Wzorzec `Decorator` powinien być stosowany:

- Aby dynamicznie i w przezroczysty sposób (tzn. nie wpływający na inne
  obiekty) dodać zobowiązania do pojedynczych obiektów.
- W wypadku zobowiązań, które mogą być cofnięte.
- Gdy rozszerzanie funkcjonalności przez definiowanie podklas jest
  niepraktyczne -- czasami jest możliwych wiele niezależnych rozszerzeń,
  które przy próbie uwzględnienia każdej z ich kombinacji prowadzą do
  gwałtownego wzrostu liczby klas.

# Struktura i uczestnicy

**IComponent** -- definiuje interfejs obiektów, do których można
dynamicznie dołączyć zobowiązania.

**ConcreteComponent** -- konkretna klasa definiująca komponent, z
którego korzysta klient.

```{figure} img/Decorator.*
:width: 80%
:align: center
```

**Decorator** -- zarządza odwołaniem do obiektu typu `IComponent` i
definiuje interfejs dopasowany do interfejsu IComponent.

**ConcreteDecoratorA, ConcreteDecoratorB** - dodaje zobowiązanie do
komponentu.

**Client -- używa zarówno obiektów typu IComponent** - konkretnych
komponentów jak i ich dekorowanych wersji.

# Konsekwencje

1.  Większa elastyczność niż przy stosowaniu statycznego dziedziczenia.
    - Mając dekoratory można dodawać i usuwać zobowiązania w czasie
      wykonywania programu. Uwzględnienie różnych implementacji klasy
      `Decorator` dla określonej klasy komponentu umożliwia mieszanie i
      dopasowywanie zobowiązań.
    - Dekoratory ułatwiają także dwukrotne dołączanie właściwości (np.
      fotografia z podwójną ramką).
2.  Unikanie przeładowania właściwościami klas na szczycie hierarchii.
    - Możliwe jest zdefiniowanie prostej klasy i przyrostowe
      rozszerzanie jej funkcjonalności za pomocą obiektów dekoratora.
      Nowe rodzaje dekoratorów są łatwe do zdefiniowania.
3.  Dekorator i jego komponent nie są identyczne.
    - Dekorator działa jak przezroczysta otoczka, jednak z punktu
      widzenia identyczności obiektów udekorowany komponent nie jest
      taki sam jak ten wyjściowy.
4.  Wiele małych obiektów.
    - Projekty wykorzystujące dekoratory prowadzą często do powstawanie
      systemów z dużą liczbą małych, podobnych do siebie obiektów.

# Implementacja

1.  Zgodność interfejsów.
    - Interfejs obiektu będącego dekoratorem musi odpowiadać
      interfejsowi dekorowanego przez niego komponentu. Klasy
      `ConcreteDecorator` muszą dziedziczyć po wspólnej klasie.
2.  Pomijanie klasy abstrakcyjnej `Decorator`.
    - Gdy zależy nam na dodaniu tylko jednego zobowiązania, nie musimy
      definiować klasy abstrakcyjnej `Decorator`.
3.  Gdy `Component` jest klasą abstrakcyjną, nie należy przeciążać jej
    dużą ilością pól składowych.

# Podsumowanie

- Dekorator umożliwia dynamiczne dodanie zobowiązanie do obiektu.
- Posługuje się zbiorem klas dekorujących (dekoratorów), które są
  wykorzystywane do dekorowania poszczególnych obiektów (składników).
- Klasy dekorujące odzwierciedlają typy obiektów dekorowanych.
- Dekoratory są tego samego typu, co obiekty dekorowane, niezależnie,
  czy zostało to osiągnięte metodą dziedziczenia czy implementacji
  odpowiednich interfejsów.
- Dekoratory zmieniają zachowania obiektów dekorowanych (składników),
  dodając nowe zachowania przed wywołaniami metod danego składnika i
  (lub) po nich lub nawet pomiędzy nimi.
- Każdy składnik może być \"otoczony\" dowolną ilością dekoratorów.

Pokrewnymi wzorcami *Dekoratora* są wzorce *Adapter*, *Composite* i
*Strategy*. Wzorzec *Adapter* dodaje obiektowi nowy interfejs.
*Dekorator* można uważać za zdegenerowany *Composite*, z jednym
komponentem. *Dekorator* jednak dodaje dodatkowe zobowiązania, nie jest
przeznaczony do agregacji obiektów. *Strategy* umożliwia zmianę wnętrza
obiektu.
