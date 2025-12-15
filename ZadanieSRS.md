# Specyfikacja Wymagań Oprogramowania (SRS) dla Aplikacji "WearWise"
### Wersja 1.0

---

## 1. Wstęp

### 1.1. Cel
Niniejszy dokument stanowi Specyfikację Wymagań Oprogramowania (SRS) dla aplikacji mobilnej **WearWise w wersji 1.0**. Jest on przeznaczony dla zespołu deweloperskiego, projektantów UX/UI, testerów oraz interesariuszy projektu. Jego celem jest precyzyjne zdefiniowanie funkcjonalności, cech oraz ograniczeń systemu, stanowiąc podstawę do dalszych prac projektowych, implementacyjnych i testowych.

### 1.2. Wizja, Zakres i Cele Produktu
*   **Wizja:** Być inteligentnym, osobistym stylistą w kieszeni każdego, kto chce ubierać się odpowiednio do pogody i własnego stylu, bez codziennego zastanawiania się "w co się ubrać?".
*   **Zakres:** Aplikacja WearWise pozwala na stworzenie cyfrowego inwentarza Twojej szafy. Użytkownik dodaje zdjęcia swoich ubrań i opisuje je kilkoma atrybutami. Następnie, nasz inteligentny algorytm, bazując na aktualnej prognozie pogody dla Twojej lokalizacji oraz Twoich indywidualnych preferencjach, każdego ranka serwuje gotową propozycję stylizacji.
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

### 2.2. Klasy Użytkowników i Aktorzy Systemu

Na podstawie analizy interakcji z systemem (przedstawionej w diagramie przypadków użycia) identyfikujemy następujących aktorów:

*   **Aktor Główny (Użytkownik Końcowy):**
    *   **Opis:** Jest to główna i jedyna klasa użytkownika ludzkiego w systemie. Reprezentuje osobę, która instaluje aplikację w celu rozwiązania swojego codziennego problemu z doborem stroju.
    *   **Interakcje z systemem:**
        *   **Zarządza swoim kontem:** Rejestruje się, loguje i definiuje swoje preferencje w ankiecie stylu.
        *   **Zarządza Wirtualną Szafą:** Buduje cyfrową garderobę poprzez dodawanie, przeglądanie i usuwanie swoich ubrań.
        *   **Konsumuje główną wartość:** Otrzymuje i przegląda codzienne, automatycznie generowane propozycje outfitów.

*   **Aktor Drugorzędny (System Zewnętrzny):**
    *   **Nazwa:** **API Pogodowe**
    *   **Opis:** Jest to zewnętrzna usługa (system maszynowy), która dostarcza kluczowych danych niezbędnych do działania aplikacji. Nie jest to użytkownik ludzki, ale pełni rolę aktora, ponieważ system wchodzi z nim w interakcję w celu realizacji jednego z przypadków użycia.
    *   **Interakcje z systemem:**
        *   Dostarcza aktualne dane meteorologiczne (temperatura, warunki, np. deszcz, słońce) dla określonej lokalizacji geograficznej na żądanie systemu WearWise.


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

