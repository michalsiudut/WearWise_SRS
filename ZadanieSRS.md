# Specyfikacja Wymagań Oprogramowania (SRS) dla Aplikacji "WearWise"

---

## 1. Wstęp

### 1.1. Cel
Niniejszy dokument stanowi Specyfikację Wymagań Oprogramowania (SRS) dla aplikacji mobilnej **WearWise**. Jest on przeznaczony dla zespołu deweloperskiego, projektantów UX/UI, testerów oraz interesariuszy projektu. Jego celem jest precyzyjne zdefiniowanie funkcjonalności, cech oraz ograniczeń systemu, stanowiąc podstawę do dalszych prac projektowych, implementacyjnych i testowych.

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

### 4.2. Mierzalna specyfikacja (dla TOP 3 atrybutów)

#### Scenariusz 1: Wydajność (Generowanie Outfitu)
| Element | Opis |
| :--- | :--- |
| **Źródło bodźca** | Użytkownik końcowy. |
| **Bodziec** | Otwarcie aplikacji na ekranie głównym. |
| **Artefakt** | Cały system (aplikacja mobilna, backend, API pogodowe). |
| **Środowisko** | Normalne działanie, użytkownik ma połączenie z internetem. |
| **Reakcja** | System pobiera pogodę, analizuje szafę użytkownika i wyświetla gotowy outfit. |
| **Miara reakcji** | Czas od otwarcia aplikacji do pełnego wyświetlenia propozycji outfitu jest **krótszy niż 2 sekundy dla 95% przypadków**. |

#### Scenariusz 2: Dostępność (Poranna Gotowość)
| Element | Opis |
| :--- | :--- |
| **Źródło bodźca** | Wewnętrzny system monitorujący. |
| **Bodziec** | Awaria pojedynczej instancji serwera aplikacyjnego lub bazy danych. |
| **Artefakt** | Infrastruktura backendowa na Supabase. |
| **Środowisko** | Godziny szczytu porannego (6:00 - 9:00 AM). |
| **Reakcja** | System powinien kontynuować działanie bez widocznej przerwy dla użytkownika. |
| **Miara reakcji** | Dostępność usługi generowania outfitów wynosi **99.5%** w skali miesiąca. |

#### Scenariusz 3: Modyfikowalność (Dodanie Nowego Atrybutu Ubrania)
| Element | Opis |
| :--- | :--- |
| **Źródło bodźca** | Deweloper. |
| **Bodziec** | Konieczność dodania nowego atrybutu do ubrań, np. "formalność" (codzienne, biznesowe). |
| **Artefakt** | Baza danych, backend (logika generowania outfitu), aplikacja mobilna (formularz dodawania). |
| **Środowisko** | Środowisko deweloperskie. |
| **Reakcja** | Deweloper modyfikuje schemat bazy, logikę i interfejs użytkownika. |
| **Miara reakcji** | Czas pracy potrzebny na zaimplementowanie i wdrożenie tej zmiany **nie przekracza 4 godzin roboczych**. |


### 4.3. Analiza Kompromisów Architektonicznych
*   **Cel (Wydajność):** Osiągnięcie czasu odpowiedzi < 2s.
*   **Możliwe rozwiązanie:** Pre-generowanie outfitów w nocy dla wszystkich użytkowników i przechowywanie ich w cache.
*   **Kompromis:**
    *   **Pozytywny:** Drastycznie poprawia **wydajność** odczuwalną przez użytkownika rano.
    *   **Negatywny:**
        *   Zwiększa **koszt operacyjny** (dodatkowe zasoby obliczeniowe w nocy).
        *   Pogarsza **elastyczność** - jeśli użytkownik doda nowe ubranie rano, nie zostanie ono uwzględnione w outficie na ten dzień, chyba że zaimplementuje się skomplikowaną logikę re-walidacji.


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

![UML Diagram](uml.png)

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

