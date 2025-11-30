Jesteś doświadczonym menedżerem produktu, którego zadaniem jest stworzenie kompleksowego dokumentu wymagań produktu (PRD) w oparciu o poniższe opisy:

<project_description>
### Główny problem
Trudność w określaniu jakie jest capacity zespołu na dany okres czasu w przyszłości i jak sie ono zmienia w wyniku nieprzewidzianych nieobecnosci (po tym okresie). Zmiana ma wpływ na ilość zrealizowanych zadań. Nie mamy dostępu do systemu hr. Wpisywanie capacity w azure jest niewygodne i członkowie zespołu tego nie robią. Członkowie zespołu dobrowolnie wpisują w excelu kiedy ich nie będzie. Dodatkowo członkowie którym wypadła nieplanowana nieobecność informują o tym na grupie teams lub przełożony informuje o tym w trakcie rozmowy, wtedy aktualizowany jest excel z nieobecnościami.

### Najmniejszy zestaw funkcjonalności
- kalendarz państwowych dni wolnych jako słownik
- prosty system kont użytkowników, użytkownikami będa osoby w roli product owner, pozostali czlonkowie zespołu nie będą się logowali do aplikacji
- po zalogowaniu użytkownik widzi listę swoich zespołów,
- dla zespołu można definiować lise ról (np. developer frontend, developer backend, tester, analityk)
- zakładanie zespółu, i dodawanie do niego osoby z odpowiednimi rolami, można podać datę rozpoczęcia i zakończenia pracy osoby w zespole
- import kalendarza nieobecności z pliku excel, kolejny import dla tego samego zespołu zastępuje poprzednie dane, poprzednie dane powinny zostać zarchiwizowane, dla kazdego importu zapisywana jest data danych
- podgląd kalendarza zespołu
- pod podglądem można wyliczyć dla podanych dat startu i końca jaka jest dostępność w MD w podziale na poszczególne role

### Co NIE wchodzi w zakres MVP
- współdzielenie informacji między zespołami - każdy ma swoją konfigurację
- edycja niedostępności członków zespołu na podglądzie kalendarza
- aplikacja mobilna
- integracja z azure
- wyznaczanie różnicy między capacity planowanym a faktycznym

### Krytetria sukcesu
- import pliku excel z nieobecnościami działa poprawnie
- poprawnie wyliczana jest liczba dostępnych MD dla każdej z roli w podanym zakresie dat
</project_description>

<project_details>
<conversation_summary>
<decisions>
1.  Format pliku Excel jest niezmienny; aplikacja musi go obsługiwać.
2.  Identyfikacja członków zespołu podczas importu odbywa się po imieniu i nazwisku. System uniemożliwia dodanie dwóch osób o tym samym imieniu i nazwisku do jednego zespołu.
3.  Brak członka zespołu (zdefiniowanego w systemie) w tabeli dla danego miesiąca w pliku Excel jest błędem. Osoba ta jest oznaczana jako nieobecna przez cały ten miesiąc, a informacja trafia do logu. Zasada dotyczy tylko tego konkretnego miesiąca.
4.  Brak tabeli dla któregokolwiek z 12 miesięcy w pliku Excel jest błędem krytycznym, który przerywa import.
5.  Znalezienie w pliku Excel wiersza z osobą, która nie jest zdefiniowana w zespole w aplikacji, przerywa import z komunikatem błędu i numerem wiersza.
6.  Widok kalendarza będzie przedstawiał 6 miesięcy ułożonych pionowo (jeden pod drugim) od miesiąca poprzedzającego aktualny, z "przylepioną" pierwszą kolumną zawierającą imiona i nazwiska.
7.  Zmiana roli członka zespołu jest możliwa poprzez zakończenie jego pracy i dodanie go ponownie z nową rolą. Nie ma możliwości usunięcia członka zespołu.
8.  Możliwe jest zarządzanie zespołami i rolami: edycja nazw, usuwanie całych zespołów (co trwale usuwa dane).
9.  Wszyscy członkowie zespołu w ramach MVP mają wymiar etatu 1.0.
10. Archiwizacja danych po imporcie polega na zmianie statusu w bazie danych, bez dostępu do danych archiwalnych z poziomu interfejsu użytkownika w MVP.
11. `Nieobecności w MD` nie uwzględnia nieobecności w sobotę, niedzielę i święta, ponieważ te dni i tak nie są liczone do dostępności.
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
    *   Użytkownicy zostaną dodani skryptem bazodanowym.
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
    *   Główny widok to kalendarz, prezentujący 6 miesięcy w układzie pionowym.
    *   Początkowo prezentowany jest miesiąc poprzedzający obecny, wraz z kolejnymi 5 miesiącami.
    *   Pierwszy wiersz zawiera kolejne dni miesiąca.
    *   Pierwsza kolumna z imionami i nazwiskami jest "przylepiona" i widoczna podczas przewijania.
    *   Komórki są kolorowane: granatowy (nieobecność), żółty (weekend lub święto), szary (dzień poza okresem pracy danej osoby).
    *   Osoby posortowane są według roli.
    *   Poniżej oraz powyżej widoku kalendarza dostępna jest nawigacja w postaci strzałek, która przesuwa widok o miesiąc do przodu lub do tyłu.

