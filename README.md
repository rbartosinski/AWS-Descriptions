# AWS-Descriptions

Knowledge base about basics and standards in Amazon Web Services.

Baza wiedzy nt. Amazon Web Services.

---

## 1.IAM - Identity and Access Management

IAM:
* scentralizowana kontrola konta AWS
* dzielenie dostępu do usług
* pozwolenia Granular Permissions
* Identity Federation
* Multifactor Authentication * MFA
* roboczy dostęp do użytkowników, urządzeń i usług
* integracja z usługami AWS
* wspracie PCI DSS Compliance

Użytkownicy
Grupy
Ustawienia pozwoleń
Role - przypisywanie usługom
Uprawnienia AWS
Policies - zasady - dokument definiujący jedno lub więcej uprawnień

Access Key i Secret Access Key - potrzebne do logowania poprzez Command Line Interface (CLI) lub API

Nazwa użytkownika i hasło - logowanie poprzez konsolę AWS

IAM:
* uniwersalny - nie podzielny na regiony,
* root - pierwszy użytkownik, tworzony wraz z kontem AWS,
* nowi użytkownicy NIE MAJĄ uprawnień,
* nowi - przypisuje im się podczas tworzenia Access Key i Secret Access Key

Role:
* pozwalają nie używać Access Key i Secret Access Key
* preferowane z perspektywy bezpieczeństwa,
* kontrolowane przez Policies,
* zmiana zasad w roli - efekt natychmiastowy,
* dodawanie/usuwanie ról z instancji EC2 bez ich wyłączania.

Least Privilege (przywilej "najmniej") * dawaj użytkownikom najmniejszą ilość wymaganego/potrzebnego dostępu.

Używaj grup - użytkownicy będą dziedziczyć prawa pozwoleń automatycznie

Access Key i Secret Access Key można regenerować

Zasada 1 Key Pair - 1 użytkownik

---

## 2. EC2 - Elastic Compute Cloud

Elastic Compute Cloud (EC2) - instancja elastycznie zmieniającej rozmiar chmury obliczeniowej.

Redukcja czasu potrzebnego do uzyskania i wdrożenia nowej aplikacji/serwera do minut.

Szybkie skalowanie (w dwóch kierunkach) w zmieniających się wymagań biznesu.


EBS (Elastic Block Storage) - dysk w chmurze:
* można do dołączyć (attach) do EC2
* umieszczony w konkretnej Availibility Zone
* automatyczna replika zabezpiecza dane pojedyńczej instancji

Typy:

SSD:
GP2 - General Purpose SSD:
* równowaga ceny i osiągów
* duża różnorodność
* do 10 tys. IOPS (wejść/sek)

IO1 - Provisioned IOPS SSD:
* najwyższe możliwości
* krótka latencja
* 10-20 tys. IOPS
* np. duże NoSQL DB

Magnetic:
ST1 - Throughput Optimized HDD:
* big data
* data werehouses
* przetwarzanie logów
* nie może być bootowalny

Cold HDD:
* najniższe koszty
* mała frekwencja użytkowania

Magnteic:
* poprzednia generacja
* może być bootowalny

ELB
Elastic Load Ballancer - równowaga obciążenia
* Application Load Ballancer
* Network Load Ballancer
* Classic Load Ballancer

Błąd 504 - bramka przekroczyła czas.
Przyczyna WEB Server lub DB Server

IPv4 Address - zobacz X-Forwarded-For-Header

Route 53
* Amazon DNS
* domeny dla instancji EC2, load ballancerów, bucketów S3

---

## 3. RDS - Relational Database Service

Online Transaction Processing OLTP
Online Analitics Processing OLAP

RDS (OLTP):
* SQL,
* MySQL,
* PostgreSQL,
* Oracle,
* Aurora,
* MariaDB.
DynamoDB(NoSQL DB)
RedShift (OLAP)
Elasticache:
* Memcached,
* Redis.

2 typy backupów w AWS:
* Automated Backups,
* Database Snapshots.

