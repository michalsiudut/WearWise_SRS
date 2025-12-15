# Specyfikacja Wymagań Oprogramowania (SRS) dla Aplikacji "WearWise"
### Wersja 1.0

---

## 1. Wstęp

### 1.1. Cel
Niniejszy dokument stanowi Specyfikację Wymagań Oprogramowania (SRS) dla aplikacji mobilnej **WearWise w wersji 1.0**. Jest on przeznaczony dla zespołu deweloperskiego, projektantów UX/UI, testerów oraz interesariuszy projektu. Jego celem jest precyzyjne zdefiniowanie funkcjonalności, cech oraz ograniczeń systemu, stanowiąc podstawę do dalszych prac projektowych, implementacyjnych i testowych.

### 1.2. Wizja, Zakres i Cele Produktu
*   **Wizja:** Być inteligentnym, osobistym stylistą w kieszeni każdego, kto chce ubierać się odpowiednio do pogody i własnego stylu, bez codziennego zastanawiania się "w co się ubrać?".
*   **Zakres:** Aplikacja "WearWise" umożliwi użytkownikom katalogowanie posiadanych ubrań poprzez dodawanie ich zdjęć i podstawowych atrybutów. Na podstawie danych pogodowych pobieranych w czasie rzeczywistym dla lokalizacji użytkownika oraz jego preferencji określonych w krótkiej ankiecie, system będzie codziennie proponował gotowy zestaw ubrań.
*   **Poza Zakresem:** Wersja 1.0 aplikacji nie będzie zawierać funkcji społecznościowych (udostępnianie stylizacji, komentowanie), e-commerce (zakup ubrań), ani zaawansowanego zarządzania szafą (np. planowanie prania).

### 1.3. Definicje, Akronimy i Skróty
*   **SRS:** Software Requirements Specification (Specyfikacja Wymagań Oprogramowania).
*   **API:** Application Programming Interface (Interfejs Programowania Aplikacji).
*   **Outfit:** Zestaw ubrań (np. koszula, spodnie, buty) proponowany przez aplikację.
*   **Supabase:** Otwartoźródłowa alternatywa dla Firebase używana jako backend (BaaS - Backend as a Service).
*   **React Native:** Framework do tworzenia aplikacji mobilnych na platformy iOS i Android.

### 1.4. Przegląd Dokumentu
Dokument ten jest podzielony na kluczowe sekcje. Rozdział 1 stanowi wprowadzenie. Rozdział 2 opisuje ogólne cechy produktu, jego użytkowników i ograniczenia. Rozdział 3 szczegółowo definiuje wymagania funkcjonalne. Rozdział 4 skupia się na atrybutach jakościowych. Rozdział 5 zawiera analizę porównawczą.

---

## 2. Opis Ogólny

### 2.1. Główne Funkcje Produktu
*   **Zarządzanie Wirtualną Szafą:** Dodawanie, kategoryzowanie i przeglądanie ubrań.
*   **Ankieta Stylu Użytkownika:** Zbieranie preferencji dotyczących stylu i komfortu.
*   **Generowanie Codziennego Outfitu:** Proponowanie zestawu ubrań na podstawie pogody i preferencji.
*   **Uwierzytelnianie Użytkownika:** Rejestracja i logowanie.

### 2.2. Klasy Użytkowników
W systemie występuje jedna główna klasa użytkownika:
*   **Użytkownik Aplikacji:** Osoba, która chce otrzymywać codzienne sugestie ubioru na podstawie posiadanej garderoby i warunków pogodowych.

### 2.3. Ograniczenia projektowe
*   **Ograniczenie Technologiczne:** Aplikacja kliencka musi być zbudowana przy użyciu **React Native**, a backend musi opierać się na platformie **Supabase**.
    *   **Źródło:** Wymagania zadania projektowego.
    *   **Wpływ na architekturę:** Narzuca konkretny stos technologiczny. Wyklucza inne frameworki mobilne (np. Flutter, Swift/Kotlin natywnie) oraz inne rozwiązania backendowe (np. customowy backend w Node.js, Firebase). Wymusza użycie PostgREST API i narzędzi dostarczanych przez Supabase do obsługi bazy danych, uwierzytelniania i przechowywania plików.

