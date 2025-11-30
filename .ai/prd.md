# Dokument wymagań produktu (PRD) - Planning Helper

## 1. Przegląd produktu
Planning Helper to aplikacja internetowa przeznaczona dla menedżerów produktów (Product Ownerów), której celem jest uproszczenie procesu śledzenia dostępności członków zespołu i precyzyjnego obliczania ich zdolności produkcyjnej (capacity). Aplikacja umożliwia importowanie harmonogramów nieobecności z predefiniowanego formatu pliku Excel, zarządzanie strukturą zespołów i ról, a także wizualizację danych w formie kalendarza. Główne funkcje obejmują automatyczne obliczanie dostępnych osobodni (MD) dla poszczególnych ról w zadanym okresie, co pozwala na bardziej realistyczne planowanie pracy i prognozowanie realizacji zadań.

## 2. Problem użytkownika
Obecnie określanie capacity zespołu na przyszłe okresy jest trudne i nieprecyzyjne. Proces ten komplikują nieprzewidziane nieobecności, które wpływają na ilość realizowanych zadań. Product Ownerzy nie mają dostępu do centralnego systemu HR, a ręczne wprowadzanie i aktualizowanie danych o dostępności w narzędziach takich jak Azure DevOps jest niewygodne i rzadko praktykowane przez członków zespołu. Informacje o planowanych i nagłych nieobecnościach są rozproszone – częściowo znajdują się w pliku Excel, a częściowo przekazywane są ustnie lub na komunikatorach. Prowadzi to do niespójności danych i utrudnia efektywne planowanie sprintów oraz długoterminowych projektów.

## 3. Wymagania funkcjonalne

### 3.1. Uwierzytelnianie i Autoryzacja
- System nie posiada publicznej rejestracji. Konta użytkowników (Product Ownerów) są tworzone bezpośrednio w bazie danych.
- Użytkownik musi się zalogować, aby uzyskać dostęp do aplikacji.

### 3.2. Zarządzanie Zespołami
- Po zalogowaniu użytkownik widzi listę swoich zespołów.
- Jeśli użytkownik nie ma zdefiniowanych zespołów, wyświetlany jest ekran powitalny z zachętą do stworzenia pierwszego zespołu.
- Użytkownik może tworzyć nowe zespoły, podając ich nazwę.
- Użytkownik może edytować nazwę istniejącego zespołu.
- Użytkownik może usunąć cały zespół, co powoduje trwałe usunięcie wszystkich powiązanych z nim danych (członków, ról, historii importów).

### 3.3. Zarządzanie Rolami
- Dla każdego zespołu użytkownik może zdefiniować listę ról (np. "Developer Frontend", "Tester", "Analityk").
- Nazwy ról muszą być unikalne w obrębie jednego zespołu.
- Użytkownik może edytować nazwę istniejącej roli.

### 3.4. Zarządzanie Członkami Zespołu
- Użytkownik może dodawać członków do zespołu, podając ich imię i nazwisko, przypisując rolę oraz opcjonalnie datę rozpoczęcia i zakończenia pracy.
- System uniemożliwia dodanie do jednego zespołu dwóch osób o tym samym imieniu i nazwisku.
- Nie ma możliwości usunięcia członka zespołu. Zmiana roli lub zakończenie pracy jest realizowane przez ustawienie daty końcowej i ewentualne dodanie tej samej osoby z nową rolą i datą początkową.
- Wszyscy członkowie zespołu w ramach MVP mają wymiar etatu 1.0.

### 3.5. Import Danych z Pliku Excel
- Aplikacja umożliwia import nieobecności z pliku `.xlsx`.
- Każdy kolejny import dla tego samego zespołu zastępuje poprzednie dane, a poprzedni zestaw danych jest archiwizowany (zmiana statusu w bazie danych, bez dostępu z poziomu UI).
- Format pliku Excel jest stały: arkusze nazwane rokiem (np. `2024`), a w nich 12 tabel (po jednej dla każdego miesiąca), oddzielonych pustymi wierszami.
- Parser importu identyfikuje tabele na podstawie nazw miesięcy (np. "Styczeń", "Luty"), ignorując wielkość liter.
- Nieobecność w pliku jest oznaczona dowolnym kolorem tła komórki innym niż biały.
- Identyfikacja członków zespołu odbywa się na podstawie imienia i nazwiska.
- Import generuje log z ostrzeżeniami i błędami.
- Walidacja importu:
    - Proces jest przerywany, jeśli w pliku brakuje tabeli dla któregokolwiek z 12 miesięcy.
    - Proces jest przerywany, jeśli w tabeli w pliku Excel znajduje się osoba, która nie jest zdefiniowana w zespole w aplikacji (z informacją o numerze wiersza).
    - Jeśli członek zespołu zdefiniowany w aplikacji nie zostanie znaleziony w tabeli dla danego miesiąca w pliku Excel, jest traktowany jako nieobecny przez cały ten miesiąc, a informacja trafia do logu jako ostrzeżenie.

