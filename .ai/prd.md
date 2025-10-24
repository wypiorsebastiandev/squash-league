# Dokument wymagań produktu (PRD) - SquashLeague (MVP)

## 1. Przegląd produktu

SquashLeague to aplikacja webowa typu PWA (Progressive Web App) stworzona w celu digitalizacji i uproszczenia procesu rejestrowania wyników w amatorskiej lidze squasha.  
Obecnie wyniki są zapisywane ręcznie na papierze i przesyłane zdjęciem do koordynatora ligi. Aplikacja ma zastąpić ten proces, zapewniając pełną cyfryzację i przejrzystość danych.

Technologia:  
Frontend: Astro + React + TypeScript + Tailwind + shadcn/ui  
Backend: Supabase (PostgreSQL, Auth)  
CI/CD: GitHub Actions  
Hosting: DigitalOcean  
Dostępność: PWA działająca offline, responsywna dla urządzeń mobilnych i desktopowych

Cel MVP:  
Udostępnić aplikację, w której wszyscy zawodnicy ligi mogą rejestrować wyniki meczów online, a administrator zarządza sezonami, rundami, grupami i punktacją.

---

## 2. Problem użytkownika

Obecny proces zarządzania wynikami w lidze squasha jest manualny, czasochłonny i podatny na błędy:

- wyniki przekazywane są w formie zdjęć papierowych kartek,
- koordynator musi ręcznie przepisywać i liczyć punkty,
- brak natychmiastowej aktualizacji tabel i historii meczów.

Skutki:

- utrata czasu na weryfikację wyników,
- brak transparentności między zawodnikami,
- ryzyko błędów w punktacji i terminach.

Rozwiązanie:

- Aplikacja umożliwia każdemu zawodnikowi wprowadzenie wyniku meczu bezpośrednio po grze.
- Dane są natychmiast walidowane, przeliczane i udostępniane zawodnikom.
- Administrator zarządza sezonem, grupami i ewentualnymi korektami w jednym miejscu.

---

## 3. Wymagania funkcjonalne

### 3.1 Role użytkowników

- Zawodnik: może logować się, przeglądać swoje mecze, wprowadzać i edytować wyniki, otrzymywać powiadomienia.
- Administrator: zarządza sezonami, rundami, grupami, zawodnikami, walkowerami i historią wyników. Może również grać w lidze.

### 3.2 Autentykacja i autoryzacja

- Rejestracja tylko poprzez zaproszenie od administratora.
- Wymagana weryfikacja e-mail i numeru telefonu.
- Dane użytkowników (imię, nazwisko, e-mail, telefon) zgodne z RODO.
- Uwierzytelnianie realizowane przez Supabase Auth (JWT).

### 3.3 Zarządzanie sezonami i rundami

- Administrator tworzy sezon z datą rozpoczęcia i zakończenia.
- W sezonie można tworzyć wiele rund.
- Po rozpoczęciu rundy edycja grup i przypisań zawodników jest zablokowana.
- Rundy są zamykane ręcznie przez administratora.

### 3.4 Zarządzanie grupami i zawodnikami

- Administrator przypisuje zawodników do grup.
- System automatycznie generuje pary meczowe w grupach „każdy z każdym”.
- Maksymalnie 100 zawodników w jednej lidze.

### 3.5 Mecze i rejestrowanie wyników

- Każdy mecz składa się z 5 setów; set do 11 punktów, z wymogiem dwóch punktów przewagi.
- Zawodnik rejestruje wynik meczu, wpisując liczbę punktów w każdym secie.
- Wynik widoczny dla obu zawodników natychmiast po zapisie.
- Brak konieczności potwierdzenia wyniku przez przeciwnika.
- Wynik można edytować do 2 dni po meczu lub do końca rundy.
- System waliduje poprawność formatu i liczby setów.
- Każda zmiana wyniku jest zapisywana w historii (audit trail).

### 3.6 Punktacja

- Punkty za mecz przyznawane są według skali:
  - 3:0 – 10 pkt
  - 3:1 – 8 pkt
  - 3:2 – 6 pkt
  - 2:3 – 4 pkt
  - 1:3 – 2 pkt
  - Dodatkowo: +1 pkt za rozegranie meczu, +2 pkt za wygraną.
- Walkower: +10 pkt dla zwycięzcy, –2 pkt dla przegrywającego.
- Punkty w tabeli przeliczane są automatycznie po każdym meczu.

### 3.7 Powiadomienia

- Kanały: e-mail, web push, SMS.
- Zdarzenia:
  - dodanie/edycja wyniku,
  - rozpoczęcie rundy,
  - przypomnienie o końcu rundy (48h przed terminem).
- System wysyła powiadomienia z ograniczeniem częstotliwości (1 powiadomienie/24h/osoba).

### 3.8 PWA i działanie offline

- Aplikacja działa offline dla:
  - widoku „Moje mecze”,
  - formularza wprowadzenia wyniku.
- Synchronizacja danych po odzyskaniu połączenia.
- Konflikty synchronizacji rozwiązywane przez wyświetlenie różnic (ostatnia wersja wygrywa).

### 3.9 Panel administracyjny

- Funkcje:
  - zarządzanie sezonami, rundami, grupami, zawodnikami,
  - ręczne nadawanie walkowerów,
  - podgląd i edycja wyników,
  - dostęp do historii zmian wyników,
  - możliwość ponownego przeliczenia punktacji (recalc).
- Wsparcie wielu administratorów; logowanie działań w audycie.

