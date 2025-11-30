1.  **Pytanie:** Jaki jest dokładny, oczekiwany format pliku Excel z nieobecnościami i czy możemy zdefiniować stały szablon, aby zminimalizować błędy importu?
    **Zalecenie:** Zalecam stworzenie i udokumentowanie sztywnego formatu pliku Excel, a nawet udostępnienie użytkownikom gotowego szablonu do pobrania w aplikacji, co zapewni spójność danych i uprości proces importu.

2.  **Pytanie:** W jaki sposób mają być archiwizowane poprzednie dane o nieobecnościach po nowym imporcie i czy użytkownik (Product Owner) powinien mieć do nich dostęp?
    **Zalecenie:** Proponuję, aby archiwizacja oznaczała zapisanie historycznej wersji pliku w systemie z możliwością jej pobrania przez użytkownika, co pozwoli na audyt i analizę historycznych zmian w planach.

3.  **Pytanie:** Jakie konkretne informacje powinien zawierać "podgląd kalendarza zespołu" i w jakiej formie (np. widok miesięczny, lista)?
    **Zalecenie:** Sugeruję, aby kalendarz w MVP był prostym widokiem miesięcznym, który wizualnie zaznacza dni nieobecności dla poszczególnych członków zespołu, z opcją filtrowania po rolach.

4.  **Pytanie:** Jak system ma obsługiwać nieobecności częściowe (np. pół dnia) lub różne typy nieobecności (np. urlop, chorobowe), jeśli plik Excel zawiera takie rozróżnienie?
    **Zalecenie:** W ramach MVP zalecam uproszczenie i traktowanie każdej nieobecności jako całego dnia niedostępności, aby nie komplikować logiki obliczeń. Obsługę nieobecności częściowych można dodać w przyszłych iteracjach.

5.  **Pytanie:** W jaki sposób będzie zarządzany "słownik państwowych dni wolnych"? Czy będzie on aktualizowany manualnie przez administratora systemu, czy powinien być zaciągany z zewnętrznego źródła dla danego kraju?
    **Zalecenie:** Rekomenduję, aby na etapie MVP kalendarz świąt był stałą, konfigurowalną listą w systemie, zarządzaną przez administratora. Umożliwi to elastyczność bez konieczności integracji z zewnętrznymi API.

6.  **Pytanie:** Czy data rozpoczęcia i zakończenia pracy osoby w zespole ma automatycznie ograniczać jej dostępność w kalendarzu i obliczeniach capacity?
    **Zalecenie:** Tak, zalecam, aby daty pracy członka zespołu były bezwzględnie uwzględniane przy obliczaniu dostępności. Osoba nie powinna generować capacity poza zdefiniowanym okresem zatrudnienia w zespole.

7.  **Pytanie:** Poza poprawnością importu i kalkulacji, jakie inne mierzalne wskaźniki sukcesu (KPI) możemy zdefiniować, np. dotyczące czasu oszczędzonego przez PO?
    **Zalecenie:** Proponuję dodać jako kryterium sukcesu skrócenie czasu potrzebnego na wyliczenie capacity zespołu na dany okres z np. 30 minut do 5 minut. Można to zweryfikować poprzez ankiety lub wywiady z użytkownikami.

8.  **Pytanie:** Jakie są oczekiwania co do walidacji danych podczas importu pliku Excel (np. co jeśli w pliku pojawi się osoba, której nie ma w zespole)?
    **Zalecenie:** Zalecam, aby proces importu generował szczegółowy raport z błędami (np. "Nie znaleziono użytkownika X", "Nieprawidłowy format daty w wierszu Y"), ale pozwalał na zaimportowanie poprawnych danych, ignorując błędne wpisy.

9.  **Pytanie:** Czy role w zespole są predefiniowane dla całej aplikacji, czy każdy Product Owner tworzy własną, unikalną listę ról dla każdego ze swoich zespołów?
    **Zalecenie:** Sugeruję, aby każdy PO mógł definiować własny, unikalny zestaw ról dla każdego zespołu. Zapewni to większą elastyczność i dokładniejsze odzwierciedlenie struktury różnych zespołów.

