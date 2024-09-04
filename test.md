### Dokumentacja aplikacji "Intrastat CSV Generator"

#### Opis aplikacji

Aplikacja "Intrastat CSV Generator" jest narzędziem desktopowym opartym na bibliotece Tkinter, które umożliwia generowanie plików CSV wymaganych do raportowania Intrastat. Aplikacja łączy się z serwerem Odoo, pobiera dane dotyczące faktur oraz produktów, a następnie tworzy pliki CSV zgodnie z określonymi specyfikacjami. Użytkownik może wybrać rok i miesiąc, dla którego dane mają być generowane, a także śledzić postęp tworzenia plików CSV za pomocą paska postępu.

#### Wymagania systemowe

- Python 3.x
- Biblioteki: `tkinter`, `ttk` (część standardowej biblioteki Tkinter), `requests`, `csv`, `calendar`, `threading`
- Dostęp do serwera Odoo z odpowiednimi uprawnieniami

#### Konfiguracja aplikacji

Przed uruchomieniem aplikacji należy skonfigurować połączenie z serwerem Odoo poprzez uzupełnienie poniższych zmiennych:

```python
odoo_url = "https://www.benbow.pl/jsonrpc"  # URL do serwera Odoo
odoo_db = "xxx"  # Nazwa bazy danych Odoo
odoo_username = xxx  # Nazwa użytkownika Odoo
odoo_password = "xxx"  # Hasło użytkownika Odoo
```

Zmienna `client_id` w funkcji `start_csv_generation` również powinna zostać dostosowana do konkretnego klienta, dla którego generowane są dane.

#### Instrukcja obsługi aplikacji

1. **Uruchomienie aplikacji**:
   - Aplikację uruchamiamy w środowisku Python za pomocą komendy `python script.py`, gdzie `script.py` to nazwa pliku z kodem aplikacji.

2. **Interfejs użytkownika**:
   - Po uruchomieniu aplikacji zobaczysz okno z polami wyboru dla roku i miesiąca oraz przyciskiem "Generuj CSV".
   - Wybierz odpowiedni rok i miesiąc, dla którego chcesz wygenerować pliki CSV.

3. **Generowanie plików CSV**:
   - Kliknij przycisk "Generuj CSV", aby rozpocząć proces generowania plików CSV.
   - Aplikacja rozpocznie pobieranie danych z Odoo w osobnym wątku, aby nie blokować interfejsu użytkownika.
   - W trakcie pobierania danych oraz generowania plików, pasek postępu będzie wskazywał bieżący stan operacji.

4. **Zakończenie procesu**:
   - Po zakończeniu generowania plików CSV pojawi się komunikat informujący o pomyślnym zakończeniu operacji.
   - Pliki CSV zostaną zapisane w bieżącym katalogu roboczym, z nazwami w formacie `intrastat_{rok}_{miesiąc}_{numer_faktury}.csv`.

#### Szczegóły dotyczące kodu

1. **Funkcja `get_products_for_client`**:
   - Pobiera faktury dla danego klienta (`client_id`) i określonego miesiąca/roku z Odoo.
   - Zwraca listę faktur z odpowiednimi pozycjami fakturowymi.

2. **Funkcja `get_product_details`**:
   - Pobiera szczegóły produktów na podstawie identyfikatorów produktów (`product_ids`) uzyskanych z faktur.
   - Zwraca listę szczegółowych informacji o produktach, takich jak identyfikator produktu, ilość, cena jednostkowa, itp.

3. **Funkcja `get_product_weight`**:
   - Pobiera wagę produktu oraz dodatkowe informacje, takie jak kod Intrastat i kraj pochodzenia.
   - Zwraca wagę oraz dodatkowe informacje o produkcie.

4. **Funkcja `get_country_code`**:
   - Tłumaczy nazwę kraju na jego kod dwuliterowy (ISO).
   - Używana do zamiany nazw krajów na ich odpowiednie kody w pliku CSV.

5. **Funkcja `generate_intrastat_csv`**:
   - Generuje pliki CSV dla określonego klienta i miesiąca/roku.
   - Pobiera dane dotyczące faktur i produktów, przetwarza je, a następnie zapisuje do pliku CSV.
   - Aktualizuje pasek postępu w interfejsie użytkownika.

6. **Funkcja `start_csv_generation`**:
   - Uruchamia proces generowania plików CSV w osobnym wątku, aby nie blokować interfejsu użytkownika.
   - Pobiera wybrane przez użytkownika wartości roku i miesiąca.

7. **Konfiguracja GUI**:
   - Tworzy główne okno aplikacji oraz interfejs użytkownika z elementami takimi jak etykiety, listy rozwijane (combobox) do wyboru roku i miesiąca, przycisk do generowania CSV oraz pasek postępu.

#### Uwagi dodatkowe

- Aplikacja jest zaprojektowana tak, aby działała asynchronicznie, co zapewnia płynność interfejsu użytkownika nawet podczas długotrwałych operacji.
- W przypadku błędów w komunikacji z Odoo, aplikacja wyświetla odpowiednie komunikaty w konsoli oraz oknie dialogowym Tkinter.
- Przed użyciem aplikacji upewnij się, że masz odpowiednie uprawnienia dostępu do serwera Odoo oraz że serwer Odoo jest prawidłowo skonfigurowany.

### Koniec dokumentacji

Aplikacja "Intrastat CSV Generator" powinna być teraz gotowa do użycia zgodnie z powyższymi instrukcjami. W razie potrzeby modyfikacji lub rozbudowy funkcjonalności, kod jest otwarty na dalsze dostosowania.
