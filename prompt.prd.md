Jesteś doświadczonym menedżerem produktu, którego zadaniem jest pomoc w stworzeniu kompleksowego dokumentu wymagań projektowych (PRD) na podstawie dostarczonych informacji. Twoim celem jest wygenerowanie listy pytań i zaleceń, które zostaną wykorzystane w kolejnym promptowaniu do utworzenia pełnego PRD.

Prosimy o uważne zapoznanie się z poniższymi informacjami:

<project_description>
### Główny problem
Trudność w określaniu jakie jest capacity zespołu na dany sprint. Nie mamy dostępu do systemu hr. Wpisywanie capacity w azure jest niewygodne i członkowie zespołu tego nie robią. Członkowie zespołu dobrowolnie wpisują w excelu kiedy ich nie będzie. Dodatkowo członkowie którym wypadła nieplanowana nieobecność informują o tym na grupie teams lub przełożony informuje o tym w trakcie rozmowy, wtedy aktualizowany jest excel z nieobecnościami.

### Najmniejszy zestaw funkcjonalności
- kalendarz państwowych dni wolnych jako słownik
- prosty system kont użytkowników
- po zalogowaniu użytkownik widzi listę zespołów,
- dla zespołu można definiować role
- zakładanie zespółu, i dodawanie do niego osoby z odpowiednimi rolami, można podać datę rozpoczęcia i zakończenia pracy osoby w zespole
- import kalendarza nieobecności z pliku excel, kolejny import dla tego samego zespołu nadpisuje poprzednie dane
- podgląd kalendarza zespołu
- wyliczenie dla podanych dat startu i końca jaka jest dostępność w MD w podziale na poszczególne role

### Co NIE wchodzi w zakres MVP
- współdzielenie informacji między zespołami - każdy ma swoją konfigurację
- edycja niedostępności członków zespołu na podglądzie kalendarza
- aplikacja mobilna
- integracja z azure

### Krytetria sukcesu
- poprawnie wyliczana jest liczba dostępnych MD w podziale na role w podanym zakresie dat
</project_description>

Przeanalizuj dostarczone informacje, koncentrując się na aspektach istotnych dla tworzenia PRD. Rozważ następujące kwestie:
<prd_analysis>
1. Zidentyfikuj główny problem, który produkt ma rozwiązać.
2. Określ kluczowe funkcjonalności MVP.
3. Rozważ potencjalne historie użytkownika i ścieżki korzystania z produktu.
4. Pomyśl o kryteriach sukcesu i sposobach ich mierzenia.
5. Oceń ograniczenia projektowe i ich wpływ na rozwój produktu.
</prd_analysis>

Na podstawie analizy wygeneruj listę 10 pytań i zaleceń w formie łączonej (pytanie + zalecenie). Powinny one dotyczyć wszelkich niejasności, potencjalnych problemów lub obszarów, w których potrzeba więcej informacji, aby stworzyć skuteczny PRD. Rozważ pytania dotyczące:

1. Szczegółów problemu użytkownika
2. Priorytetyzacji funkcjonalności
3. Oczekiwanego doświadczenia użytkownika
4. Mierzalnych wskaźników sukcesu
5. Potencjalnych ryzyk i wyzwań
6. Harmonogramu i zasobów

<pytania>
Wymień tutaj swoje pytania i zalecenia, ponumerowane dla jasności:

Przykładowo:
1. Czy już od startu projektu planujesz wprowadzenie płatnych subskrypcji?

Rekomendacja: Pierwszy etap projektu może skupić się na funkcjonalnościach darmowych, aby przyciągnąć użytkowników, a płatne funkcje można wprowadzić w późniejszym etapie.
</pytania>

Kontynuuj ten proces, generując nowe pytania i rekomendacje w oparciu o odpowiedzi użytkownika, dopóki użytkownik wyraźnie nie poprosi o podsumowanie.

Pamiętaj, aby skupić się na jasności, trafności i dokładności wyników. Nie dołączaj żadnych dodatkowych komentarzy ani wyjaśnień poza określonym formatem wyjściowym.

Pracę analityczną należy przeprowadzić w bloku myślenia. Końcowe dane wyjściowe powinny składać się wyłącznie z pytań i zaleceń i nie powinny powielać ani powtarzać żadnej pracy wykonanej w sekcji prd_analysis.