10. **Pytanie:** Jak system powinien interpretować weekendy przy obliczaniu dostępności w MD? Czy zawsze są one dniami wolnymi?
    **Zalecenie:** Rekomenduję wprowadzenie prostego ustawienia na poziomie zespołu, które definiuje dni robocze (domyślnie poniedziałek-piątek). Pozwoli to na prawidłowe obliczenia capacity i obsługę niestandardowych harmonogramów pracy.

---

1. Excel będzie zawierał dane dotyczące kolejnych lat w zakładkach o nazwach 2023, 2024 itd. W każdej zakładce będzie 12 tabelek z miesiącami. Tabele oddzielone sa pustymi wierszami (różna ilość pustych wierszy). Pierwszy wiersz tabelki zawiera nazwe miesiąca po polsku. Pierwsza kolumna jest nieistotna. W drugiej sa imiona i nazwiska członków zespołu. Kolejne kolumny to dni miesiąca (1..31). Nieobecności sa oznaczone w komurkach dowolnym kolorem. Przykładowy plik to [nieobecności.xlsx](../../nieobecno%C5%9Bci.xlsx).
2. Archiwizacja polega na oznaczeniu w bazie wierszy statusem 'A' - archived. Na MVP nie będzie dostępu do tych danych z aplikacji.
3. Widok ma pokazywać bierzący rok z możliwością nawigacji do poprzednich lat. W bierzącym roku powinniśmy widzieć wszystkie miesiące od stycznia do grudnia w formie siatki. Wiersze to członkowie zespołu, kolumny to dni miesiąca. Nieobecności zaznaczone są kolorem granatowym tła komórki. Soboty, Niedziele oraz dni wolne maja być oznaczone kolorem żółtym.
4. W MVP nie obsługujemy nieobecności częściowych. Każda nieobecność oznacza cały dzień niedostępności.
5. Słownik dni wolnych będzie zarządzany skryptem bazodanowym i będzie dotyczył tylko Polski.
6. Tak, daty te mają ograniczać dostępność członka zespołu. Komórki dla dat kiedy dany czlonek zespołu jaszcze nie rozpoczoł lub juz zakończył prace powinny byc oznaczone kolorem szarym. 
7. Nie trzeba dodatkowych KPI.
8. Podczas importu powstanie log z ignorowanymi osobami (nie ma ich w systemie). Import powinien się zakończyć sukcesem nawet jeśli niektóre osoby zostaną zignorowane.
9. Każdy PO tworzy własną liste ról dla swojego zespołu.
10. Weekend to zawsze sobota i niedziela. Te dni oraz dni wolne ze słownika oznaczają capacity 0.

---

Na podstawie dostarczonych odpowiedzi, oto kolejna lista pytań i zaleceń mających na celu dalsze uszczegółowienie wymagań.

1.  **Pytanie:** Jak system ma identyfikować członków zespołu podczas importu z Excela, aby poprawnie przypisać nieobecności (np. po imieniu i nazwisku, adresie e-mail)? Co w przypadku literówek lub różnych form zapisu tego samego imienia i nazwiska?
    **Zalecenie:** Zalecam wprowadzenie unikalnego identyfikatora dla każdego członka zespołu (np. służbowy adres e-mail) podczas dodawania go do zespołu w aplikacji. Ten sam identyfikator powinien być używany w pliku Excel, co wyeliminuje problemy z dopasowywaniem po imieniu i nazwisku.

2.  **Pytanie:** Struktura pliku Excel z 12 tabelami w jednym arkuszu, oddzielonymi zmienną liczbą pustych wierszy, jest podatna na błędy parsowania. Czy możemy uprościć ten format?
    **Zalecenie:** Sugeruję zdefiniowanie formatu, w którym każdy miesiąc znajduje się w osobnym, nazwanym arkuszu (np. "Styczeń", "Luty" itd.) lub, jeszcze prościej, zdefiniowanie jednej tabeli na cały rok z kolumnami: `Identyfikator_Użytkownika`, `Data_Nieobecności`. Uprości to znacznie logikę importu i zmniejszy ryzyko błędów.