*   **Ograniczenie Zależności Zewnętrznej:** System jest w pełni zależny od zewnętrznego dostawcy API pogodowego.
    *   **Źródło:** Kluczowa funkcjonalność aplikacji.
    *   **Wpływ na architekturę:** Konieczność zaprojektowania modułu integrującego się z API pogodowym, który będzie odporny na jego ewentualne awarie lub zmiany. Należy zaimplementować mechanizmy buforowania (caching) odpowiedzi, aby zminimalizować liczbę zapytań i koszty oraz zapewnić działanie aplikacji w przypadku chwilowej niedostępności usługi.

### 2.4. Założenia projektowe
*   **Założenie Techniczne:**
    *   **Założenie:** Zakładamy, że darmowy plan wybranego API pogodowego (np. OpenWeatherMap) zapewni wystarczającą dokładność danych i limit zapytań dla początkowej fazy działania aplikacji (MVP).
    *   **Ryzyko:** Jeśli API będzie niedokładne, będzie miało zbyt niski limit zapytań lub zostanie wyłączone, główna funkcja generowania outfitów przestanie działać poprawnie, co zrujnuje doświadczenie użytkownika.
    *   **Plan walidacji:** W pierwszym tygodniu projektu deweloper przeprowadzi testy integracyjne z 2-3 dostawcami API pogodowego, aby zweryfikować dokładność danych dla kilku lokalizacji oraz sprawdzić realne limity i opóźnienia w odpowiedziach.

*   **Założenie Dotyczące Użytkownika:**
    *   **Założenie:** Zakładamy, że użytkownicy będą gotowi poświęcić czas na sfotografowanie i skatalogowanie co najmniej 15-20 sztuk swoich ubrań, aby system mógł generować sensowne propozycje.
    *   **Ryzyko:** Jeśli proces dodawania ubrań będzie zbyt uciążliwy, użytkownicy porzucą aplikację, zanim system zgromadzi wystarczająco danych, by być użytecznym. Aplikacja zostanie uznana za bezwartościową.
    *   **Plan walidacji:** Przed implementacją docelowej funkcjonalności, projektant UX stworzy klikalny prototyp (w Figmie) procesu dodawania ubrania i przeprowadzi testy użyteczności z 5 potencjalnymi użytkownikami, mierząc czas i subiektywną ocenę łatwości tego procesu.


## 3. Wymagania Funkcjonalne

### 3.1. Zarządzanie Wirtualną Szafą
*   **Opis:** Umożliwia użytkownikom dodawanie i kategoryzowanie posiadanych ubrań.
*   **Historyjka Użytkownika:** Jako użytkownik, chcę móc dodać zdjęcie mojego ubrania i przypisać je do kategorii, abym mógł zbudować cyfrową wersję mojej szafy.
*   **Cel Biznesowy:** Zgromadzenie danych o garderobie użytkownika, które są niezbędne do działania głównej funkcji aplikacji.
*   **Warunki Wstępne:** Użytkownik jest zalogowany w systemie.
*   **Warunki Końcowe:** Nowy element garderoby zostaje zapisany w systemie i jest powiązany z profilem użytkownika.
*   **Kryteria Akceptacji:**
    *   **WF-SZAFA-01: Pomyślne dodanie nowego ubrania (Scenariusz Główny)**
        *   **Given:** Jestem zalogowanym użytkownikiem na ekranie "Moja Szafa".
        *   **When:** Kliknę przycisk "Dodaj ubranie", zrobię zdjęcie, wybiorę kategorię (np. "Koszula") i typ pogody (np. "Na ciepłe dni"), a następnie zatwierdzę formularz.
        *   **Then:** Zdjęcie ubrania jest przesyłane do Supabase Storage.
        *   **And:** W bazie danych zostaje utworzony nowy rekord ubrania z linkiem do zdjęcia i przypisanymi atrybutami.
        *   **And:** Nowe ubranie pojawia się na liście w mojej wirtualnej szafie.

