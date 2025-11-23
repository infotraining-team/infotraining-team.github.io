---
title: Observer - obserwator
---

**Przeznaczenie**

- Określa zależność jeden-do-wiele między obiektami
- Gdy jeden obiekt zmienia stan, wszystkie obiekty od niego zależne są o
  tym automatycznie powiadamiane i uaktualniane

**Kontekst**

- Zmiana stanu jednego obiektu wymaga zmiany innych i nie wiadomo, ile
  obiektów trzeba zmienić

**Problem**

- Obiekt powinien być w stanie powiadamiać inne obiekty, nie przyjmując
  żadnych założeń co do tego, co te obiekty reprezentują -- wynikiem są
  luźniejsze powiązania między obiektami

# Struktura

```{figure} img/Observer.*
:width: 80%
:align: center
```

# Uczestnicy

**Subject**

- zna swoich obserwatorów
- może go obserwować dowolna liczba obserwatorów
- zapewnia interfejs dołączania i odłączania obserwatorów

**Observer**

- definiuje interfejs uaktualniania się obiektów, które powinny być
  powiadamiane o zmianach, jakie zaszły w obserwowanym obiekcie

**ConcreteSubject**

- przechowuje stan, który interesuje obiekty typu `ConcreteObserver`
- gdy zmienia się jego stan wysyła powiadomienia do swoich obserwatorów

**ConcreterObserver**

- utrzymuje odwołania do obiektu `ConcreteSubject`
- przechowuje stan, który powinien być spójny ze stanem obserwowanego
- implementuje interfejs obserwatora służący do uaktualniania stanu

# Współpraca

`ConcreteSubject` zawsze powiadamia swoich obserwatorów, gdy wystąpi
zmiana, która może doprowadzić do niespójności jego stanu ze stanem
obserwatora

Po otrzymaniu powiadomienia o zmianie, która wystąpiła w obserwowanym
obiekcie, `ConcreteObserver` może zapytać go o informacje dotyczące tej
zmiany. Obserwator używa tych informacji do uaktualnienia swojego stanu.

# Konsekwencje

1.  Abstrakcyjne powiązanie między obiektem obserwowanym a obserwatorem.
    Obserwowany wie, że ma listę obserwatorów, z których każdy
    dostosowuje się do interfejsu klasy `Observer`. Obserwowany i
    obserwator mogą należeć do różnych warstw abstrakcji w systemie.
2.  Wsparcie dla rozsyłania komunikatów. Powiadomienie jest
    automatycznie nadawane do wszystkich zainteresowanych obiektów,
    które je zaprenumerowały.
3.  Nieoczekiwane uaktualnienia. Pozornie nieszkodliwa operacja
    dotycząca obiektu obserwowanego może spowodować kaskadę uaktualnień
    w obserwatorach i obiektach od nich zależnych.

# Implementacja

1.  Obserwowanie więcej niż jednego obserwowanego. Obserwowany może po
    prostu przekazać siebie jako argument operacji `Update()`,
    informując w ten sposób obserwatora, którego obserwowanego powinien
    sprawdzić.
2.  Zagwarantowanie, że przed rozesłaniem powiadomienia stan
    obserwowanego jest wewnętrznie spójny.
3.  Przesyłanie informacji o zmianie do obserwatora -- dwa modele:
    - **model push** -- obserwowany wysyła szczegółową informację o
      zmianie (bez względu, czy obserwatorzy tego chcą, czy nie)
    - **model pull** -- obserwowany nie wysyła niczego poza
      powiadomieniem, a obserwatorzy jawnie pytają potem o szczegóły
4.  Nie można uzależniać poprawnego funkcjonowania aplikacji od
    określonej kolejności powiadamiania przesyłanego do obiektów
    obserwujących.

# Podsumowanie

1.  Umożliwia obiektom dynamiczne rejestrowanie zależności między
    obiektami, dzięki czemu obiekty mogą powiadamiać swoje obiekty
    zależne o istotnych zmianach swoich stanów.
2.  Obiekty obserwujące są luźno powiązane z obiektem obserwowanym --
    obiekt obserwowany wie o nich tylko tyle, że posiadają
    zaimplementowany interfejs klasy `Observer`.
3.  Informacje o zmianach stanu obiektu obserwowanego mogą być wysyłane
    lub pobierane.