3.  **Pytanie:** W jaki sposób w interfejsie użytkownika będzie realizowana funkcja "wyliczania dostępności w MD dla podanych dat"? Gdzie użytkownik wprowadzi daty i jak zostanie zaprezentowany wynik?
    **Zalecenie:** Proponuję umieszczenie pod kalendarzem dedykowanej sekcji z dwoma polami wyboru daty (`Data od`, `Data do`) oraz przyciskiem "Oblicz". Wynik powinien być wyświetlony w prostej tabeli: `Rola` | `Dostępne Osobodni (MD)`.

4.  **Pytanie:** Jak system powinien obsłużyć sytuację, gdy członek zespołu zmienia rolę w trakcie okresu, dla którego liczone jest capacity? Czy jedna osoba może mieć tylko jedną rolę przez cały czas pracy w zespole?
    **Zalecenie:** Dla MVP zalecam uproszczenie, w którym każdy członek zespołu ma przypisaną tylko jedną rolę na cały okres pracy w zespole. Możliwość zmiany ról w czasie można dodać w przyszłości.

5.  **Pytanie:** Roczny widok kalendarza w formie siatki (członkowie w wierszach, dni w kolumnach) może być bardzo szeroki i nieczytelny na standardowych ekranach. Jakie są oczekiwania co do nawigacji i responsywności tego widoku?
    **Zalecenie:** Sugeruję, aby domyślnie widok pokazywał jeden miesiąc (bieżący) z możliwością łatwej nawigacji do kolejnych/poprzednich miesięcy. Widok roczny mógłby być opcjonalnym, uproszczonym widokiem "z lotu ptaka", bez podziału na poszczególne dni.

6.  **Pytanie:** W jaki sposób system ma identyfikować, że komórka w Excelu jest pokolorowana, skoro "dowolny kolor" oznacza nieobecność? Czy chodzi o dowolny kolor tła inny niż biały?
    **Zalecenie:** Należy precyzyjnie zdefiniować, co oznacza "pokolorowana komórka". Rekomenduję ustalenie, że każda komórka, której tło nie jest puste/białe, oznacza nieobecność. Zapewni to jednoznaczną logikę dla parsera.

7.  **Pytanie:** Czy Product Owner powinien mieć możliwość ręcznego dodania/edycji pojedynczej nieobecności bezpośrednio w aplikacji, czy jedyną drogą jest re-import całego pliku Excel?
    **Zalecenie:** Chociaż edycja w kalendarzu została wykluczona z MVP, warto rozważyć dodanie prostej funkcji "Dodaj pojedynczą nieobecność" (wymagającej podania członka zespołu i daty). Pozwoli to na szybkie nanoszenie drobnych korekt bez konieczności modyfikacji i ponownego importu pliku Excel.

8.  **Pytanie:** Jak system ma się zachować, jeśli importowany plik Excel będzie zawierał zduplikowane wpisy dla tej samej osoby w tym samym miesiącu (np. dwie tabelki dla stycznia w jednym arkuszu)?
    **Zalecenie:** Proponuję, aby w przypadku znalezienia duplikatu dla danego miesiąca, importer przetworzył dane z pierwszej napotkanej tabeli i zignorował kolejne, dodając odpowiedni wpis w logu importu.

9.  **Pytanie:** Czy przy definiowaniu członka zespołu w systemie (przed importem) powinniśmy określić jego indywidualny wymiar czasu pracy (np. 1.0 dla pełnego etatu, 0.5 dla połowy)?
    **Zalecenie:** Tak, zalecam dodanie pola "Wymiar etatu" (domyślnie 1.0) dla każdego członka zespołu. Umożliwi to w przyszłości dokładniejsze obliczanie capacity dla osób pracujących w niepełnym wymiarze godzin, a w MVP zapewni, że każdy dzień roboczy liczy się jako 1 MD.

10. **Pytanie:** Co powinien zobaczyć użytkownik po zalogowaniu, jeśli nie ma jeszcze zdefiniowanego żadnego zespołu?
    **Zalecenie:** Po pierwszym zalogowaniu lub gdy użytkownik nie ma żadnych zespołów, powinien zobaczyć stronę powitalną z wyraźnym przyciskiem/wezwaniem do działania "Stwórz swój pierwszy zespół", co ułatwi rozpoczęcie pracy z aplikacją.

