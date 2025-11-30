<conversation_summary>
<decisions>
1.  Format pliku Excel jest niezmienny; aplikacja musi go obsługiwać.
2.  Identyfikacja członków zespołu podczas importu odbywa się po imieniu i nazwisku. System uniemożliwia dodanie dwóch osób o tym samym imieniu i nazwisku do jednego zespołu.
3.  Brak członka zespołu (zdefiniowanego w systemie) w tabeli dla danego miesiąca w pliku Excel jest błędem. Osoba ta jest oznaczana jako nieobecna przez cały ten miesiąc, a informacja trafia do logu. Zasada dotyczy tylko tego konkretnego miesiąca.
4.  Brak tabeli dla któregokolwiek z 12 miesięcy w pliku Excel jest błędem krytycznym, który przerywa import.
5.  Znalezienie w pliku Excel wiersza z osobą, która nie jest zdefiniowana w zespole w aplikacji, przerywa import z komunikatem błędu i numerem wiersza.
6.  Widok kalendarza będzie przedstawiał 12 miesięcy ułożonych pionowo (jeden pod drugim) dla wybranego roku, z "przylepioną" pierwszą kolumną zawierającą imiona i nazwiska.
7.  Zmiana roli członka zespołu jest możliwa poprzez zakończenie jego pracy i dodanie go ponownie z nową rolą. Nie ma możliwości usunięcia członka zespołu.
8.  Możliwe jest zarządzanie zespołami i rolami: edycja nazw, usuwanie całych zespołów (co trwale usuwa dane).
9.  Wszyscy członkowie zespołu w ramach MVP mają wymiar etatu 1.0.
10. Archiwizacja danych po imporcie polega na zmianie statusu w bazie danych, bez dostępu do danych archiwalnych z poziomu interfejsu użytkownika w MVP.
    </decisions>

<matched_recommendations>
1.  Proces importu będzie generował szczegółowy log z informacjami o błędach i ostrzeżeniach (np. o osobach oznaczonych jako nieobecne z powodu braku w pliku).
2.  Pod widokiem kalendarza znajdzie się sekcja z polami wyboru daty (`Data od`, `Data do`) i przyciskiem "Oblicz", a wynik zostanie przedstawiony w tabeli `Rola` | `Dostępne Osobodni (MD)`.
3.  Po zalogowaniu użytkownik bez zdefiniowanych zespołów zobaczy stronę powitalną z wezwaniem do działania "Stwórz swój pierwszy zespół".
4.  Parser importu będzie ignorował wielkość liter w nazwach miesięcy i będzie obsługiwał zdefiniowany słownik nazw (Styczeń, Luty, itd.).
5.  Parser będzie inteligentnie ignorował kolumny z dniami nieistniejącymi w danym miesiącu (np. 31 kwietnia).
6.  System będzie walidował, czy data zakończenia pracy członka zespołu nie jest wcześniejsza niż data rozpoczęcia.
7.  Po pomyślnym imporcie widok kalendarza odświeży się automatycznie.
8.  Wprowadzona zostanie podstawowa walidacja dla nazw zespołów i ról (np. limit znaków).
9.  Brak zdefiniowanych dat rozpoczęcia/zakończenia pracy oznacza, że członek zespołu jest traktowany jako pracujący bezterminowo w ramach analizowanego okresu.
10. Parser, po znalezieniu nazwy miesiąca, zweryfikuje, czy w kolejnych wierszach znajdują się członkowie zespołu zdefiniowani w systemie, aby potwierdzić, że jest to poprawna tabela danych.
    </matched_recommendations>

<prd_planning_summary>
### Podsumowanie Planowania PRD

#### a. Główne wymagania funkcjonalne produktu
Produkt ma na celu rozwiązanie problemu z określeniem capacity zespołu deweloperskiego. Użytkownikiem jest Product Owner.

