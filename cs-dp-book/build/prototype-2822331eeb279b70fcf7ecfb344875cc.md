---
title: Prototype - prototyp
---

**Przeznaczenie**

- Określa rodzaj tworzonych obiektów, używając prototypowego egzemplarza
- Tworzy nowe obiekty, kopiując ten prototyp

**Kontekst**

- Rzeczywiste typy obiektów, które chcemy utworzyć nie są znane
- Klasy, których instancje chcemy tworzyć, są ładowane dynamicznie
- Stan obiektów klasy może przyjmować tylko jedną z kilku dozwolonych
  wartości

**Problem**

- Chcemy tworzyć nowe egzemplarze obiektów bez wiedzy o tym, jaka jest
  ich konkretna klasa
- Chcemy uniknąć budowania hierarchii klas fabryk, która jest
  porównywalna z hierarchią klas produktów

# Struktura

```{figure} img/Prototype.*
:width: 80%
:align: center
```

# Uczestnicy

**IPrototype** -- deklaruje interfejs klonowania się

**ConcretePrototype** -- implementuje operację klonowania się

**Client** -- tworzy nowy obiekt, prosząc prototyp o sklonowanie się

# Współpraca

Klient prosi prototyp o sklonowanie się.

# Konsekwencje

1.  Dodawanie i usuwanie produktów w czasie wykonywania programu.
    `Prototype` ułatwia włączanie do systemu nowych produktów
    konkretnych przez rejestrowanie prototypowych egzemplarzy u klienta.
    Jest to bardziej elastyczne rozwiązanie niż w przypadku innych
    wzorców kreacyjnych.
2.  Umożliwia specyfikowanie nowych prototypowych obiektów przez
    urozmaicanie struktury. Złożone struktury definiowane w trakcie
    działania programu mogą być również klonowane.
3.  Zredukowana liczba podklas.

# Implementacja

Przy implementowaniu prototypów należy rozważyć:

1.  Używanie menedżera prototypów -- tablicy asocjacyjnej, która
    przekazuje prototyp pasujący do podanego klucza, mającej operacje do
    rejestrowania prototypu pod podanym kluczem i do wyrejestrowywania
    go.
2.  Implementowanie operacji `Clone()` -- płytka/głęboka kopia obiektu.
3.  Inicjowanie klonów -- jeśli w klasach rozważanego prototypu są już
    zdefiniowane operacje ustalania wartości istotnej części stanu
    obiektów, to klienci mogą użyć tych operacji bezpośrednio po
    klonowaniu.

# Wzorce pokrewne

1.  **Abstract Factory** -- rywalizuje z wzorcem `Prototype`. Można
    jednak używać obu wzorców razem. *Abstract Factory* może
    przechowywać zbiór prototypów, z których mają być klonowane
    obiekty-produkty.
2.  **Composite**, **Decorator** -- wzorce te są często używane wraz ze
    wzorcem `Prototype`.

# Podsumowanie

1.  Wzorzec umożliwia tworzenie nowych obiektów poprzez kopiowanie
    prototypowego egzemplarza.
2.  Klasy, których egzemplarze są klonowane, mogą być specyfikowane w
    trakcie wykonania programu.
3.  Pozwala uprościć hierarchię klas fabryk, która jest porównywalna z
    hierarchią klas produktów.
