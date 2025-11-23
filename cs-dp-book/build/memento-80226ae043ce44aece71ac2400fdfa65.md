---
title: Memento - pamiątka
---

**Przeznaczenie**

- Nie naruszając kapsułkowania, zapamiętuje i udostępnia na zewnątrz
  stan wewnętrzny obiektu, tak że obiekt może być później przywrócony do
  zapamiętanego stanu
- *Memento* jest obiektem przechowującym migawkę wewnętrznego stanu
  innego obiektu -- jej źródła
- Mechanizm anulowania prosi źródło o instancje klasy `Memento` wtedy,
  kiedy chce ustawić punkt kontrolny stanu źródła
- Źródło inicjuje memento informacjami opisującymi jego stan bieżący
- Jedynie źródło może tworzyć/odtwarzać informacje z `Memento`
- Obiekt klasy `Memento` jest dla innych obiektów „nieprzezroczysty"

**Kontekst**

- Aplikacja wymaga zapamiętania migawki stanu obiektu w celu jego
  późniejszego przywrócenia (np. w operacji `Undo()`)

**Problem**

- Chcemy przechować migawkę stanu bez naruszania hermetyzacji obiektu

# Struktura

```{figure} img/Memento.*
:width: 80%
:align: center
```

# Uczestnicy

**Memento**

- przechowuje stan wewnętrzny obiektu `Originator` -- i to taką jego
  część, jaka wg źródła powinna być przechowana
- chroni stan przed dostępem ze strony innych obiektów niż źródło
- pamiątki mają faktycznie dwa interfejsy
- opiekun widzi zawężony interfejs pamiątki -- może jedynie przekazać
  pamiątkę innym obiektom
- źródło widzi szeroko zakrojony interfejs, który umożliwia mu uzyskanie
  dostępu do wszystkich danych niezbędnych do odtworzenia swojego
  poprzedniego stanu
- najlepiej byłoby, gdyby tylko to źródło, które wytworzyło pamiątkę,
  miało dostęp do jej stanu wewnętrznego

**Originator**

- tworzy pamiątkę zawierającą migawkę swojego bieżącego stanu
  wewnętrznego
- wykorzystuje pamiątkę do odtworzenia swojego stanu wewnętrznego

**Caretaker**

- jest odpowiedzialny za opiekę nad pamiątką
- nigdy nie wykonuje operacji na pamiątce ani nie bada jej zawartości

# Współpraca

Opiekun (`Caretaker`) prosi źródło (`Originator`) o pamiątkę, przez
jakiś czas ją przechowuje, a następnie zwraca źródło.

Pamiątki są pasywne. Jedynie źródło, które stworzyło pamiątkę będzie
przypisywać bądź odtwarzać jej stan.

# Konsekwencje

1.  Zachowanie granic hermetyzacji.
    - Pamiątka umożliwia uniknięcie ujawniania informacji, którymi
      jedynie źródło powinno zarządzać, ale które mimo tego muszą być
      przechowywane poza nim. Wzorzec ten izoluje inne obiekty od
      potencjalnie złożonego wnętrza źródła, zachowując w ten sposób
      granice kapsułkowania.
2.  Uproszczenie implementacji obiektu źródła.
3.  Użycie pamiątek może być kosztowne. Jeśli hermetyzacja i odtwarzanie
    stanu źródła jest kosztowne, wzorzec ten może okazać się
    nieodpowiedni.
4.  Ukryte koszty związane z opieką nad pamiątkami.

# Wzorce pokrewne

**Command** z możliwością anulowania wykonanej operacji. Obiekt
`Memento` może służyć do przywrócenia stanu aplikacji.

# Podsumowanie

1.  Zapamiętuje i udostępnia bez naruszenia zasad hermetyzacji
    wewnętrzny stan obiektu.
2.  Dzięki pamiątce obiekt może być przywrócony do poprzedniego stanu.