---

1. Członków zespołu identyfikujemy po imieniu i nazwisku. W przypadku literówek osoba nie zostanie znaleziona i zostanie zignorowana podczas importu.
2. Niestety nie możemy zmienić formatu pliku Excel.
3. Tak, dokładnie tak jak opisano w zaleceniu.
4. Zgoda, jedna rola na cały okres pracy w zespole. Przy czym zmanę roli mozna dokonać poprzez zakończenie pracy danej osoby w zespole i dodanie jej ponownie z nową rolą.
5. Ma byc widać wszystkie 12 miesięcy na raz. Domyślnie ma być widoczny bieżący rok. Nawigacja do poprzednich lat ma być poprzez przyciski przewijania lat. 
6. Tak, każda komórka z tłem innym niż biały oznacza nieobecność.
7. Nie, jedyną drogą jest re-import pliku Excel.
8. Zgoda z zaleceniem.
9. Na MVP wszyscy członkowie zespołu mają wymiar etatu 1.0.
10. Zgoda z zaleceniem.

---

Na podstawie dostarczonych odpowiedzi, oto kolejna lista pytań i zaleceń mających na celu dalsze uszczegółowienie wymagań.

1.  **Pytanie:** Skoro identyfikacja odbywa się po imieniu i nazwisku, jak system ma się zachować, gdy w zespole zdefiniowanym w aplikacji jest dwóch członków o tym samym imieniu i nazwisku?
    **Zalecenie:** Zalecam, aby system uniemożliwiał dodanie do jednego zespołu dwóch osób o identycznym imieniu i nazwisku. Powinno to być walidowane na poziomie interfejsu użytkownika, aby zapobiec niejednoznaczności podczas importu.

2.  **Pytanie:** W jaki sposób parser ma niezawodnie zlokalizować 12 tabel z miesiącami w arkuszu Excel, skoro są one oddzielone zmienną i nieokreśloną liczbą pustych wierszy?
    **Zalecenie:** Proponuję, aby logika importu skanowała arkusz wiersz po wierszu w poszukiwaniu predefiniowanych polskich nazw miesięcy (np. "Styczeń", "Luty"). Znalezienie takiej nazwy w pierwszej kolumnie tabeli będzie sygnałem rozpoczęcia parsowania danych dla danego miesiąca.

3.  **Pytanie:** Widok 12 miesięcy jednocześnie (ok. 365 dni w kolumnach) będzie wymagał znacznego przewijania w poziomie. Jakie jest oczekiwane zachowanie nagłówków (imiona i nazwiska członków zespołu) podczas tego przewijania?
    **Zalecenie:** Sugeruję zaimplementowanie "przylepionej" pierwszej kolumny z imionami i nazwiskami. Dzięki temu nazwisko członka zespołu pozostanie widoczne na ekranie podczas przewijania kalendarza w poziomie, co znacznie poprawi czytelność.

4.  **Pytanie:** Co w sytuacji, gdy członek zespołu zdefiniowany w aplikacji nie występuje w importowanym pliku Excel na liście osób dla danego miesiąca? Czy oznacza to jego 100% dostępność w tym miesiącu?
    **Zalecenie:** Tak, zalecam przyjęcie zasady, że brak osoby w pliku Excel dla danego okresu oznacza jej pełną dostępność (brak zaplanowanych nieobecności), pod warunkiem, że mieści się to w jej dacie zatrudnienia w zespole.

5.  **Pytanie:** Jak system ma interpretować nazwy miesięcy w pliku Excel? Czy musimy obsługiwać odmianę przez przypadki (np. "Styczeń" vs "Stycznia") lub wielkość liter?
    **Zalecenie:** Rekomenduję, aby parser ignorował wielkość liter i był przygotowany na obsługę podstawowych form gramatycznych nazw miesięcy (mianownik, np. "Styczeń"). Należy stworzyć i udokumentować listę akceptowalnych nazw dla każdego miesiąca.