### 3.2. Generowanie Codziennego Outfitu
*   **Opis:** System automatycznie generuje i prezentuje użytkownikowi propozycję ubioru na dany dzień.
*   **Historyjka Użytkownika:** Jako użytkownik, chcę codziennie rano otrzymywać jedną, gotową propozycję ubioru, abym nie musiał/a tracić czasu na podejmowanie decyzji.
*   **Cel Biznesowy:** Dostarczenie kluczowej wartości aplikacji, która rozwiązuje codzienny problem użytkownika i zachęca do regularnego korzystania z produktu.
*   **Warunki Wstępne:** Użytkownik jest zalogowany, ma w szafie co najmniej 5 ubrań z różnych kategorii i udzielił zgody na dostęp do lokalizacji.
*   **Warunki Końcowe:** Użytkownik widzi na ekranie głównym zestawienie ubrań (outfit) na dziś.
*   **Kryteria Akceptacji:**
    *   **WF-OUTFIT-01: Wygenerowanie propozycji dla słonecznej pogody (Scenariusz Główny)**
        *   **Given:** Jestem zalogowanym użytkownikiem, a prognoza pogody dla mojej lokalizacji to 25°C i słońce.
        *   **And:** W mojej szafie znajdują się T-shirty, krótkie spodenki i sandały oznaczone jako odpowiednie na ciepłą pogodę.
        *   **When:** Otwieram aplikację.
        *   **Then:** System pobiera aktualne dane pogodowe.
        *   **And:** System wyświetla mi zestaw składający się z jednego T-shirtu, jednych krótkich spodenek i jednych sandałów z mojej szafy.
    *   **WF-OUTFIT-02: Brak wystarczającej liczby ubrań (Scenariusz Wyjątkowy)**
        *   **Given:** Jestem nowym, zalogowanym użytkownikiem.
        *   **And:** Dodałem do mojej szafy tylko 2 T-shirty.
        *   **When:** Otwieram aplikację, oczekując propozycji outfitu.
        *   **Then:** System powinien wyświetlić komunikat: "Dodaj więcej ubrań z różnych kategorii (np. spodnie, buty), abyśmy mogli tworzyć dla Ciebie stylizacje!".
        *   **And:** Propozycja outfitu nie jest generowana.

### 3.3. Priorytetyzacja Wymagań dla MVP
| Funkcja | Korzyść | Kara | Koszt | Ryzyko | Priorytet = (K+Ka)/(Ko+R) | Kandydat do MVP? |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| Uwierzytelnianie użytkownika | 8 | 13 | 3 | 2 | **4.20** | **Tak** |
| Dodawanie ubrań do szafy | 13 | 21 | 8 | 5 | **2.61** | **Tak** |
| Generowanie outfitu | 21 | 13 | 13 | 8 | **1.62** | **Tak** |
| Ankieta preferencji stylu | 8 | 5 | 5 | 3 | **1.62** | **Tak** |
| Edycja/Usuwanie ubrań | 5 | 8 | 3 | 2 | **2.60** | **Tak** |
| Udostępnianie stylizacji | 3 | 1 | 8 | 5 | **0.31** | Nie |

**Uzasadnienie:** Do MVP wybrano funkcje o najwyższym priorytecie, które tworzą kompletny, minimalny cykl życia użytkownika: rejestracja, dodanie ubrań i otrzymanie propozycji. Bez którejkolwiek z nich aplikacja jest bezużyteczna.

---

## 4. Atrybuty Jakościowe

### 4.1. Priorytetyzacja Atrybutów
1.  **Wydajność:** Kluczowa dla pozytywnego pierwszego wrażenia. Użytkownik oczekuje natychmiastowej propozycji ubioru po otwarciu aplikacji.
2.  **Dostępność:** Aplikacja musi być dostępna każdego ranka, gdy użytkownik się ubiera. Awaria w tym momencie podważa sens istnienia produktu.
3.  **Modyfikowalność:** Ważna w kontekście przyszłego rozwoju. Architektura musi pozwalać na łatwe dodawanie nowych funkcji (np. integracja z e-commerce, funkcje społecznościowe) bez przepisywania całości.
4.  **Bezpieczeństwo:** Standardowy wymóg, ale mniej krytyczny niż w aplikacjach finansowych. Obejmuje ochronę danych logowania i wizerunku (zdjęcia ubrań).

## 5. Odkrywanie i Analiza Wymagań

### 5.1. Analiza Porównawcza

**Krok 1: Identyfikacja Konkurencji/Wzorców**
*   **Konkurencja bezpośrednia:** Aplikacje takie jak Cladwell, Stylebook, Acloset.
*   **Konkurencja pośrednia:** Pinterest (inspiracje modowe), aplikacje pogodowe z "indeksem ubioru".
*   **Wzorce funkcjonalne:** Aplikacje do katalogowania (np. książek, filmów) pod kątem łatwości dodawania nowych pozycji.

