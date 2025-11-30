Na podstawie opisu MVP z @prd.md oraz stacku technologicznego z @tech-stack.md przygotuj prompt dla generatora proof of concept, który pozwoli nam zweryfikować podstawową funkcjonalność aplikacji czyli wczytywanie nieobecności z pliku excel a następnie prezentowanie ich w formie kalendarza. Wyklucz wszystkie nadmiarowe funkcje. Zaznacz, aby generator rozplanował pracę i uzyskał moją akceptację zanim przejdzie do tworzenia PoC.
***


# Prompt dla Generatora Proof of Concept - Planning Helper

## Cel PoC
Zweryfikuj podstawową funkcjonalność aplikacji Planning Helper: importowanie nieobecności z pliku Excel i prezentowanie ich w formie kalendarza.

## Zakres funkcjonalny PoC (tylko niezbędne minimum)

### 1. Import z Excel
- Parser pliku `.xlsx` zgodny z formatem: arkusze nazwane rokiem, 12 tabel miesięcznych
- Identyfikacja nieobecności na podstawie koloru tła komórki (innego niż biały)
- Identyfikacja członków zespołu po imieniu i nazwisku
- Podstawowa walidacja: obecność wszystkich 12 miesięcy

### 2. Kalendarz nieobecności
- Wyświetlenie 6 miesięcy w układzie pionowym
- Kolorowanie komórek: granatowy (nieobecność), żółty (weekend/święto), biały (dostępny)
- Pierwsza kolumna z imionami "przyklejona" podczas przewijania poziomego
- Podstawowa nawigacja między miesiącami

### 3. Uproszczone zarządzanie danymi
- Tymczasowa lista członków zespołu (bez bazy danych)
- Możliwość wczytania przykładowego pliku Excel

## Wykluczenia z PoC
- System uwierzytelniania i autoryzacji
- Zarządzanie zespołami, rolami i członkami
- Obliczanie capacity (MD)
- Baza danych (użyj pamięci tymczasowej)
- Zaawansowana walidacja importu
- Edycja danych po imporcie
- Responsywność mobilna

## Stos technologiczny
[UWAGA: Uzupełnij na podstawie tech-stack.md]

## Wymagania przed implementacją
**WAŻNE: Przed rozpoczęciem kodowania przedstaw szczegółowy plan implementacji zawierający:**
1. Strukturę komponentów aplikacji
2. Sposób parsowania pliku Excel
3. Model danych w pamięci
4. Architekturę widoku kalendarza
5. Przykładowy format pliku Excel do testów

**Uzyskaj moją akceptację planu przed przejściem do implementacji.**

## Kryteria sukcesu PoC
- Poprawne wczytanie pliku Excel z nieobecnościami
- Wyświetlenie kalendarza z kolorowaniem statusów
- Możliwość nawigacji między miesiącami
- Stabilne działanie bez błędów krytycznych