Automated Backups:
* pozwalają na odtworzenie bazy z dowolnego punktu w okresie przechowania (rentention period): 1-35 dni
* zawierają pełny dzienny snapshot bazy i logi transactions tego dnia
* domyślnie włączone
* tworzone bez aktywnego okna konsoli
* może wystąpić wrażenie opóźnienia

Snapshots:
* tworzone manualnie - inicjuje użytkownik
* są przechowywane po usunięciu instacji RDS (w odróżnieniu od Automated Backups)

Szyfrowanie (encryption)
Dla SQL zrobione za pomocą Key Management Service (KMS)
Szyfrowane są instancja, dane przechowywane i backupy.

Dla istniejących wcześniej instancji baz danych szyfrowanie AWS nie jest dostępne.
Żeby zasztfrować taką bazę należy zrobić jej snapshot, następnie jego kopię i tę kopię zaszyfrować.

MULTI AZ RDS
pozwala na stworzenie dokładnej kopii produkcyjnej bazy danych w innej strefie dostępności (Av. Z)
Działa na zasadzie replik i synchronizacji z oryginałem w czasie rzeczywistym.
Jeśli pojawi się bład RDS przełączy awaryjnie (failover) do działającej instancji bez ingerencji admina.

Multi AZ stosowane powinno być tylko w krytycznych sytuacjach (disaster recovery).
Do zwiększania wydajności baz służyć mają READ REPLICA.
Multi AZ wspiera (5): SQL Server, MySQL Server, Oracle, PostgreSQL, MariaDB.

READ REPLICA
kopie produkcyjnej bazy danych tylko do odczytu
możliwe jest osiągnięcie asynchronicznej repliki pierwszej instancji RDS
używane są do mocno obciążonych baz danych lub/i do skalowania

Read replica wspiera (4): PostgreSQL, MySQL, MariaDB, Aurora.

READ REPLICA RDS:
* do wrpowadzenia RR muszą być włączone Automated Backups
* możesz mieć 5 replik każdej bazy danych
* możesz mieć repliki replik (opóźnienia)
* każda replika ma swój DNS endpoint
* repliki mogą mieć ustawione Multi AZ
* można stworzyć replikę Multi AZ
* zezwolenie na replikę w innym regionie


ELASTICACHE RDS
powala wdrożyć, operować i skalować in-memory cache w chmurze

Usługa ulepsza, zwiększa wydajność aplikacji poprzez zezwolenie na odczyt informacji z szybkiej, zorganizowanej pamięci cache zamiast pobierania wolno danych zapisanych na dyskach baz danych.

Typy Elasticache:
1 * Memcached - szybko adaptowalny do pamięci system cachowania obiektów.
2 * Redis - open-source pamięciowy system Klucz-Wartość, wspiera różne struktury danych np. posortowane SetListy

Elasticache wspiera Master/Slave repliki i Multi AZ.

Memcached (przykłady wykorzystania):
* cachowanie obiektów jest priorytetowe,
* most simply caching model,
* uruchamiane są duże węzły cache i potrzebna jest wielowątkowa wydajność w ich wykorzystaniu
* możliwość skalowania horyzontalnego

Redis
* bardziej zaawansowane typy danych jak listy, sety,
* sortowanie datasetów w pamięci i tabele wyników (leaderboards),
* wymagana jest trwałość (persistence)
* chcesz używać różnych Av. Zones w przełączeniu awaryjnym.

Elasticache używane do odciążania DB opartych na odczycie danych read-heavy stress-load, raczej nie do zapisów

RedShift dobre do odciążania baz danych związanych z analizą OLAP

---

## 4. S3 - Simple Storage Service

S3 - Simple Storage Service - zapisywanie obiektów:
* bezpieczne,
* trwałe,
* wysoko-skalowalne,
* łatwe w użyciu,
* każda ilość danych,
* z każdego miejsca w sieci.

Bazuje na obiektach.

Dane są rozciągnięte na wiele urządzeń dając wiele udogodnień.

Pliki: od 0 bajtów do 5 TB / 1 plik

Nie ma limitu danych.

Bucket - universal name space (name unique globally)

http://s-3-eu-west-1.amazonaws.com/.....

