## Dokumentacja Pluginu VatScraper do Plentymarkets

### Spis treści

1. [Wstęp](#wstęp)
2. [Struktura pluginu](#struktura-pluginu)
3. [Omówienie kluczowych klas](#omówienie-kluczowych-klas)
   - [VatScraperServiceProvider](#vatscraperserviceprovider)
   - [CreateShippingServiceProvider](#createshippingserviceprovider)
   - [ShippingServiceProvider](#shippingserviceprovider)
   - [Constants](#constants)
   - [Procedures](#procedures)
   - [ShipmentController](#shipmentcontroller)
4. [Funkcjonalność](#funkcjonalność)
5. [Integracje zewnętrzne](#integracje-zewnętrzne)
6. [Logowanie i debugowanie](#logowanie-i-debugowanie)

---

### Wstęp

Plugin **VatScraper** dla platformy **Plentymarkets** służy do zarządzania procesami zamówień, weryfikacji danych firmowych poprzez zewnętrzne API oraz integracji z systemem Odoo. Plugin ten automatyzuje procesy związane z obsługą zamówień i przetwarzaniem danych klientów, umożliwiając łatwiejsze zarządzanie zamówieniami i ich integrację z zewnętrznymi systemami.

### Struktura pluginu

Plugin składa się z kilku kluczowych komponentów:

- **Providers** - Dostawcy usług, takie jak `VatScraperServiceProvider`, które rejestrują usługi pluginu.
- **Migrations** - Klasy odpowiedzialne za migracje danych, takie jak `CreateShippingServiceProvider`.
- **Helpers** - Pomocnicze klasy i stałe, takie jak `ShippingServiceProvider` i `Constants`.
- **EventProcedures** - Procedury zdarzeń, które reagują na określone zdarzenia w systemie, takie jak `Procedures`.
- **Controllers** - Kontrolery, które zarządzają interakcją z zewnętrznymi usługami, takie jak `ShipmentController`.

### Omówienie kluczowych klas

#### VatScraperServiceProvider

Klasa `VatScraperServiceProvider` jest dostawcą usług dla pluginu VatScraper. Jest odpowiedzialna za rejestrację usług oraz procedur zdarzeń związanych z zamówieniami.

- **Metoda `register`**: Obecnie pusta, ale może być używana do rejestrowania usług pluginu.
  
- **Metoda `boot`**: Rejestruje procedurę zdarzeń `StartScrap` zdefiniowaną w klasie `Procedures`, która jest wywoływana, gdy wystąpi zdarzenie typu `ORDER`.

#### CreateShippingServiceProvider

Klasa `CreateShippingServiceProvider` zajmuje się migracją dostawców usług wysyłkowych w systemie Plentymarkets.

- **Właściwości**: 
  - `$shippingServiceProviderRepository` - Instancja repozytorium dostawców usług wysyłkowych.
  
- **Metoda `__construct`**: Inicjalizuje repozytorium dostawców usług wysyłkowych.
  
- **Metoda `run`**: Aktualnie pusta, ale służy do uruchamiania logiki migracji.

#### ShippingServiceProvider

Klasa `ShippingServiceProvider` definiuje stałe dla dostawcy usług wysyłkowych używane przez plugin.

- **Stałe**:
  - `PLUGIN_NAME` - Nazwa pluginu.
  - `SHIPPING_SERVICE_PROVIDER_NAME` - Nazwa dostawcy usług wysyłkowych.

#### Constants

Klasa `Constants` zawiera stałe używane w pluginie.

- **Stałe**:
  - `PLUGIN_NAME` - Nazwa pluginu.
  - `VOLUME_TYPE_FROM_SHIPPING_PACKAGE` - Typ objętości pochodzący z opakowania wysyłkowego.
  - `VOLUME_TYPE_FROM_ITEMS_DATA` - Typ objętości pochodzący z danych przedmiotów.

#### Procedures

Klasa `Procedures` definiuje logikę procedur zdarzeń uruchamianych w odpowiedzi na określone zdarzenia systemowe.

- **Właściwości**: Przechowują konfiguracje, tokeny API, repozytorium zamówień i inne niezbędne dane.
  
- **Metoda `StartScrap`**: Główna procedura uruchamiana po wystąpieniu zdarzenia `ORDER`. Pobiera zamówienia, weryfikuje dane klienta, a także integruje się z zewnętrznymi API do sprawdzania danych firmowych i rejestrowania leadów w systemie Odoo.

- **Metoda `CodeKitMagic`**: Wysyła zapytanie do zewnętrznego API w celu walidacji numeru VAT i zwraca nazwę firmy.

- **Metoda `checkCompanyDetails`**: Wysyła zapytania do API Outscraper w celu uzyskania szczegółów firmy.

- **Metoda `transfterOdoo`**: Przesyła dane firmy do systemu Odoo i tworzy nowy lead.

- **Metoda `getEmailScrap`**: Wysyła zapytanie do API Outscraper w celu uzyskania kontaktów i adresów email.

- **Metoda `getCountryIdByCode`**: Wysyła zapytanie do API Odoo w celu uzyskania identyfikatora kraju na podstawie kodu kraju.

#### ShipmentController

Klasa `ShipmentController` zarządza wysyłkami.

- **Metoda `getLabels`**: Obecnie zwraca pustą tablicę, może być używana do pobierania etykiet wysyłkowych.

### Funkcjonalność

Plugin VatScraper obsługuje wiele funkcji, w tym:

1. **Rejestracja procedur zdarzeń** - Automatyczne uruchamianie procedur w odpowiedzi na określone zdarzenia zamówień.
2. **Walidacja danych firmowych** - Weryfikacja numeru VAT i innych szczegółów firmy za pomocą zewnętrznego API.
3. **Integracja z Odoo** - Automatyczne przesyłanie danych do systemu Odoo, tworzenie leadów i aktualizacja danych.
4. **Obsługa zamówień** - Przetwarzanie i analizowanie zamówień, w tym dodatkowych zamówień tego samego klienta.

### Integracje zewnętrzne

Plugin integruje się z następującymi usługami zewnętrznymi:

- **Outscraper** - Używany do weryfikacji danych firmowych oraz pobierania kontaktów i adresów email.
- **Odoo** - System CRM używany do tworzenia leadów i zarządzania danymi firmowymi.
- **Zewnętrzne API do walidacji VAT** - Używane do walidacji numerów VAT klientów.

### Logowanie i debugowanie

Plugin intensywnie wykorzystuje logowanie do śledzenia błędów i procesów. Wszystkie główne operacje, w tym zapytania do zewnętrznych API, wyniki oraz błędy są logowane za pomocą metody `getLogger(Constants::PLUGIN_NAME)->error()`.

Logi są kluczowe dla debugowania i monitorowania działania pluginu w środowisku produkcyjnym. Zaleca się regularne sprawdzanie logów, aby upewnić się, że wszystkie procesy przebiegają poprawnie i aby szybko reagować na wszelkie nieprawidłowości.