### 3.6. Wizualizacja Danych
- Główny widok aplikacji to kalendarz nieobecności zespołu.
- Kalendarz prezentuje dane dla 6 miesięcy jednocześnie, w układzie pionowym (jeden pod drugim). Domyślnie widok startuje od miesiąca poprzedzającego bieżący.
- Pierwsza kolumna z imionami i nazwiskami jest "przylepiona" do ekranu i pozostaje widoczna podczas przewijania horyzontalnego.
- Komórki kalendarza są kolorowane w celu łatwej identyfikacji statusu:
    - Granatowy: nieobecność (na podstawie importu).
    - Żółty: weekend (sobota, niedziela) lub święto państwowe.
    - Szary: dzień poza zdefiniowanym okresem pracy członka zespołu.
- Lista członków zespołu jest posortowana alfabetycznie według ról.
- Nad i pod kalendarzem znajduje się nawigacja (strzałki), umożliwiająca przewijanie widoku o jeden miesiąc do przodu lub do tyłu.

### 3.7. Obliczanie Capacity
- Pod widokiem kalendarza znajduje się formularz z polami wyboru daty (`Data od`, `Data do`) oraz przyciskiem "Oblicz".
- Po obliczeniu system wyświetla dwie tabele podsumowujące:
    1.  Podsumowanie per członek zespołu: `Imię i Nazwisko`, `Rola`, `Planowane MD` (dni robocze w okresie), `Nieobecności w MD`, `Dostępne MD`.
    2.  Podsumowanie per rola: `Rola`, `Planowane MD`, `Nieobecności w MD`, `Dostępne MD`.
- Obliczenia `Nieobecności w MD` nie uwzględniają sobót, niedziel i świąt.
- System korzysta z wbudowanego słownika świąt państwowych.

## 4. Granice produktu
Następujące funkcjonalności i cechy znajdują się poza zakresem wersji MVP (Minimum Viable Product):
- Współdzielenie informacji (konfiguracji zespołów, ról) między różnymi użytkownikami (Product Ownerami).
- Edycja nieobecności członków zespołu bezpośrednio w widoku kalendarza.
- Dedykowana aplikacja mobilna.
- Integracja z zewnętrznymi systemami, takimi jak Azure DevOps.
- Funkcjonalność wyznaczania i porównywania różnicy między planowanym a faktycznym capacity po zakończeniu okresu.
- Obsługa różnych wymiarów etatu (innych niż 1.0).
- Dostęp do zarchiwizowanych danych z poziomu interfejsu użytkownika.

## 5. Historyjki użytkowników

### ID: US-001
- Tytuł: Logowanie do aplikacji
- Opis: Jako Product Owner, chcę móc zalogować się do aplikacji przy użyciu moich danych uwierzytelniających, aby uzyskać dostęp do moich zespołów i danych o ich dostępności.
- Kryteria akceptacji:
    - 1. Na stronie logowania znajdują się pola na login i hasło oraz przycisk "Zaloguj".
    - 2. Po wprowadzeniu poprawnych danych uwierzytelniających (założonych w bazie) i kliknięciu "Zaloguj", zostaję przekierowany do głównego widoku aplikacji (listy zespołów lub kalendarza).
    - 3. Po wprowadzeniu niepoprawnych danych, pod formularzem wyświetla się komunikat o błędzie "Nieprawidłowy login lub hasło".
    - 4. Użytkownik niezalogowany nie ma dostępu do żadnej innej strony poza stroną logowania.

### ID: US-002
- Tytuł: Tworzenie nowego zespołu
- Opis: Jako Product Owner, chcę mieć możliwość stworzenia nowego zespołu, aby móc zarządzać jego składem, rolami i dostępnością.
- Kryteria akceptacji:
    - 1. W widoku listy zespołów znajduje się przycisk "Stwórz nowy zespół".
    - 2. Po kliknięciu przycisku pojawia się formularz z polem na nazwę zespołu.
    - 3. Po wprowadzeniu nazwy i zatwierdzeniu, nowy zespół pojawia się na liście moich zespołów.
    - 4. Nazwa zespołu nie może być pusta i musi mieć maksymalnie 100 znaków.