Upload plików: odbierasz HTTP 200 jeśli się udało.

Zastępnowanie (metoda PUT) może zabrać trochę czasu.

System oparty na zasadzie Klucz-Wartość, gdzie:
* klucz - nazwa obiektu,
* wartość - dane (sekwencje bajtów)
* VersionID - ważne dla wersjonowania
* Metadata - dane nt. przechowywania
* Subresources - konfiguracja Bucketa (Bucket Policies, Access Control List, Cross Origin Res. Sharing)

S3 - 5 klas przechowania:

S3 Standard:
* gwarancja dostępności (availibilty) 99.99%
* gwarancja trwałości (durability) 99.99999999999% (11x9)
* zarządzanie cyklem życia plików
* wersjonowanie
* szyfrowanie
* bezpieczeństwo porzez kontrolę dostępu i Policies

S3 IA (Infrequently Accessed):
* dla danych mniej używanych, do których dostęp natychmiastowy jest także potrzebny
* mniejsza opłata miesięczna niż w S3 Standard, ale płaci się także za pobranie
* availibility 99.9%
* durability 99.99999999999% (11x9)

S3 OneZone IA:
* jw. ale dane są przechowywane tylko w jednej Availibility Zone
* availibility 99.5%
* durability 99.99999999999% (11x9)
* koszt o 20% niższy niż S3 IA

Reduced Redundancy Storage:
* availibility 99.99%
* durability 99.99%
* dane mogą być utworzone ponownie jeśli przepadną

Glacier:
* tani
* tylko archiwalne dane
* dostęp do danych po ok 3-5 godz.

S3 - opłaty:
* użycie w GB,
* requesty (każde GET, PUT, COPY etc.)
* przechowywanie - zarządzanie - analizy etc.
* zarządzanie danymi - wychodzą poza S3
* przyspieszenie transferu * użycie CloudFront do optymalizacji transferu

S3 nie pasuje do instalacji na systemach opercyjnych lub włączonych bazie danych

Bezpieczeństwo S3:
* domyślnie buckety są prywatne,
* można zmienić ustawienia kontroli dostępu używając:
- poziom bucketa - Bucket Policies
- poziom obiektu - Lista Kontroli Dostępu

Szyfrowanie S3:
* w ruchu: TLS/SSL
* w spoczynku (at rest):
- Server Side Encryption:
1. SSE-S3 - S3 Managed Keys
2. S3-KMS - AWS Key Management Service
3. SSE-C - Server Side Enc. with Customer Provided Keys
- Client Side Encryption.

Za każdym razem po uploadzie pliku do S3 jest inicjowane żądanie PUT

Do headera żądania zostaje dodany parametr x-amz-server-side-encryption. Może mieć dwie opcje:
1. AES256 - SSE-S3 S3 Managed Keys
2. aws:kms - SSE KMS

Dodanie parametru 'x-amz' do headera żądania mówi S3 żeby użyć szyfrowania podczas wgrywania.

Możesz wymusić użycie SSE definiując Bucket Policy, które odrzucają żądania S3 PUT bez dołączonego headera 'x-amz'.

---

## 5. CloudFront

Edge location - miejsce, w którym treści są cache'owane, mogą być też zapisywane. Speracja regionów AWS Region Availibility Zones.

Origin - pierwotna lokalizacja przechowywania plików dystrybuowanych w CloudFront.

Origin:
* S3 Bucket
* instancje EC2
* Elastic Load Ballancer
* Route 53

Distribution - nazwa nadawana CloudFrontowi zawierająca skład kolekcji lokalizacji Edge.

Web Distribution - używana do stron WWW.

RTMP - używana do streamingu.

CloudFront - może być używany do dostarczania całkowitycyh stron WWW zawierających dynamiczne, statyczne, streamingowe i interaktywne treści używając globalnej sieci miejsc Edge.

Requesty takich treści są automatycznie kierowane do najblizszego punktu Edge. Treść jest dostarczana w najlepszej możliwej wydajności.

CF może być u żyty do optymalizacji wydajności dla użytkowników dostępu do stron zamieszczonych na S3.

