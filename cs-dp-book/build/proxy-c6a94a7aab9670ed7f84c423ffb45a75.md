---
title: Proxy
---

**Przeznaczenie**

- Zapewnia substytut lub reprezentanta innego obiektu w celu sterowania
  dostępem do niego

**Kontekst**

- Tworzenie obiektów i ich inicjalizacja w trakcie działania programu
  jest kosztowne
- Potrzebna jest kontrola dostępu do obiektu

**Problem**

- Optymalizacja kosztownych procesów lub kontrola dostępu powinna być
  przezroczysta dla klienta

# Scenariusz

Chcemy napisać edytor dokumentów, który umożliwia osadzanie obiektów
graficznych:

- otwieranie dokumentów powinno być szybkie
- optymalizacja nie powinna mieć wpływu na części programu związane z
  wyświetlaniem czy formatowaniem

Rozwiązanie:

- użycie innego obiektu, pełnomocnika rysunku, który zastąpi prawdziwy
  rysunek
- pełnomocnik zachowuje się jak rysunek i zajmuje się jego utworzeniem,
  gdy jest to konieczne

```{figure} img/Proxy-Image.*
:width: 80%
:align: center
```

# Rodzaje obiektów Proxy

Proxy ma zastosowanie zawsze wtedy, gdy potrzeba bardziej uniwersalnego
lub bardziej wyrafinowanego odwołania do obiektu.

Rodzaje obiektów *Proxy*:

1.  **Remote proxy** -- jest lokalnym reprezentantem obiektu
    znajdującego się w innej przestrzeni adresowej (stub -- namiastka;
    .NET Remoting).
2.  **Virtual proxy** -- tworzy kosztowne obiekty na żądanie.
3.  **Protection proxy** -- kontroluje dostęp do oryginalnego obiektu.
4.  **Smart proxy** -- modyfikuje żądanie przed przesłaniem go do
    oryginalnego obiektu.

# Struktura

```{figure} img/Proxy.*
:width: 80%
:align: center
```

# Uczestnicy

**Proxy**

- przechowuje odwołanie, które umożliwia mu dostęp do prawdziwego
  przedmiotu
- zapewnia taki sam interfejs jak interfejs przedmiotu (`Subject`)
- obiekt *Proxy* może być zastąpiony obiektem typu `Subject`
- kontroluje dostęp do prawdziwego przedmiotu i może być odpowiedzialny
  za tworzenie przedmiotu oraz usuwanie

**Subject** -- klasa definiująca wspólny interfejs dla prawdziwego
przedmiotu (`RealSubject`) i pełnomocnika (`Proxy`)

**RealSubject** -- definiuje rzeczywisty przedmiot reprezentowany przez
pełnomocnika

# Współpraca

Obiekt pełnomocnika (*proxy*), jeśli trzeba, przekazuje żądania do
prawdziwego obiektu (*realSubject*) (w zależności od rodzaju
pełnomocnika).

# Konsekwencje

1.  Wzorzec *Proxy* wprowadza dodatkowy poziom pośredniości przy
    dostępie do obiektu.
2.  *Remote Proxy* -- może ukryć fakt, że obiekt znajduje się w innej
    przestrzeni adresowej/
3.  *Virtual Proxy* -- może wykonywać optymalizacje, np. tworzenie
    obiektu na żądanie, kopiowanie-przy-zapisaniu.
4.  *Protection Proxy* i *Smart Proxy* -- umożliwiają wykonywanie
    dodatkowych czynności porządkowych przy dostępie do obiektu.

# Implementacja

*Proxy* różnią się między sobą tym, jak bardzo ich implementacja jest
podobna do implementacji dekoratorów:

- *Protection Proxy* -- może być zaimplementowany dokładnie tak samo jak
  dekorator
- *Remote Proxy* -- nie zawiera bezpośredniego odwołania do swojego
  prawdziwego przedmiotu, a jedynie pośrednie (identyfikator hosta i
  lokalny adres na nim)

# Wzorce pokrewne

1.  **Adapter** -- zapewnia inny interfejs do adaptowanego obiektu.
    *Proxy* zapewnia taki sam interfejs, jak interfejs przedmiotu.
2.  **Decorator** -- pomimo podobnej implementacji, przeznaczenie wzorca
    Proxy jest inne. *Decorator* dodaje zobowiązania do obiektu, a
    *Proxy* steruje dostępem do obiektu.

# Podsumowanie

1.  Zapewnia obiekt pośrednika, dzięki któremu możemy optymalizować
    wywołanie kosztownych operacji lub kontrolować dostęp do oryginału.
2.  Interfejs obiektu *Proxy* jest taki sam jak interfejs oryginału.