6.  **Pytanie:** Czy w widoku kalendarza powinna istnieć wizualna różnica między weekendem a dniem wolnym od pracy (świętem), skoro oba są oznaczone na żółto i mają zerowe capacity?
    **Zalecenie:** Dla uproszczenia MVP proponuję, aby nie było wizualnego rozróżnienia – oba typy dni będą traktowane jako dni wolne od pracy i oznaczane tym samym kolorem. Rozróżnienie można wprowadzić w przyszłości, jeśli pojawi się taka potrzeba.

7.  **Pytanie:** W jaki sposób użytkownik (PO) dodaje członków zespołu do systemu przed importem? Jakie dane oprócz imienia i nazwiska są wymagane?
    **Zalecenie:** Proponuję stworzenie prostego formularza "Dodaj członka zespołu", który będzie wymagał podania `Imienia i Nazwiska`, `Roli w zespole` oraz opcjonalnie `Daty rozpoczęcia` i `Daty zakończenia` pracy.

8.  **Pytanie:** Czy po imporcie pliku użytkownik powinien otrzymać podsumowanie, np. "Zaimportowano dane dla 10 z 12 członków zespołu. Zignorowano 2 osoby (Jan Kowalski, Anna Nowak) z powodu braku w systemie"?
    **Zalecenie:** Tak, zdecydowanie zalecam wyświetlanie szczegółowego logu/podsumowania po każdej operacji importu. Zwiększy to transparentność procesu i pozwoli użytkownikowi szybko zidentyfikować ewentualne problemy z danymi.

9.  **Pytanie:** Jak system ma się zachować, jeśli w pliku Excel, w kolumnach dni (1-31), pojawią się dni nieistniejące w danym miesiącu (np. 31 kwietnia)?
    **Zalecenie:** Parser powinien inteligentnie ignorować kolumny z dniami, które nie występują w danym miesiącu i roku (np. 30 lutego lub 31 kwietnia), aby uniknąć błędów.

10. **Pytanie:** Czy przy tworzeniu zespołu lub dodawaniu ról powinny istnieć jakieś ograniczenia, np. co do długości nazwy, użycia znaków specjalnych?
    **Zalecenie:** Rekomenduję wprowadzenie podstawowej walidacji dla nazw zespołów i ról (np. ograniczenie długości do 50 znaków, zakaz używania pewnych znaków specjalnych), aby zapewnić spójność danych i uniknąć potencjalnych problemów z wyświetlaniem.
---
1. System będzie uniemożliwiał dodanie do jednego zespołu dwóch osób o identycznym imieniu i nazwisku.
2. Parser będzie skanował arkusz wiersz po wierszu w poszukiwaniu predefiniowanych polskich nazw miesięcy w pierwszych 40 kolumnach.
3. Każdy kolejny miesiąc będzie wyświetlany ponizej poprzedniego. Pierwsza kolumna z imionami i nazwiskami będzie przylepiona. Dzięki temu unikniemy przewijania w poziomie.
4. Brak osoby to błąd. Osoba musi być obecna w pliku Excel aby była brana pod uwagę w kalkulacji dostępności. Powinna być oznaczona jako nieobecna cały miesiąc a w logu importu powinna zostać umieszczona informacja o braku osoby. 
5. Parser będzie ignorował wielkość liter i będzie przygotowany na obsługę nazw miesięcy ze słownika: Styczeń, Luty, Marzec, Kwiecień, Maj, Czerwiec, Lipiec, Sierpień, Wrzesień, Październik, Listopad, Grudzień.
6. Zgoda z zaleceniem.
7. PO będzie dodawał nowy zespół poprzez formularz z polami: nazwa zespołu, dodaj rolę (nazwa roli). Następnie będzie dodawał członków zespołu poprzez formularz z polami: imię i nazwisko, wybór roli z listy ról zespołu, data rozpoczęcia pracy(niewymagane), data zakończenia pracy (niewymagane).
8. Zgoda z zaleceniem.
9. Zgoda z zaleceniem.
10. Zgoda z zaleceniem.
---
Na podstawie dostarczonych odpowiedzi, oto kolejna lista pytań i zaleceń mających na celu dalsze uszczegółowienie wymagań.