## Dodatek D: Szczegółowa Logika Algorytmu Rekomendacji "WearWise"

### D.1. Wstęp
Niniejszy dodatek definiuje techniczną logikę działania silnika rekomendacji w aplikacji WearWise. Algorytm jest systemem deterministycznym, opartym na regułach (Rule-Based System) oraz funkcji kosztu (Cost Function), mającym na celu dobranie optymalnego zestawu ubrań poprzez minimalizację "punktów karnych" za niedopasowanie termiczne, pogodowe i stylistyczne.

### D.2. Model Danych Wejściowych
Algorytm przetwarza dwa strumienie danych:

1.  **Szafa Użytkownika (`CLOSET`)**: Lista obiektów, gdzie każdy przedmiot posiada atrybuty:
    * `id`: Unikalny identyfikator.
    * `type`: Kategoria (top, bottom, shoes).
    * `layer`: Warstwa (1 - baza, 2 - środek, 3 - wierzch) – dotyczy tylko kategorii 'top'.
    * `warmth`: Indeks ciepła (skala 1-10).
    * `rain_index`: Odporność na wodę (skala 0-10, gdzie 0 = przemakalne, 10 = wodoodporne).
    * `tags`: Zbiór tagów (np. `["casual", "gym", "elegant"]`).
    * `color`: Kolor dominujący (np. `"black"`, `"navy"`).

2.  **Kontekst (`UserRequest`)**:
    * `temperature`: Temperatura powietrza (°C).
    * `wind_speed`: Prędkość wiatru (km/h).
    * `weather_code`: Kod pogody WMO (definiuje opady).
    * `occasion`: Kontekst wyjścia (`"casual"`, `"work"`, `"date"`, `"gym"`).
    * `mood_score`: Preferencja stylu (1-5).

### D.3. Krok 1: Fizyka Pogody (Pre-processing)
System normalizuje surowe dane pogodowe do wartości odczuwalnych przez człowieka.

* **Obliczanie Temperatury Odczuwalnej ($T_{real}$):**
    Korekta uwzględniająca czynnik chłodzący wiatru (*Wind Chill*). Stosowana, gdy $T_{air} < 20^\circ C$.
    $$T_{real} = T_{air} - (Wiatr \times 0.6)$$
* **Normalizacja Opadów:**
    Kody WMO są mapowane na skalę intensywności deszczu (0-30). Wartość $>5$ aktywuje tryb deszczowy.

### D.4. Krok 2: Filtracja Krytyczna (Hard Constraints)
Zanim system przystąpi do doboru zestawu, eliminuje z puli dostępnych ubrań te, które naruszają krytyczne zasady bezpieczeństwa lub kontekstu.

1.  **Filtr Wodny:**
    * *Warunek:* Jeśli Intensywność Deszczu $> 5$.
    * *Akcja:* Odrzuć przedmioty z `rain_index < 5` (np. sandały, zamsz, jasne płótno).
2.  **Filtr Termiczny (Zima):**
    * *Warunek:* Jeśli $T_{real} < 10^\circ C$.
    * *Akcja:* Odrzuć przedmioty o `warmth < 3` (np. szorty, cienkie spodnie).
    * *Wyjątek:* T-shirty (`layer: 1`) są dozwolone jako bielizna termiczna/podkoszulek.
3.  **Filtr Termiczny (Upał):**
    * *Warunek:* Jeśli $T_{real} > 25^\circ C$.
    * *Akcja:* Odrzuć przedmioty o `warmth > 4` (np. kurtki zimowe, ocieplane buty).
4.  **Filtr Okazji (Context Filter):**
    * *Warunek:* Zdefiniowana okazja inna niż "casual".
    * *Akcja:*
        * Dla `occasion="date"`: Odrzuć tagi `sport`, `gym`.
        * Dla `occasion="gym"`: Wymagaj tagu `sport` lub `gym`.

