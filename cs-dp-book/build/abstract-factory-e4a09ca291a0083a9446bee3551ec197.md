---
title: Abstract factory - fabryka abstrakcyjna
---

**Przeznaczenie**

- Dostarcza interfejs do tworzenia całych rodzin spokrewnionych lub
  zależnych od siebie obiektów bez konieczności określania ich klas
  rzeczywistych

**Kontekst**

- W systemie istnieją rodziny powiązanych ze sobą obiektów (produktów),
  zaprojektowane tak, by obiekty były używane razem i ograniczenie to
  powinno być zachowane
- System powinien być niezależny od tego, jak jego produkty są tworzone,
  składowane i reprezentowane

**Problem**

- Chcemy umożliwić konfigurację systemu przy użyciu jednej z wielu
  rodzin produktów
- Kod powinien być zależny od interfejsów lub klas abstrakcyjnych
- Chcemy dostarczyć bibliotekę klas produktów, ujawniając tylko ich
  interfejsy, a nie implementacje

**Przykład**

```{figure} img/AbstractFactory-DbProvider.*
:width: 100%
:align: center
```

# Struktura

```{figure} img/AbstractFactory.*
:width: 100%
:align: center
```

# Uczestnicy

**IAbstractFactory** -- deklaruje interfejs tworzenia produktów
abstrakcyjnych

**ConcreteFactory** -- implementuje operacje tworzenia produktów
konkretnych

**IProduct** -- interfejs produktu

**ProductA**, **ProductB**

- definiuje obiekt będący produktem, który ma być utworzony przez
  odpowiednią fabrykę konkretną
- dziedziczy po interfejsie `Product`

**Client** -- używa jedynie interfejsów `IAbstractFactory`, `IProductA`
oraz `IProductB`

# Współpraca

- Podczas wykonywania programu jest zwykle tworzony jeden egzemplarz
  klasy `ConcreteFactory` - ta konkretna fabryka tworzy obiekty-produkty
  o pewnej szczególnej implementacji
- `AbstractFactory` zdaje się w kwestii tworzenia produktów na swoją
  podklasę (`ConcreteFactory`)

# Konsekwencje

1.  Odseparowanie klas konkretnych.
    - Pomaga zapanować nad tym, jakie klasy obiektów tworzy dana
      aplikacja. Ponieważ fabryka hermetyzuje odpowiedzialność i proces
      tworzenia obiektów-produktów, izoluje klientów od klas
      implementujących.
2.  Łatwiejsza wymiana rodzin produktów.
    - Klasa fabryki konkretnej pojawia się w aplikacji zwykle tylko raz
      -- tam, gdzie podejmowana jest decyzja o wyborze rodziny klas
      implementacji.
    - Umożliwia to łatwą zmianę fabryki konkretnej używanej przez
      aplikację.
3.  Spójność produktów.
    - Współpraca produktów wymaga, by aplikacja używała za jednym razem
      obiektów tylko z danej rodziny
4.  Utrudnione dołączenie nowych produktów.
    - Interfejs `IAbstractFactory` ustala zbiór obiektów, które mogą być
      utworzone. Obsługa nowych rodzajów produktów wymaga rozszerzenia
      interfejsu fabryki, co z kolei pociąga reimplementacje wszystkich
      podklas.