### ID: US-003
- Tytuł: Widok powitalny dla nowego użytkownika
- Opis: Jako nowy Product Owner, który loguje się po raz pierwszy i nie ma jeszcze żadnych zespołów, chcę zobaczyć stronę powitalną, która zachęci mnie do stworzenia pierwszego zespołu.
- Kryteria akceptacji:
    - 1. Po zalogowaniu, jeśli nie mam zdefiniowanych żadnych zespołów, widzę stronę z komunikatem powitalnym.
    - 2. Na stronie znajduje się wyraźny przycisk lub link "Stwórz swój pierwszy zespół", który prowadzi do formularza tworzenia zespołu.
    - 3. Na tej stronie nie ma listy zespołów ani innych elementów interfejsu.

### ID: US-004
- Tytuł: Przeglądanie listy zespołów
- Opis: Jako Product Owner, chcę widzieć listę wszystkich moich zespołów, aby móc szybko wybrać ten, którym chcę zarządzać.
- Kryteria akceptacji:
    - 1. Po zalogowaniu (jeśli mam już zespoły) widzę listę wszystkich moich zespołów.
    - 2. Każdy element na liście zawiera nazwę zespołu i jest klikalny.
    - 3. Kliknięcie na nazwę zespołu przekierowuje mnie do widoku kalendarza dla tego zespołu.

### ID: US-005
- Tytuł: Edycja nazwy zespołu
- Opis: Jako Product Owner, chcę mieć możliwość zmiany nazwy istniejącego zespołu.
- Kryteria akceptacji:
    - 1. Obok nazwy każdego zespołu na liście lub w widoku szczegółów zespołu znajduje się opcja "Edytuj".
    - 2. Po jej wybraniu mogę edytować nazwę zespołu w polu tekstowym.
    - 3. Po zatwierdzeniu zmian, nowa nazwa jest widoczna na liście zespołów i we wszystkich powiązanych widokach.
    - 4. Walidacja nazwy (niepusta, limit znaków) jest taka sama jak przy tworzeniu zespołu.

### ID: US-006
- Tytuł: Usuwanie zespołu
- Opis: Jako Product Owner, chcę mieć możliwość usunięcia całego zespołu, aby pozbyć się nieaktualnych lub błędnie utworzonych danych.
- Kryteria akceptacji:
    - 1. Obok nazwy każdego zespołu na liście lub w widoku szczegółów zespołu znajduje się opcja "Usuń".
    - 2. Po kliknięciu "Usuń" pojawia się modalne okno z prośbą o potwierdzenie operacji i informacją, że jest ona nieodwracalna.
    - 3. Po potwierdzeniu, zespół i wszystkie jego dane (członkowie, role, importy) są trwale usuwane z systemu.
    - 4. Zespół znika z listy moich zespołów.

### ID: US-007
- Tytuł: Definiowanie ról w zespole
- Opis: Jako Product Owner, chcę zdefiniować listę ról dla mojego zespołu, aby móc precyzyjnie przypisywać je członkom zespołu.
- Kryteria akceptacji:
    - 1. W widoku zarządzania zespołem znajduje się sekcja "Role".
    - 2. W tej sekcji jest przycisk "Dodaj rolę", który otwiera formularz do wpisania nazwy roli.
    - 3. Po dodaniu, nowa rola pojawia się na liście ról w zespole.
    - 4. Nazwa roli nie może być pusta i musi być unikalna w obrębie zespołu.

### ID: US-008
- Tytuł: Dodawanie członka do zespołu
- Opis: Jako Product Owner, chcę dodać nowego członka do mojego zespołu, przypisując mu rolę oraz opcjonalnie daty pracy, aby uwzględnić go w planowaniu capacity.
- Kryteria akceptacji:
    - 1. W widoku zarządzania zespołem znajduje się sekcja "Członkowie zespołu" z przyciskiem "Dodaj członka".
    - 2. Formularz dodawania zawiera pola: "Imię i Nazwisko", listę rozwijaną z rolami zdefiniowanymi dla zespołu, oraz opcjonalne pola dat "Data rozpoczęcia pracy" i "Data zakończenia pracy".
    - 3. Po wypełnieniu i zatwierdzeniu formularza, nowy członek pojawia się na liście członków zespołu.
    - 4. System nie pozwala na dodanie osoby o tym samym imieniu i nazwisku, które już istnieje w tym zespole.
    - 5. Data zakończenia pracy nie może być wcześniejsza niż data rozpoczęcia.

