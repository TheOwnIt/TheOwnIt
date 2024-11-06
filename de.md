# Dokumentacja Projektu Integracja Plentymarkets - Baselinker

## 1. Opis projektu

Projekt "Integracja Plentymarkets - Baselinker" stanowi narzędzie zaprojektowane do zarządzania i automatyzacji integracji między systemami Plentymarkets i Baselinker. Aplikacja wykonana w Pythonie z użyciem biblioteki tkinter umożliwia uruchamianie, monitorowanie i zarządzanie zadaniami integracyjnymi z interfejsu graficznego.

## 2. Środowisko wykonawcze

- **Język programowania:** Python 3
- **Zależności zewnętrzne:** 
  - tkinter
  - datetime
  - subprocess
  - threading
- **System operacyjny:** Dowolny system wspierający Python 3

## 3. Struktura i opis modułów

### 3.1. Klasa IntegrationApp

#### Metody:

- `__init__(self, root)`: Konstruktor klasy, inicjalizuje interfejs użytkownika i strukturę danych integracji.
- `run_integration(self, key)`: Metoda do uruchamiania integracji. Uruchamia skrypt exe odpowiedzialny za daną integrację i inicjalizuje timer do cyklicznego uruchamiania.
- `toggle_integration(self, key)`: Metoda do włączania/wyłączania integracji. Kontroluje stan timera integracji.

#### Atrybuty:

- `root`: Główny widget aplikacji.
- `integrations`: Słownik zawierający konfigurację integracji, w tym czas wykonania, ostatnie uruchomienie, timer i ścieżkę do pliku wykonywalnego.

### 3.2. Struktura `integrations`

Słownik `integrations` przechowuje konfiguracje poszczególnych integracji:

```python
'integrations': {
    'Orders': {'interval': 900, 'last_run': None, 'timer': None, 'exe': 'orders.exe'},
    'Stocks': {'interval': 7200, 'last_run': None, 'timer': None, 'exe': 'stocks.exe'},
    'Status': {'interval': 900, 'last_run': None, 'timer': None, 'exe': 'status.exe'},
    'Products': {'interval': 86400, 'last_run': None, 'timer': None, 'exe': 'Products.exe'}
}
```

## 4. Uruchomienie i zakończenie pracy

Aplikację uruchamia się przez wykonanie pliku źródłowego. Aplikacja działa w pętli głównej Tkinter (`root.mainloop()`).

## 5. Zarządzanie zdarzeniami

Zarządzanie zdarzeniami w aplikacji odbywa się za pomocą przycisków w interfejsie graficznym, które uruchamiają i zatrzymują odpowiednie integracje.

## 6. Logika obsługi błędów

Aplikacja nie zawiera obecnie zaawansowanej obsługi błędów dla operacji zewnętrznych (np. uruchomienie pliku exe). Rekomendowane jest dodanie odpowiednich mechanizmów obsługi błędów dla pełnej produkcyjnej wersji.

## 7. Możliwości rozwoju

- Dodanie szczegółowych logów działania.
- Implementacja GUI do konfiguracji parametrów integracji.
- Rozszerzenie obsługi błędów i wyjątków.

## 8. Zależności

- Python 3.6 lub nowszy
- Biblioteka tkinter
- Pakiety datetime, subprocess, threading

## 9. Instalacja i konfiguracja

Instalacja wymaga zainstalowanego Pythona oraz wymienionych zależności. Uruchomienie aplikacji odbywa się przez komendę `python <nazwa_pliku>.py` w terminalu lub środowisku programistycznym wspierającym Pythona.


