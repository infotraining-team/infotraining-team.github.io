---
title: Visitor - odwiedzający
---

**Przeznaczenie**

- Określa operację, która ma być wykonana na elementach struktury
  obiektowej
- Umożliwia definiowanie nowej operacji bez modyfikowania klas
  elementów, na których ona działa

**Kontekst**

- Wiele różnych i niepowiązanych ze sobą operacji musi być wykonanych na
  obiektach w pewnej strukturze obiektowej, a chcemy uniknąć
  „zanieczyszczenia" klas tych obiektów tymi operacjami -- *Visitor*
  umożliwia trzymanie powiązanych ze sobą operacji razem przez
  zdefiniowanie ich w jednej klasie
- Klasy definiujące strukturę obiektową rzadko się zmieniają, ale chcemy
  często definiować nowe operacje w tej strukturze

**Problem**

- Chcemy umożliwić łatwe dodawanie nowej operacji do struktury
  obiektowej bez konieczności otwierania klas tej struktury

# Struktura

```{figure} img/Visitor.*
:width: 80%
:align: center
```

# Uczestnicy

**Visitor**

- Deklaruje operację `Visit()` dla każdej klasy `ConcreteElement` w
  strukturze obiektowej. Nazwa i sygnatura operacji określają klasę,
  która wysyła żądania odwiedzenia (`Visit()`) do odwiedzającego
  (Visitor). Umożliwia to odwiedzającemu ustalenie klasy konkretnej
  odwiedzanego elementu. Odwiedzający może potem uzyskać bezpośredni
  dostęp do elementu poprzez jego interfejs.

**ConcreteVisitor** - implementuje każdą z operacji zadeklarowanych
przez `Visitor` (odwiedzającego). Każda operacja implementuje fragment
algorytmu zdefiniowanego dla odpowiedniej klasy obiektu w strukturze.
`ConcreteVisitor` zapewnia kontekst dla algorytmu i przechowuje jego
stan lokalny. Stan ten często kumuluje wyniki uzyskiwane podczas
przechodzenia struktury.

**Element** -- definiuje operację `Accept()`, otrzymującą odwiedzającego
jako argument

**ConcreteElement** -- implementuje operację `Accept()`, która otrzymuje
odwiedzającego jako argument

**ObjectStructure**

- Może podać swoje elementy
- Może zapewnić wysokopoziomowy interfejs umożliwiający odwiedzającemu
  odwiedzanie elementów
- Może być kompozytem lub kolekcją, taką jak lista czy zbiór

# Współpraca

- Klient stosujący wzorzec *Visitor* musi tworzyć obiekt
  `ConcreteVisitor`, a następnie przejść strukturę obiektową,
  odwiedzając każdy element za pomocą odwiedzającego (`Visitor`).
- Gdy jakiś element jest odwiedzany, wywołuje operację odwiedzającego
  odpowiednią dla swojej klasy. Element dostarcza siebie samego jako
  argument tej operacji, żeby -- jeśli to koniecznie -- umożliwić
  odwiedzającemu dostęp do jego stanu.

# Konsekwencje

1.  Łatwe dodawanie nowych operacji. Odwiedzający ułatwiają dodawanie
    operacji zależących od komponentów złożonych obiektów. Nową operację
    na strukturze obiektowej definiuje się po prostu przez dodanie
    nowego odwiedzającego (`Visitor`).
2.  Zebranie razem powiązanych ze sobą operacji, a rozdzielenie tych
    niepowiązanych. Powiązane ze sobą zachowania nie są rozsiane po
    wszystkich klasach definiujących strukturę obiektową, lecz są
    umiejscowione w klasie `Visitor`. Upraszcza to zarówno klasy
    definiujące elementy, jak i algorytmy zdefiniowane w odwiedzających.
3.  Trudne dodawanie nowych klas `ConcreteElement`.

# Implementacja

1.  Wykonywana operacja zależy zarówno od typu odwiedzającego, jak i od
    typu odwiedzanego przezeń elementu.
2.  Zamiast statycznie wiązać operacje w interfejsie `Element` można
    umieścić je w odwiedzającym (`Visitor`) i użyć `Accept()` w celu
    dokonania powiązania ich w czasie wykonywania programu.
3.  Rozszerzenie interfejsu `Element` sprowadza się do zdefiniowania
    jednej nowej podklasy `Visitor`, a nie wielu nowych podklas klasy
    `Element`.

# Wzorce pokrewne

**Composite** -- Odwiedzających można użyć do zastosowania operacji dla
wszystkich elementów struktury obiektowej, zdefiniowanej przez wzorzec
*Composite*.