### ID: US-009
- Tytuł: Import pliku Excel z nieobecnościami
- Opis: Jako Product Owner, chcę zaimportować plik Excel z harmonogramem nieobecności, aby zaktualizować dane o dostępności mojego zespołu.
- Kryteria akceptacji:
    - 1. W widoku zespołu znajduje się przycisk "Importuj z Excela".
    - 2. Po kliknięciu mogę wybrać plik `.xlsx` z mojego komputera.
    - 3. Po pomyślnym zaimportowaniu danych, widok kalendarza automatycznie się odświeża, pokazując zaktualizowane nieobecności.
    - 4. Po imporcie wyświetlany jest komunikat o sukcesie oraz podsumowanie z logu (np. "Import zakończony pomyślnie. Ostrzeżenia: 1").
    - 5. Jeśli import się nie powiedzie z powodu błędu krytycznego (brak miesiąca, nieznany członek zespołu), proces jest przerywany, a ja widzę szczegółowy komunikat o błędzie.
    - 6. Poprzednie dane o nieobecnościach dla tego zespołu są archiwizowane w bazie danych.

### ID: US-010
- Tytuł: Przeglądanie kalendarza nieobecności
- Opis: Jako Product Owner, chcę widzieć kalendarz nieobecności dla całego zespołu, aby mieć szybki wgląd w to, kto i kiedy jest niedostępny.
- Kryteria akceptacji:
    - 1. Główny widok zespołu to kalendarz pokazujący 6 kolejnych miesięcy.
    - 2. Wiersze reprezentują członków zespołu, a kolumny dni miesiąca.
    - 3. Komórki są pokolorowane zgodnie ze statusem: nieobecność, weekend/święto, dzień poza okresem zatrudnienia.
    - 4. Pierwsza kolumna z imionami i nazwiskami jest zawsze widoczna podczas przewijania w poziomie.
    - 5. Mogę nawigować do poprzednich lub następnych miesięcy za pomocą przycisków strzałek.

### ID: US-011
- Tytuł: Obliczanie capacity zespołu
- Opis: Jako Product Owner, chcę obliczyć dostępne capacity (w MD) dla poszczególnych ról w moim zespole w wybranym okresie, aby móc realistycznie planować pracę.
- Kryteria akceptacji:
    - 1. Pod kalendarzem znajduje się formularz z polami "Data od", "Data do" i przyciskiem "Oblicz".
    - 2. Po wybraniu dat i kliknięciu "Oblicz", poniżej formularza pojawiają się dwie tabele z wynikami.
    - 3. Pierwsza tabela pokazuje podsumowanie dla każdego członka zespołu w podanym okresie, zawierające kolumny: `Imię i Nazwisko`, `Rola`, `Planowane MD`, `Nieobecności w MD`, `Dostępne MD`.
    - 4. Druga tabela pokazuje zagregowane podsumowanie dla każdej roli, zawierające kolumny: `Rola`, `Planowane MD`, `Nieobecności w MD`, `Dostępne MD`.
    - 5. Obliczenia poprawnie uwzględniają weekendy, święta państwowe, indywidualne nieobecności oraz daty rozpoczęcia/zakończenia pracy członków zespołu.

## 6. Metryki sukcesu
- 1. Poprawność importu: 100% plików Excel zgodnych ze zdefiniowanym formatem jest importowanych bez błędów. System poprawnie identyfikuje i obsługuje wszystkie zdefiniowane przypadki brzegowe (np. brak pracownika w pliku, pracownik spoza zespołu).
- 2. Poprawność obliczeń: Wyniki obliczeń capacity (dostępne MD) są w 100% zgodne z ręcznymi obliczeniami dla zestawu testowych scenariuszy, uwzględniających różne kombinacje nieobecności, świąt, weekendów i okresów pracy.
- 3. Czas wykonania importu: Czas przetwarzania standardowego pliku Excel (12 miesięcy, 15-osobowy zespół) nie przekracza 5 sekund.
- 4. Adopcja narzędzia: Co najmniej 90% Product Ownerów, dla których narzędzie zostało stworzone, aktywnie korzysta z aplikacji (co najmniej jeden import w miesiącu).

