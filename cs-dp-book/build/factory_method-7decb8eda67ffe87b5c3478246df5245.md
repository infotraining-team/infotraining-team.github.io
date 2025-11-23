---
title: Factory Method - metoda wytwórcza
---

Wzorzec *Factory Method* należy do grupy wzorców kreacyjnych i
rozwiązuje problem tworzenia obiektów, bez określania ich konkretnych
typów.

# Przeznaczenie Factory Method

- Definiuje interfejs pozwalający na tworzenie obiektów, ale
  odpowiedzialność za tworzenie obiektów jest delegowana do klas
  pochodnych.
- Wykorzystując mechanizm dziedziczenia pozwala klasom pochodnym
  decydować, jakiej klasy obiekt zostanie utworzony.

# Kontekst/Problem

Zasady OOP:

- Programuj pod kątem interfejsu, a nie implementacji.
- Preferuj zależności od klas abstrakcyjnych.
- Unikaj zależności od klas konkretnych, szczególnie wtedy, gdy
  charakteryzują się one dużą zmiennością.

Fragment kodu naruszającego powyższe zasady:

``` csharp
IShape shape = new Circle();
```

**Kontekst**

- Klasy konkretne charakteryzują się dużą zmiennością.

**Problem**

- Chcemy tworzyć instancje konkretnych klas w warunkach zależności tylko
  od abstrakcyjnych interfejsów.
- Klasa nie może przewidzieć, jakich klas obiekty musi tworzyć.
- Informacja o typie tworzonego obiektu (produktu) znana jest dopiero w
  czasie wykonywania programu.

# Rozwiązanie

```{figure} img/FactoryMethod.*
:width: 100%
:align: center
Schemat UML wzorca Factory Method
```

# Struktura i uczestnicy

**IProduct** -- definiuje interfejs obiektów tworzonych przez metodę
wytwórczą

**ConcreteProduct** -- implementuje interfejs `IProduct`

**Creator**

- deklaruje metodę wytwórczą, która przekazuje obiekt typu `IProduct`
- może implementować domyślną implementację metody wytwórczej, która
  przekazuje domyślny typ obiektu
- może wywoływać metodę wytwórczą, aby utworzyć obiekt implementujący
  interfejs `IProduct`

**ConcreteCreator** -- przedefiniowuje metodę wytwórczą, żeby
przekazywała konkretny produkt

Obiekt klasy `Creator` deleguje do swoich klas pochodnych
odpowiedzialność za takie zdefiniowanie metody wytwórczej, by
przekazywała egzemplarz odpowiedniej klasy `ConcreteProduct`
(implementującej interfejs `IProduct`).

# Konsekwencje

1.  Eliminuje potrzebę wstawiania specyficznych dla danej aplikacji klas
    w kod. Kod aplikacji odwołuje się jedynie do klasy abstrakcyjnej lub
    interfejsu IProduct, dlatego może działać z dowolnym produktem
    konkretnym.
2.  Tworzenie obiektów wewnątrz klasy za pomocą metody wytwórczej jest
    bardziej elastyczne niż tworzenie ich bezpośrednio.
3.  Wzorzec *Factory Method* daje klasom pochodnym punkt zaczepienia do
    dostarczenia rozszerzonej implementacji obiektu.
4.  Promuje luźne powiązania między obiektami, ponieważ redukuje
    zależność kodu aplikacji od konkretnych klas.
5.  Wzorzec *Factory Method* umożliwia łączenie równoległych hierarchii
    klas:
    - Równoległe hierarchie klas powstają wtedy, gdy klasa przekazuje
      niektóre ze swych zobowiązań odrębnej klasie.
    - Metoda wytwórcza pozwala zdefiniować związek między tak powstałymi
      hierarchiami.

# Implementacja

Istnieją dwie główne odmiany wzorca:

1.  Klasa `Creator` jest klasą abstrakcyjną/interfejsem i nie zapewnia
    implementacji dla deklarowanej metody wytwórczej. Klasy pochodne
    muszą definiować własną implementację.
2.  Klasa `Creator` jest klasą konkretną i zapewnia domyślną
    implementację metody wytwórczej.

## Sparametryzowane metody wytwórcze

- Metoda wytwórcza otrzymuje parametr identyfikujący rodzaj tworzonego
  obiektu.
- Parametryzacja umożliwia jednej metodzie wytwórczej tworzenie wielu
  rodzajów produktów.

``` csharp
public interface IShapeCreator
{
    IShape CreateShape(params object[] args);        
}

public class ShapeFactory
{
    private IDictionary<string, IShapeCreator> _creators = new Dictionary<string, IShapeCreator>();

    public IShape Create(string id, params object[] args)
    {
        return _creators[id].CreateShape(args);
    }

    public void Register(string id, IShapeCreator creator)
    {
        _creators.Add(id, creator);
    }

    public void Unregister(string id)
    {
        _creators.Remove(id);
    }
}
```

# Podsumowanie

*Factory Method* umożliwia inicjowanie przez jeden obiekt procesu
tworzenia innego obiektu w sytuacji, gdy nie jest znana klasa tworzonego
obiektu. Kod klienta jest nakierowany na interfejsy. Wzorzec *Factory
Method* umożliwia łączenie równoległych hierarchii klas.

Pokrewnym wzorcem do *Factory Method* jest wzorzec *Abstract Factory*.
Fabrykę abstrakcyją często implementuje się za pomocą metod wytwórczych.