### 3.10 Analityka

- Dashboard z podstawowymi wskaźnikami:
  - liczba rozegranych meczów,
  - liczba walkowerów,
  - średni czas raportowania wyników,
  - procent aktywnych użytkowników.

---

## 4. Granice produktu

### W zakresie MVP:

- Rejestracja i logowanie użytkowników (na zaproszenie)
- Definiowanie sezonów, rund i grup
- Automatyczne generowanie par meczowych
- Rejestrowanie i edycja wyników
- Automatyczna punktacja i tabela wyników
- Powiadomienia e-mail/SMS/web push
- Offline PWA
- Dashboard administracyjny i audit trail

### Poza zakresem MVP:

- Automatyczne awanse i spadki między rundami
- Integracje z zewnętrznymi kalendarzami lub płatnościami
- Komentarze lub funkcje społecznościowe
- Publiczny widok tabeli
- Eksport danych do CSV
- Integracja z systemami rezerwacji kortów

---

## 5. Historyjki użytkowników

### US-001 – Rejestracja użytkownika po zaproszeniu

Opis: Jako zawodnik chcę zarejestrować się po otrzymaniu zaproszenia od admina, aby uzyskać dostęp do ligi.  
Kryteria akceptacji:

- Otrzymuję e-mail z linkiem rejestracyjnym.
- Po kliknięciu ustawiam hasło, akceptuję regulamin i RODO.
- Wymagana jest weryfikacja e-mail oraz numeru telefonu.

### US-002 – Logowanie użytkownika

Opis: Jako zawodnik chcę logować się do aplikacji, aby przeglądać swoje mecze i wyniki.  
Kryteria akceptacji:

- Logowanie przy użyciu e-maila i hasła.
- Sesja utrzymywana przez token Supabase.
- Po zalogowaniu użytkownik trafia na ekran „Moje mecze”.

### US-003 – Utworzenie sezonu przez admina

Opis: Jako administrator chcę utworzyć nowy sezon, aby rozpocząć ligę.  
Kryteria akceptacji:

- Formularz z datą rozpoczęcia i zakończenia sezonu.
- Po utworzeniu mogę dodać rundy.

### US-004 – Tworzenie rundy

Opis: Jako administrator chcę dodać rundę w ramach sezonu, aby organizować rozgrywki etapami.  
Kryteria akceptacji:

- Runda ma daty startu i końca.
- Po rozpoczęciu rundy przypisania graczy są zablokowane.

### US-005 – Przypisanie zawodników do grup

Opis: Jako admin chcę przypisać zawodników do grup, aby ustalić strukturę rozgrywek.  
Kryteria akceptacji:

- Lista zawodników dostępna w panelu.
- Możliwość przypisania zawodnika do grupy.
- System generuje pary meczowe „każdy z każdym”.

### US-006 – Rejestracja wyniku meczu

Opis: Jako zawodnik chcę wpisać wynik meczu, aby został on zapisany w systemie.  
Kryteria akceptacji:

- Formularz z 5 setami i liczbą punktów.
- Walidacja: dokładnie 5 setów, wynik do 11 z przewagą 2.
- Po zapisaniu wysyłane powiadomienie do przeciwnika.

### US-007 – Edycja wyniku meczu

Opis: Jako zawodnik lub admin chcę móc edytować wynik meczu w określonym czasie, aby poprawić błędy.  
Kryteria akceptacji:

- Edycja do 48h od meczu lub do końca rundy.
- Każda zmiana zapisywana w historii audytu.

### US-008 – Nadanie walkowera

Opis: Jako admin chcę nadać walkower, aby odnotować brak rozegranego meczu.  
Kryteria akceptacji:

- Formularz „Nadaj WO” z wyborem powodu.
- System automatycznie przypisuje punkty (+10, –2).
- Zdarzenie widoczne w audycie.

### US-009 – Powiadomienia o zmianach

Opis: Jako zawodnik chcę otrzymywać powiadomienia o zmianach w moich meczach.  
Kryteria akceptacji:

- Powiadomienia e-mail, web push lub SMS.
- Użytkownik może wybrać preferowany kanał.

### US-010 – Tryb offline

Opis: Jako zawodnik chcę móc wprowadzać wynik nawet bez internetu.  
Kryteria akceptacji:

- Formularz dostępny offline.
- Dane zapisują się lokalnie.
- Po odzyskaniu połączenia wynik synchronizuje się automatycznie.

### US-011 – Przegląd tabeli punktowej

Opis: Jako zawodnik chcę zobaczyć aktualną tabelę grupy, aby znać swoją pozycję.  
Kryteria akceptacji:

- Tabela aktualizuje się automatycznie po każdym meczu.
- Widok tylko dla zawodników ligi.

### US-012 – Analityka dla admina

Opis: Jako admin chcę widzieć wskaźniki aktywności, aby monitorować przebieg ligi.  
Kryteria akceptacji:

- Dashboard pokazuje liczbę rozegranych meczów, WO, średni czas raportowania, aktywność użytkowników.

---

## 6. Metryki sukcesu

1. 100% rozegranych meczów zarejestrowanych w aplikacji.
2. <5% meczów edytowanych po zapisie.
3. ≥90% aktywnych użytkowników tygodniowo.
4. Średni czas od zakończenia meczu do rejestracji <6h.
5. 99,5% uptime podczas rund.
6. ≥80% pozytywnych opinii zawodników w ankiecie po sezonie pilotażowym.

---
