<conversation_summary>

<decisions>
1. MD (Man-Days) oznacza pełny dzień roboczy, brak częściowych nieobecności w MVP
2. Excel zawiera 12 miesięcznych tabel z nazwami osób i kolorowanymi komórkami oznaczającymi nieobecność
3. Wszystkie nieobecności mają jednakowy wpływ na dostępność
4. Import przez scrum mastera/team leada przed każdym sprintem
5. Kalendarz dni wolnych tylko dla Polski
6. Minimalne dane: nazwa osoby i rola w zespole
7. Brak raportów w MVP - tylko wyświetlanie w aplikacji
8. Brak powiadomień w MVP
9. Maksymalnie 100 osób w zespole, 100 zespołów na użytkownika
10. Archiwizacja bez możliwości przeglądania w MVP
11. Ignorowanie osób z Excel nieobecnych w systemie
12. Każdy kolor w Excel oznacza nieobecność
13. Informowanie o braku danych gdy zakres wykracza poza Excel
14. Kalendarz bez filtrowania/sortowania
15. Podsumowanie dostępności całościowo dla zakresu dat
16. Walidacja dat członków zespołu vs nieobecności
17. Wymaganie dokładnie 12 miesięcy w Excel
18. Kalendarz dni wolnych zarządzany skryptem bazodanowym
19. Archiwizowane dane w formacie umożliwiającym odtworzenie
20. Precyzyjne komunikaty błędów
21. Logowanie ignorowanych osób z Excel
22. Komunikaty ostrzegawcze o braku danych
23. Wizualne odróżnienie dni wolnych
24. Podsumowanie tylko liczby MD
25. Walidacja struktury 12 miesięcy w Excel
26. Kalendarz dni wolnych: tabela z rokiem i datą
27. Brak podglądu archiwum w MVP
28. Brak ról - dostęp tylko do własnych zespołów
29. Walidacja unikalności nazw zespołów
30. Jeden kalendarz dla Polski
31. Oznaczenie nieobecnych granatowym kolorem
32. Brak metadanych importu
33. Maksymalne długości: zespół 50 znaków, członek 30 znaków
34. Możliwość edycji/usuwania zespołów i członków
35. Format daty DD-MM-RRRR
36. Brak backup/restore w MVP
37. Autoryzacja login/hasło
38. Wsparcie Chrome, Firefox, Edge
39. Generowanie logów wszystkich operacji
</decisions>

<matched_recommendations>
1. Zdefiniowanie jasnej jednostki MD i sposobu obsługi nieobecności
2. Określenie schematu importu na podstawie struktury Excel
3. Ustalenie jednolitego wpływu wszystkich typów nieobecności na dostępność
4. Zdefiniowanie procesu importu i odpowiedzialności
5. Określenie geograficznego zakresu aplikacji
6. Zdefiniowanie minimalnego zestawu danych personalnych
7. Ustalenie ograniczeń skalowania dla architektury systemu
8. Zaprojektowanie struktury archiwum dla audytu
9. Określenie strategii mapowania nazw między Excel a systemem
10. Zdefiniowanie sposobu interpretacji kolorów w Excel
11. Określenie poziomu walidacji struktury pliku
12. Zaprojektowanie struktury tabeli dla dni świątecznych
13. Ustalenie reguł nazewnictwa i walidacji
14. Zdefiniowanie konwencji wizualnych dla statusu członków
15. Określenie ograniczeń długości pól dla projektowania bazy danych
    </matched_recommendations>

<prd_planning_summary>

**Główne wymagania funkcjonalne produktu:**

System zarządzania dostępnością zespołów agile składający się z:
- Systemu kont użytkowników (scrum masterzy, team leadzi) z autoryzacją login/hasło
- Zarządzania zespołami do 100 osób z definiowanymi rolami
- Importu nieobecności z plików Excel (12 miesięczne tabele)
- Kalendarza polskich dni wolnych zarządzanego skryptami
- Widoku kalendarza zespołu z wizualnym oznaczeniem nieobecności (granatowy kolor)
- Kalkulatora dostępności w MD dla określonych zakresów dat z podziałem na role
- Walidacji dat członkostwa vs nieobecności
- Archiwizacji poprzednich importów
- Logowania wszystkich operacji użytkowników

**Kluczowe historie użytkownika:**
1. Scrum Master loguje się do systemu i widzi listę swoich zespołów
2. Tworzy nowy zespół, definiuje role i dodaje członków z okresami pracy
3. Przed sprintem importuje plik Excel z nieobecnościami
4. Sprawdza kalendarz zespołu z wizualną reprezentacją dostępności
5. Wylicza dostępność w MD dla okresu sprintu z podziałem na role
6. W trakcie sprintu dokonuje dodatkowego importu przy nieplanowanych nieobecnościach
7. Edytuje skład zespołu lub usuwa nieaktywne zespoły

**Kryteria sukcesu:**
- Precyzyjne wyliczanie liczby dostępnych MD dla każdej roli w podanym zakresie dat
- Poprawa jakości planowania sprintów poprzez lepszą widoczność capacity
- Redukcja czasu potrzebnego na określanie dostępności zespołu
- Zwiększenie dokładności planowania względem rzeczywistej realizacji

**Wymagania techniczne:**
- Aplikacja webowa dla przeglądarek Chrome, Firefox, Edge
- Format daty DD-MM-RRRR w całej aplikacji
- Maksymalne długości: nazwa zespołu 50 znaków, nazwa członka 30 znaków
- Obsługa do 100 zespołów na użytkownika
- Baza danych z archiwizacją i logowaniem operacji

</prd_planning_summary>

<unresolved_issues>
Wszystkie kluczowe kwestie zostały rozstrzygnięte w trakcie konwersacji. System MVP jest dobrze zdefiniowany z jasnymi ograniczeniami funkcjonalnymi i technicznymi. Zidentyfikowano obszary do rozwoju w przyszłych wersjach (częściowe nieobecności 0.5 MD, elastyczność formatu Excel, zarządzanie rolami użytkowników, funkcje analityczne).
</unresolved_issues>

</conversation_summary>