1.  **Pytanie:** Zgodnie z pkt. 4, jeśli członek zespołu zdefiniowany w systemie nie zostanie znaleziony w tabeli dla danego miesiąca w pliku Excel, jest oznaczany jako nieobecny przez cały ten miesiąc. Czy ta zasada dotyczy tylko tego konkretnego miesiąca, a w kolejnych miesiącach (jeśli jest obecny w pliku) jego dostępność jest liczona normalnie?
    **Zalecenie:** Proponuję, aby w przypadku braku osoby w tabeli danego miesiąca, system przyjmował dla niej 0 MD dostępności w tym miesiącu i dodawał wpis do logu. W pozostałych miesiącach, w których osoba występuje w pliku, jej dostępność powinna być obliczana standardowo na podstawie danych z Excela.

2.  **Pytanie:** Jak system ma się zachować, jeśli w importowanym pliku Excel w ogóle nie ma tabeli dla któregoś z miesięcy (np. brakuje tabeli "Marzec")?
    **Zalecenie:** Rekomenduję, aby w takiej sytuacji import był kontynuowany, a dla brakującego miesiąca wszyscy członkowie zespołu byli traktowani tak, jakby nie zostali znalezieni w pliku (zgodnie z logiką z pkt. 4) – czyli mieli 0 MD dostępności w tym miesiącu, z odpowiednią adnotacją w logu importu.

3.  **Pytanie:** W jaki sposób system ma się upewnić, że znaleziona nazwa miesiąca (np. "Luty") jest faktycznie nagłówkiem tabeli, a nie przypadkowym tekstem w innej komórce?
    **Zalecenie:** Sugeruję, aby parser po znalezieniu nazwy miesiąca weryfikował, czy w kolumnie poniżej znajdują się imiona i nazwiska, które pasują do członków zespołu zdefiniowanych w systemie. Dopiero po takim potwierdzeniu powinien rozpocząć przetwarzanie tabeli.

4.  **Pytanie:** Daty rozpoczęcia i zakończenia pracy członka zespołu są opcjonalne. Jak system ma interpretować dostępność osoby, jeśli te daty nie zostaną podane?
    **Zalecenie:** Proponuję przyjąć, że brak daty rozpoczęcia oznacza, że osoba jest dostępna od "zawsze" (w kontekście przeglądanych lat), a brak daty zakończenia oznacza, że jest dostępna "bezterminowo".

5.  **Pytanie:** Czy Product Owner powinien mieć możliwość edycji lub usunięcia zespołu, roli lub członka zespołu po ich utworzeniu?
    **Zalecenie:** Zalecam dodanie podstawowych funkcji zarządzania: edycji nazwy zespołu, edycji nazwy roli, edycji danych członka zespołu (imię, nazwisko, rola, daty) oraz możliwości ich usuwania. Usunięcie zespołu powinno wiązać się z usunięciem wszystkich jego danych.

6.  **Pytanie:** Co się stanie, jeśli w pliku Excel w drugiej kolumnie (imię i nazwisko) znajdzie się wartość, która nie jest członkiem zespołu (np. pusty wiersz, suma, dodatkowy opis)?
    **Zalecenie:** Parser powinien po prostu ignorować takie wiersze i nie umieszczać ich w logu błędów, aby nie zaśmiecać go wpisami, które nie są próbą importu danych dla konkretnej osoby.

7.  **Pytanie:** W jaki sposób w interfejsie będzie wyglądać dodawanie ról do zespołu? Czy będzie to pole tekstowe do wpisania nazwy i przycisk "Dodaj", który dodaje rolę do listy poniżej?
    **Zalecenie:** Tak, proponuję prosty mechanizm: pole tekstowe na nazwę roli i przycisk "Dodaj rolę". Po dodaniu, nazwa roli powinna pojawić się na liście ról zdefiniowanych dla danego zespołu, z opcją jej usunięcia.