**Krok 2: Tabela Porównawcza**
| Kryterium | Cladwell | Stylebook | Acloset | **WearWise (plan)** |
| :--- | :--- | :--- | :--- | :--- |
| **Model Biznesowy** | Subskrypcja | Płatność jednorazowa | Freemium (funkcje premium) | **Darmowa (MVP)** |
| **Automatyczne usuwanie tła** | Tak | Tak (narzędzia) | Tak (automatyczne) | **Nie (MVP)** |
| **Generowanie outfitów** | Tak, automatyczne | Tak, manualne i auto | Tak, automatyczne | **Tak, automatyczne** |
| **Aspekt społecznościowy** | Nie | Tak (udostępnianie) | Tak (mocno rozbudowany) | **Nie (MVP)** |
| **Integracja z pogodą** | Tak | Tak | Tak | **Tak (kluczowa funkcja)** |
| **Łatwość użycia (UX)** | Wysoka, minimalistyczny | Średnia, dużo funkcji | Wysoka, nowoczesny UI | **Priorytet: Maksymalna prostota** |

**Krok 3: Synteza Wyników i Wnioski**
*   **Co konkurencja robi dobrze?** Automatyzacja (usuwanie tła, generowanie stylizacji) jest standardem. Integracja z pogodą jest obecna, ale często jako jedna z wielu funkcji.
*   **Gdzie są ich słabe punkty?** Wiele aplikacji jest przeładowanych funkcjami (planowanie podróży, statystyki noszenia), co może przytłaczać nowego użytkownika. Modele subskrypcyjne lub płatne z góry stanowią barierę wejścia.
*   **Wpływ na WearWise:** Naszą szansą jest **skupienie się na jednej rzeczy i robienie jej doskonale**: codziennej, automatycznej propozycji outfitu opartej na pogodzie. Rezygnujemy z zaawansowanych funkcji zarządzania szafą i aspektów społecznościowych w MVP, aby zaoferować maksymalnie prosty i darmowy produkt, który rozwiązuje jeden, konkretny problem. To nasza główna przewaga konkurencyjna.

---

## Dodatki

### Dodatek A: Modele Analityczne

#### Diagram Przypadków Użycia

### Dodatek B: Persony Użytkowników

#### Persona 1: "Zajęty Profesjonalista"
*   **Imię:** Jakub
*   **Wiek:** 32 lata
*   **Zawód:** Programista
*   **Cele:** Chce dobrze wyglądać w pracy, ale nienawidzi tracić czasu na poranne decyzje dotyczące ubioru. Chce mieć pewność, że jego strój jest adekwatny do pogody, zwłaszcza gdy dojeżdża do biura rowerem.
*   **Frustracje:** Często ubiera się nieodpowiednio do pogody (za ciepło/za zimno). Jego szafa jest pełna ubrań, ale rano ma wrażenie, że "nie ma się w co ubrać".

#### Persona 2: "Świadoma Studentka"
*   **Imię:** Zofia
*   **Wiek:** 21 lat
*   **Zawód:** Studentka psychologii
*   **Cele:** Chce w pełni wykorzystywać ubrania, które już posiada, aby ograniczyć konsumpcjonizm. Lubi eksperymentować ze stylem, ale potrzebuje inspiracji, jak łączyć swoje ubrania w nowe zestawy.
*   **Frustracje:** Zapomina o ubraniach, które ma na dnie szafy. Wydaje pieniądze na nowe rzeczy, mimo że ma już podobne.

### Dodatek C: Kwestie do Rozwiązania
1.  Jaki będzie dokładny koszt korzystania z API pogodowego po przekroczeniu darmowego progu? (Do zbadania)
2.  Jak dokładnie zdefiniować "styl" użytkownika w ankiecie, aby był on zrozumiały i użyteczny dla algorytmu? (Wymaga prototypowania i testów UX)
3.  W jaki sposób aplikacja ma obsługiwać ubrania uniwersalne, np. jeansy, które pasują zarówno na chłodne, jak i cieplejsze dni? (Wymaga decyzji projektowej)
4.  Czy zdjęcia ubrań będą przechowywane prywatnie, czy Supabase domyślnie udostępnia je publicznie przez URL? (Kwestia bezpieczeństwa do weryfikacji)
