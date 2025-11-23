---
title: Dependency Injection
---

**Wstrzykiwanie zależności (DI)** jest mechanizmem oddzielenia
konstrukcji obiektu od jego użytkowania. Jest realizacją techniki
**Inversion of Control** do zarządzania zależnościami. Odwraca
realizację zależności poprzez przeniesienie drugorzędnej
odpowiedzialności z obiektu do innego obiektu, dedykowanego do tych
zadań, przez co zapewnia się zachowanie **Zasady Pojedynczej
Odpowiedzialności (SRP)**.

```{figure} img/di/di.*
:width: 80%
:align: center
```

W kontekście zarządzania zależnościami obiekt nie powinien być
odpowiedzialny za samodzielne tworzenie zależności. Powinien przekazywać
tę odpowiedzialność do innego \"autorytarnego\" mechanizmu, odwracając w
ten sposób sterowanie (np. do kontenera IoC).

# Przykład klasy z silnymi zależnościami.

W konstruktorze klasy `BusinessService` definiowane są zależności od
klas konkretnych. Nie ma możliwości zmiany implementacji usług
wykorzystywanych przez instancję klasy bez zmiany implementacji
konstruktora. Naruszona jest Zasada Otwarte/Zamknięte (OCP) oraz Zasada
Pojedynczej Odpowiedzialności (SRP), ponieważ instancja
`BusinessService` poza swoją podstawową odpowiedzialnością związaną z
logiką biznesową, hermetyzuje również odpowiedzialność związaną z
zarządzaniem zależnościami.

``` csharp
public class BusinessService
{
    private readonly string _databaseConnectionString
        = ConfigurationManager
            .ConnectionStrings["MyConnectionString"].ConnectionString;

    private readonly string _webServiceAddress
        = ConfigurationManager
            .AppSettings["MyWebServiceAddress"];

    private readonly LoggingDataSink _loggingDataSink;
    private DataAccessComponent _dataAccessComponent;
    private WebServiceProxy _webServiceProxy;
    private LoggingComponent _loggingComponent;

    public BusinessService()
    {
        _loggingDataSink = new LoggingDataSink();
        _loggingComponent = new LoggingComponent(_loggingDataSink);
        _webServiceProxy = new WebServiceProxy(_webServiceAddress);
        _dataAccessComponent = new DataAccessComponent(_databaseConnectionString);
    }

    public decimal GetPriceById(int id)
    {
        // implementacja wykorzystująca obiekty zależne
    }
}
```

Klasa `BussinessService` jest też słabo testowalna, ponieważ zależności
są zdefiniowane statycznie i nie mamy możliwości podstawienia w ich
miejsce obiektów pozorujących w celu odizolowania serwisu od bazy
danych, web-service\'u i komponentów logowania.

Wzorzec wstrzykiwania zależności stanowi alternatywny sposób
organizowania kodu w celu uniknięcia ścisłego powiązania
`BusinessService` z klasami zależnymi.

``` csharp
public class BusinessService
{
    private IDataAccessComponent _dataAccessComponent;
    private IWebServiceProxy _webServiceProxy;
    private ILoggingComponent _loggingComponent;

    public BusinessService(IDataAccessComponent dataAccessComponent,
                           IWebServiceProxy webServiceProxy,
                           ILoggingComponent loggingComponent)
    {
        _loggingComponent = loggingComponent;
        _webServiceProxy = webServiceProxy;
        _dataAccessComponent = dataAccessComponent;
    }

    public decimal GetPriceById(int id)
    {
        // implementacja wykorzystująca obiekty zależne
    }
}
```

Po implementacji techniki DI klasa `BusinessService` spełnia zasady OCP
i SRP oraz jest w pełni testowalna.

Typy wstrzykiwania zależności:

- Wstrzykiwanie zależności w oparciu o konstruktor oznacza, że obiekt
  otrzymuje zależności jako argumenty konstruktora.
- Wstrzykiwanie w oparciu o pole oznacza, że implementacja jest
  przekazywana bezpośrednio do zmiennej instancji. Otrzymujący ją obiekt
  nie ma możliwości przeprowadzenia żadnego przetwarzania w momencie
  otrzymania zależności.