8.  **Pytanie:** Czy system powinien w jakikolwiek sposób walidować format dat wpisywanych w formularzu dodawania członka zespołu (np. czy data zakończenia nie jest wcześniejsza niż data rozpoczęcia)?
    **Zalecenie:** Tak, zalecam wprowadzenie walidacji, która uniemożliwi zapisanie członka zespołu, jeśli jego data zakończenia pracy jest wcześniejsza niż data rozpoczęcia.

9.  **Pytanie:** Czy widok kalendarza powinien w jakiś sposób odświeżać się automatycznie po zakończeniu importu nowego pliku Excel?
    **Zalecenie:** Tak, po pomyślnym zakończeniu importu i zamknięciu okna z logiem, widok kalendarza powinien automatycznie odświeżyć się, aby odzwierciedlić nowe dane o nieobecnościach.

10. **Pytanie:** Czy w widoku kalendarza, oprócz kolorowania komórek, powinny być widoczne nagłówki z nazwami miesięcy i numerami dni?
    **Zalecenie:** Tak, dla czytelności widoku konieczne jest wyświetlanie nagłówków. Proponuję, aby nad każdą miesięczną siatką znajdowała się nazwa miesiąca, a nad kolumnami numery dni (1-31).
---
1. Tak, zasada dotyczy tylko konkretnego miesiąca.
2. Taka sytuacja to błąd i import powinien się zakończyć niepowodzeniem z odpowiednim komunikatem.
3. Zgoda z zaleceniem.
4. Zgoda z zaleceniem.
5. Można usunąć zespół - wtedy dane zostaną trwale usunięte. Można edytować nazwę zespołu, nazwę roli oraz dane członka zespołu. Można dodawać nowych członków zespołu. Nie można usuwać członków zespołu.
6. Import powinien zostac przerwany z komunikatem błędu i numerem wiersza gdzie nastąpił problem.
7. Zgoda z zaleceniem.
8. Zgoda z zaleceniem.
9. Zgoda z zaleceniem.
10. Zgoda z zaleceniem. Tylko nazwa ma być nad całą siatką miesięcy a numery dni nad kolumnami dni.
---

Jesteś asystentem AI, którego zadaniem jest podsumowanie rozmowy na temat planowania PRD (Product Requirements Document) dla MVP i przygotowanie zwięzłego podsumowania dla następnego etapu rozwoju. W historii konwersacji znajdziesz następujące informacje:
1. Opis projektu
2. Zidentyfikowany problem użytkownika
3. Historia rozmów zawierająca pytania i odpowiedzi
4. Zalecenia dotyczące zawartości PRD

Twoim zadaniem jest:
1. Podsumować historię konwersacji, koncentrując się na wszystkich decyzjach związanych z planowaniem PRD.
2. Dopasowanie zaleceń modelu do odpowiedzi udzielonych w historii konwersacji. Zidentyfikuj, które zalecenia są istotne w oparciu o dyskusję.
3. Przygotuj szczegółowe podsumowanie rozmowy, które obejmuje:
   a. Główne wymagania funkcjonalne produktu
   b. Kluczowe historie użytkownika i ścieżki korzystania
   c. Ważne kryteria sukcesu i sposoby ich mierzenia
   d. Wszelkie nierozwiązane kwestie lub obszary wymagające dalszego wyjaśnienia
4. Sformatuj wyniki w następujący sposób:

<conversation_summary>
<decisions>
[Wymień decyzje podjęte przez użytkownika, ponumerowane].
</decisions>

<matched_recommendations>
[Lista najistotniejszych zaleceń dopasowanych do rozmowy, ponumerowanych]
</matched_recommendations>

<prd_planning_summary>
[Podaj szczegółowe podsumowanie rozmowy, w tym elementy wymienione w kroku 3].
</prd_planning_summary>

<unresolved_issues>
[Wymień wszelkie nierozwiązane kwestie lub obszary wymagające dalszych wyjaśnień, jeśli takie istnieją]
</unresolved_issues>
</conversation_summary>

Końcowy wynik powinien zawierać tylko treść w formacie markdown. Upewnij się, że Twoje podsumowanie jest jasne, zwięzłe i zapewnia cenne informacje dla następnego etapu tworzenia PRD.