1.  **Zarządzanie Zespołami i Rolami:**
    *   Użytkownik (PO) może tworzyć zespoły.
    *   Dla każdego zespołu PO definiuje listę ról (np. "Developer Frontend", "Tester").
    *   PO dodaje członków zespołu, podając `Imię i Nazwisko`, przypisując `Rolę` oraz opcjonalnie `Datę rozpoczęcia` i `Datę zakończenia` pracy.
    *   System uniemożliwia dodanie do zespołu dwóch osób o tym samym imieniu i nazwisku.
    *   Możliwa jest edycja nazw zespołów, ról i danych członków zespołu. Nie można usuwać członków zespołu, ale można usunąć cały zespół.

2.  **Import Nieobecności z Pliku Excel:**
    *   Import odbywa się z pliku `.xlsx` o sztywnej strukturze: arkusze z latami (`2023`, `2024`), a w nich 12 tabel dla każdego miesiąca, oddzielonych pustymi wierszami.
    *   Parser skanuje plik w poszukiwaniu nazw miesięcy, aby zlokalizować tabele.
    *   Nieobecność jest identyfikowana przez dowolny kolor tła komórki inny niż biały.
    *   **Logika walidacji:** Import zostaje przerwany, jeśli: a) brakuje tabeli dla któregokolwiek miesiąca; b) w tabeli znajduje się osoba niezdefiniowana w systemie.
    *   Jeśli członek zespołu zdefiniowany w systemie nie występuje w tabeli dla danego miesiąca, jego dostępność w tym miesiącu wynosi 0 MD, a zdarzenie jest logowane.
    *   Nowy import zastępuje poprzednie dane dla danego zespołu.

3.  **Wizualizacja Danych (Kalendarz):**
    *   Główny widok to kalendarz roczny, prezentujący 12 miesięcy w układzie pionowym.
    *   Pierwsza kolumna z imionami i nazwiskami jest "przylepiona" i widoczna podczas przewijania.
    *   Komórki są kolorowane: granatowy (nieobecność), żółty (weekend lub święto), szary (dzień poza okresem pracy danej osoby).
    *   Dostępna jest nawigacja między latami.

4.  **Obliczanie Capacity:**
    *   Poniżej kalendarza znajduje się formularz do wyboru zakresu dat.
    *   Po kliknięciu "Oblicz" system wyświetla tabelaryczne podsumowanie dostępnych osobodni (MD) w podziale na role zdefiniowane w zespole.
    *   Obliczenia uwzględniają weekendy (sobota, niedziela) i święta (z bazy danych) jako dni o zerowej dostępności.

#### b. Kluczowe historie użytkownika i ścieżki korzystania
1.  **Konfiguracja Zespołu:** Jako PO, chcę stworzyć nowy zespół, zdefiniować dla niego role i dodać członków wraz z ich okresem pracy, aby przygotować system do śledzenia ich dostępności.
2.  **Import Danych:** Jako PO, chcę zaimportować plik Excel z planem nieobecności, aby zaktualizować dane o dostępności mojego zespołu w aplikacji.
3.  **Przegląd Dostępności:** Jako PO, chcę widzieć roczny kalendarz nieobecności dla całego zespołu, aby mieć szybki wgląd w zaplanowane wolne.
4.  **Planowanie Pojemności:** Jako PO, chcę obliczyć dostępne capacity (w MD) dla poszczególnych ról w moim zespole w zadanym okresie, aby móc realistycznie planować pracę.

#### c. Ważne kryteria sukcesu i sposoby ich mierzenia
1.  **Poprawność importu:** Import pliku Excel z nieobecnościami działa zgodnie ze zdefiniowaną logiką i poprawnie przetwarza dane.
2.  **Poprawność obliczeń:** Liczba dostępnych osobodni (MD) dla każdej z ról w podanym zakresie dat jest wyliczana poprawnie, uwzględniając wszystkie zdefiniowane warunki (nieobecności, weekendy, święta, okres pracy).

</prd_planning_summary>

<unresolved_issues>
Na podstawie przeprowadzonej rozmowy wszystkie kluczowe kwestie dla MVP zostały wyjaśnione i podjęto konkretne decyzje. Nie zidentyfikowano nierozwiązanych problemów wymagających dalszych wyjaśnień na tym etapie.
</unresolved_issues>
</conversation_summary>