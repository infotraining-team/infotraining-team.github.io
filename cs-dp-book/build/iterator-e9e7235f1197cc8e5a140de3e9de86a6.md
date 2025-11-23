---
title: Iterator
---

**Przeznaczenie**

- Iterator stosujemy tam, gdzie potrzebujemy jednolitego interfejsu
  sposobu sekwencyjnego przeglądania elementów w zagregowanych obiektach
- Chcemy zapewnić sekwencyjny dostęp do elementów agregatu bez
  ujawniania jego reprezentacji wewnętrznej
  - iteracja po elementach agregatu jest zahermetyzowana w osobnym
    obiekcie
  - implementacja agregatu jest zwolniona z odpowiedzialności za obsługę
    operacji przeglądania danych

**Kontekst**

- Istnieje agregat, który powinien być przeglądany w sposób sekwencyjny

**Problem**

- Chcemy zapewnić sekwencyjny dostęp do agregatu bez ujawniania jego
  struktury wewnętrznej

# Struktura

```{figure} img/Iterator.*
:width: 80%
:align: center
```

# Uczestnicy

**Iterator** -- definiuje interfejs dostępu do elementów i przechodzenia
ich

**ConcreteIterator**

- implementuje interfejs iteratora
- pamięta bieżącą pozycję osiągniętą przy przechodzeniu agregatu

**Aggregate** -- definiuje interfejs tworzenia obiektów-iteratorów

**ConcreteAggregate** -- implementuje interfejs tworzenia iteratora tak,
by przekazywał egzemplarz odpowiedniego iteratora konkretnego
(`ConcreteIterator`) - jest to implementacja wzorca *Factory Method*

# Współpraca

Klasa `ConcreteIterator` śledzi, który obiekt w agregacie jest bieżący,
i potrafi wskazać następny obiekt.

# Konsekwencje

1.  Możliwość rozmaitego przechodzenia agregatów. Iteratory ułatwiają
    zmianę algorytmu przechodzenia (np. preorder, inorder). Wystarczy
    zastąpić jeden egzemplarz iteratora innym. Można też definiować
    podklasy iteratora, aby uwzględnić nowe sposoby przechodzenia.
2.  Iteratory upraszczają interfejs agregatu (`Aggregate`). Interfejs
    przechodzenia zawarty w klasie `Iterator` eliminuje potrzebę
    istnienia podobnego interfejsu w agregacie.
3.  W danej chwili może się odbywać więcej niż jedno przechodzenie
    agregatu. `Iterator` śledzi na bieżąco swój stan związany z
    przechodzeniem.

# Iterator -- implementacja .NET

Implementacja wzorca Iterator w .NET Framework wykorzystuje dwa
interfejsy:

- `IEnumerable`
- `IEnumerator`

```{figure} img/Iterator-IEnumerator.*
:width: 80%
:align: center
```

Możliwa jest uproszczone implementacja iteratora w .NET z wykorzystaniem
interfejsu `IEnumerable` i instrukcji `yield return`

```{figure} img/Iterator-yield-return.*
:width: 80%
:align: center
```

# Wzorce pokrewne

**Composite** -- iteratory stosuje się często do struktur
rekurencyjnych, takich jak kompozyty (*Composite*). **Factory Method**
-- iteratory polimorficzne wykorzystują metody wytwórcze do tworzenia
instancji klas implementujących interfejs `Iterator`

# Podsumowanie

1.  Zapewnia sekwencyjny dostęp do elementów obiektu zagregowanego bez
    ujawniania jego reprezentacji.
2.  Wylicza elementy agregatu i hermetyzuje te operacje w osobnym
    obiekcie.

Iterator zapewnia wspólny interfejs przeglądania elementów agregatu,
umożliwiając korzystanie z polimorfizmu przy pisaniu kodu, który
korzysta z tych elementów.