CF jest zoptymalizowany do działania z innymi usługami AWS jak:
* S3
* EC2
* ELB
* Route 53
* może współpracować także z innymi rozwiązaniami nie będącymi oryginalnie postawionymi w AWS.

2 typy dystrybucji CF:
* Web Distribution - dla stron, używa HTTP/S.
* RTMP Distribution - Adobe Real Time Messaging Protocol, do streamingu mediów, treści multimedialnych flash

Do Edge Location można zapisywać (not just read-only)

CloudFront Edge Locations wykorzystują S3 Transfer Acceleration do zredukowania opóźnień uploadu na S3. Obiekty są cache'owane na czas określony w parametrze TTL (Time To Live). Możesz wyczyścić cache obiektów, ale będzie trzeba zapłacić.

Cross Origin Resource Sharing (CORS):
* używane do określenia dostępu z oryginalnej lokalizacji do innych usług AWS
* domyślnie zasoby w jednym buckecie nie moga mieć dostępu do zasobów w drugim
* do pozwolenia na interakcję niezbędna jest konfiguracja CORS i włączenie dostępu do oryginalnego (Enable Access for the Origin)
* zawsze używaj S3 website URL, nie zaś regular Bucket URL

Performance Optimization dla S3:
* S3 - GET-intensive workloads - użyć CF
* mixed workloads: ustwić random prefix np. HexHash do nazwy klucza zapobiegając powielaniu nazw plików zapisanych na tej samej partycji. Uniknąć sekwencyjnego klucza nazw obiektów

---

## 6. API Getaway

API Getaway - w pełni zarządzana usługa przynosząca API na każdą skalę, będące dodatkowo łatwe w publikacji, utrzymaniu, monitorowaniu i zabezpieczaniu. Za pomoca kilku kliknięć możesz stworzyć API działające na zasadzie Front Doors dla swoich aplikacji np. uruchomionych na EC2, AWS Lambda czy każdej innej webowej aplikacji.

API Getaway daje:
* ustawienie endpointów HTTPS do zdefiniowania RESTFul API
* bezserwerowe połączenie z usługami jak AWS Lambda czy DynamoDB
- wysłanie każdego endpointa do osobnego celu
- uruchomienie z małymi kosztami
- bezwysiłkowe (effortlessly) skalowanie
- śledzenie i kontrola poprzez klucze API
- tłumienie (throttle) żądań zapobiegając atakom
- połączenie z CloudWatchem do tworzenia logów każdego żądania i monitorowania
- utrzymanie różnych wersji tego API

Konfiguracja API Getaway:
* zdefinuj kontener API
* zdefiniuj zasoby i zagnieżdżone zasoby (URL)
*  dla każdego zasobu:
- wybierz metody HTTP
- ustaw bezpieczeństwo
- wybierz cel (EC2, Lambda etc.)
- ustaw żądania i odpowioedź transformacji
* wdróż API do fazy Stage
- użyć API Getaway domeny domyślnej
- może być też dobrana domena użytkownika
- wsparcie AWS Certificate Manager - darmowe SSL/TLS Certs.

API Getaway - Same Origin Policy:
jest ważnym elementem bezpieczeństwa aplikacji webowych. Pod zasadą SOP przeglądarka zezwala skryptom zawartym na pierwszej stronie na dostep do drugiej strony tylko jesli obie mają ten sam 'Origin'. Działa to zapobiegając Cross-Side-Scripting XSS Attacks.

SOP jest egzekwowane przez przeglądarkę, ignorowane natomiast przez narzędzia typu CURL, POSTman.

SOP rozluźnia mechanizm CORS zezwalający na alternatywne zasoby na stronie tzn. żadane z innej domeny niż pierwotna.

API Getaway jest:
* wysokopoziomowe
* caching objętości do zwiększenia wydajności
* małe koszty automatycznego skalowania
* logi wyników w CloudWatch
* używając JS/AJAX dla wielu domen należy upewnić się o włączeniu CORS w ustawieniach API Getaway
*  CORS wykonywane po stronie klienta