### D.5. Krok 3: Strategia Warstw (Dynamic Layering)
Algorytm dynamicznie ustala szablon zestawu (sloty do wypełnienia) w zależności od $T_{real}$.

| Warunki Termiczne | Struktura Zestawu (Sloty) | Cel Ciepła (Target Warmth) |
| :--- | :--- | :--- |
| **Lato** ($T_{real} \ge 22^\circ C$) | `[Top L1] + [Bottom] + [Shoes]` | **4 - 5 pkt** |
| **Przejściowa** ($10^\circ C \le T_{real} < 22^\circ C$) | `[Top L1] + [Top L2] + [Bottom] + [Shoes]` | **10 - 15 pkt** |
| **Zima** ($T_{real} < 10^\circ C$) | `[Top L1] + [Top L2] + [Top L3] + [Bottom] + [Shoes]` | **25+ pkt** |

### D.6. Krok 4: Funkcja Kosztu i Selekcja (Scoring)
System generuje możliwe kombinacje ubrań pasujące do wyznaczonych slotów. Każda kombinacja jest oceniana funkcją karną. Wygrywa zestaw z wynikiem najbliższym `0`.

**Wzór Funkcji Kosztu:**
$$Score = (0.6 \cdot \Delta Warm) + (0.5 \cdot \Delta Rain) + (0.5 \cdot StylePenalty)$$

**Składniki:**
* **$\Delta Warm$**: Wartość bezwzględna różnicy między *Celem Ciepła* a sumą atrybutów `warmth` wszystkich elementów zestawu.
* **$\Delta Rain$**: Różnica między intensywnością deszczu a średnią wodoodpornością zestawu (karane tylko niedostateczne zabezpieczenie).
* **$StylePenalty$ (Fashion Police)**:
    * **+50 pkt** (Kara krytyczna): Za wykrycie "zakazanych par" kolorystycznych (np. Czerń + Brąz, Czerwień + Zieleń).
    * **-10 pkt** (Bonus): Za zestaw monochromatyczny (spójność kolorystyczna).

### D.7. Schemat Działania Algorytmu

```mermaid
graph TD
    %% Definicja stylów
    classDef process fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef decision fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;
    classDef terminator fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    Start((Start)) --> Input[Pobranie Danych:<br/>Pogoda + Szafa + Okazja]
    Input --> Physics[Obliczanie Fizyki:<br/>Wind Chill & Rain Intensity]
    Physics --> Filter{HARD CONSTRAINTS<br/>Filtrowanie Szafy}
    
    Filter -->|Odrzuć| Bin[Kosz:<br/>Przemakalne, Nieadekwatne termicznie,<br/>Zły styl na okazję]
    Filter -->|Akceptuj| Valid[Lista: Valid Closet]
    
    Valid --> Layers{Jaka Temperatura?}
    
    Layers -->|Zimno <10C| L3[Sloty: Baza + Środek + Kurtka]
    Layers -->|Średnio 10-22C| L2[Sloty: Baza + Środek]
    Layers -->|Ciepło >22C| L1[Sloty: Baza]
    
    L3 & L2 & L1 --> Combo[Generowanie Kombinacji<br/>Iloczyn Kartezjański]
    
    Combo --> Score{OCENA PUNKTOWA}
    Score --> Calc1[Kara za różnicę ciepła]
    Score --> Calc2[Kara za brak wodoodporności]
    Score --> Calc3[Kara za gryzące się kolory]
    
    Calc1 & Calc2 & Calc3 --> Sum[Suma Punktów Karnych]
    Sum --> Best[Wybierz zestaw z MIN Score]
    Best --> Output(((WYNIK:<br/>Lista ID Ubrań)))

    class Input,Physics,Valid,L1,L2,L3,Combo,Calc1,Calc2,Calc3,Sum,Best process;
    class Filter,Layers,Score decision;
    class Start,Output terminator;