4.  **Obliczanie Capacity:**
    *   Poniżej kalendarza znajduje się formularz do wyboru zakresu dat.
    *   Po kliknięciu "Oblicz" system wyświetla tabelaryczne podsumowanie dla kazdego członka zespołu: liczbę dni w miesiącu bez sobót, niedziel i świąt jako `Planowane MD`, `Nieobecności w MD` i `Dostępne MD` .
    *   Następnie podsumowanie jest grupowane według ról, pokazując łączne `Planowane MD`, `Nieobecności w MD` i `Dostępne MD` dla każdej roli.
    *   Obliczenia uwzględniają weekendy (sobota, niedziela) i święta (z bazy danych) jako dni o zerowej dostępności.

#### b. Kluczowe historie użytkownika i ścieżki korzystania
1.  **Konfiguracja Zespołu:** Jako PO, chcę stworzyć nowy zespół, zdefiniować dla niego role i dodać członków wraz z ich okresem pracy, aby przygotować system do śledzenia ich dostępności.
2.  **Import Danych:** Jako PO, chcę zaimportować plik Excel z planem nieobecności, aby zaktualizować dane o dostępności mojego zespołu w aplikacji.
3.  **Przegląd Dostępności:** Jako PO, chcę widzieć kalendarz nieobecności dla całego zespołu, aby mieć szybki wgląd w zaplanowane wolne.
4.  **Planowanie Pojemności:** Jako PO, chcę obliczyć dostępne capacity (w MD) dla poszczególnych ról w moim zespole w zadanym okresie, aby móc realistycznie planować pracę.

#### c. Ważne kryteria sukcesu i sposoby ich mierzenia
1.  **Poprawność importu:** Import pliku Excel z nieobecnościami działa zgodnie ze zdefiniowaną logiką i poprawnie przetwarza dane.
2.  **Poprawność obliczeń:** Liczba dostępnych osobodni (MD) dla każdej z ról w podanym zakresie dat jest wyliczana poprawnie, uwzględniając wszystkie zdefiniowane warunki (nieobecności, weekendy, święta, okres pracy).

</prd_planning_summary>

<unresolved_issues>
Na podstawie przeprowadzonej rozmowy wszystkie kluczowe kwestie dla MVP zostały wyjaśnione i podjęto konkretne decyzje. Nie zidentyfikowano nierozwiązanych problemów wymagających dalszych wyjaśnień na tym etapie.
</unresolved_issues>
</conversation_summary>
</project_details>

Wykonaj następujące kroki, aby stworzyć kompleksowy i dobrze zorganizowany dokument:

1. Podziel PRD na następujące sekcje:
   a. Przegląd projektu
   b. Problem użytkownika
   c. Wymagania funkcjonalne
   d. Granice projektu
   e. Historie użytkownika
   f. Metryki sukcesu

2. W każdej sekcji należy podać szczegółowe i istotne informacje w oparciu o opis projektu i odpowiedzi na pytania wyjaśniające. Upewnij się, że:
   - Używasz jasnego i zwięzłego języka
   - W razie potrzeby podajesz konkretne szczegóły i dane
   - Zachowujesz spójność w całym dokumencie
   - Odnosisz się do wszystkich punktów wymienionych w każdej sekcji

3. Podczas tworzenia historyjek użytkownika i kryteriów akceptacji
   - Wymień WSZYSTKIE niezbędne historyjki użytkownika, w tym scenariusze podstawowe, alternatywne i skrajne.
   - Przypisz unikalny identyfikator wymagań (np. US-001) do każdej historyjki użytkownika w celu bezpośredniej identyfikowalności.
   - Uwzględnij co najmniej jedną historię użytkownika specjalnie dla bezpiecznego dostępu lub uwierzytelniania, jeśli aplikacja wymaga identyfikacji użytkownika lub ograniczeń dostępu.
   - Upewnij się, że żadna potencjalna interakcja użytkownika nie została pominięta.
   - Upewnij się, że każda historia użytkownika jest testowalna.

Użyj następującej struktury dla każdej historii użytkownika:
- ID
- Tytuł
- Opis
- Kryteria akceptacji

4. Po ukończeniu PRD przejrzyj go pod kątem tej listy kontrolnej:
   - Czy każdą historię użytkownika można przetestować?
   - Czy kryteria akceptacji są jasne i konkretne?
   - Czy mamy wystarczająco dużo historyjek użytkownika, aby zbudować w pełni funkcjonalną aplikację?
   - Czy uwzględniliśmy wymagania dotyczące uwierzytelniania i autoryzacji (jeśli dotyczy)?

5. Formatowanie PRD:
   - Zachowaj spójne formatowanie i numerację.
   - Nie używaj pogrubionego formatowania w markdown ( ** ).
   - Wymień WSZYSTKIE historyjki użytkownika.
   - Sformatuj PRD w poprawnym markdown.

Przygotuj PRD z następującą strukturą:

```markdown
# Dokument wymagań produktu (PRD) - {{app-name}}
## 1. Przegląd produktu
## 2. Problem użytkownika
## 3. Wymagania funkcjonalne
## 4. Granice produktu
## 5. Historyjki użytkowników
## 6. Metryki sukcesu
```

Pamiętaj, aby wypełnić każdą sekcję szczegółowymi, istotnymi informacjami w oparciu o opis projektu i nasze pytania wyjaśniające. Upewnij się, że PRD jest wyczerpujący, jasny i zawiera wszystkie istotne informacje potrzebne do dalszej pracy nad produktem.

Ostateczny wynik powinien składać się wyłącznie z PRD zgodnego ze wskazanym formatem w markdown, który zapiszesz w pliku .ai/prd.md
