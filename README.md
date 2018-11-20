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


Import API

Importowanie API z zewnętrznego pliku do API Getaway jest możliwe za pomocą Swagger 2.0.

1. Importując API możesz stworzyć nowe API rejestrując POST request zawierające definicje Swagger z łądunkiem i konfiguracją endpointa.

2. Możesz też zaktualizować istniejące API, używając PUT requesta zawierającego definicję Swagger. Można aktualizować nadpisując nową definicją, podmieniając w istniejących API. Specyfikujesz opcje używając parametru Mode Query w żądaniu URL.


API Throttling

Domyślnie w API Getaway stan oczekiwania na żądania jest ustawiony na 10 tys. rps.

Maksymalne równoległe (concurrent) ustawienie może wynosić 5 tys. rps (na wszytskie API).

Jeśli przekroczysz 10 tys. rps/1 API lub 5 tys. rps / wszystkie otrzymasz błąd 429: Too many requests.

Możesz skonfigurować API Getaway - SOAP Webservice Passtrough.

---

## 6. AWS Lambda

Lambda functions - wersjonowanie funkcji. 1 lub więcej funkcji, wynikiem czego można pracować z różnymi wersjami środowiska, jak testowe, rozwijane, produkcyjne.

Każda wersja ma unikalną nazwę ARN - Amazon Resource Name.

Po publikacji wersji staje się ona niemutowalna.

Lambda utrzymuje ostatnią wersję funkcji w zmiennej $LATEST. Po aktualizacji kodu funkcji Lambda zamieni kod $LATEST na rzeczywiście ostatnią.

Aliasy wersji

Po aktualizacji/inicjalizacji tworzenia funkcji $LATEST możesz opuclikować wersję pierwszą.

Tworząc alias nazwany np 'prod' będziesz mógł później używać do wywołania wersji pierwszej lambdy. W przypadku problemów można więc łatwo zrobić roll back do wersji wcześniejszej.

Lambda:
* bezserwerowa
* scales-out (not up) automatycznie
* funkcje są niezależne 1 event = 1 funkcja
* lambda może wyłąpać (catch) inną lambdę, wówczas 1 event - x funkcji
* wiele wersji funkcji Lambda
* ostatnia funkcja używa zmiennej $LATEST
* zakwalifikowana wersja używa $LATEST
* wersje niemutowalne po publikacji
* możliwe jest podzielenie ruchu używając różnych aliasów - zastępując $LATEST stworzonymi aliasami
* może robic rzeczy globalnie
* do backupu można użyć bucketu S3

---

## 8. AWS Step Functions

AWS SF pozwalają wizualizować i testować bezserwerowe aplikacje.

Dają możliwość graficznego wykazu i wizualizacji jak działąją poszczególne komponenty aplikacji w serii kroków. Czyni to prostym stawianie tego rodzaju wielo-krokowych aplikacji.

Step Functions automatycznie wyłapują i śledzą każdy krok i każdą próbę błędu, aplikacja jest sprawdzana w przewidzianej kolejności.

SF zapisuję log stanu każdego kroku dlatego można szybko zdiagnozować i debugować ew. błedy.

---

## 9. AWS X-Ray

X-Ray - usługa zbiera dane nt. żądań generowanych przez daną aplikację i daje narzędzia do pokazu tych danychm filtrowania, wzmocnienia sygnału do indentyfikacji możlwości ew. optymalizacji.

Dla każdego żądania można zobaczyć nie tylko prośbę o dostęp i odpowiedź,ale też szczegółowe informacje nt. wykonywania żądań w kolejnych usługach AWS, mikroserwisach, bazach danych czy API.

Architektura aplikacji może być maksymalnie skomplikowana, X-Ray pozwoli śledzić i debugować mimo to.

X-Ray SDK daje:
- przechywytniki do dodania do kodu danej aplikacji, do śladów przychodzących przez żadania HTTP
- client handlers - aplikacja może użyć innych usług AWS
- client HTTP - wzywa inne wewnętrzne i zewnętrzne usługi.

X-Ray - integracja:
- ELB
- Lambda
- API Getaway
- EC2
- Elastic Beanstalk

X-Ray - języki programowania:
- Java
- Go
- Node.js
- Python
- Ruby
- .NET

---

## 10. DynamoDB

DynamoDB:
- szybka i elastyczna NoSQL baza danych
- dla aplikacji potrzebujących milisekundowych opóźnień na każdą skalę
- pełne zarządzanie
- wsparcie dla modeli dwudokumentowych i/lub klucz-wartość
- niezawodna

Dane umieszczone są na dysku SSD, rozciągnięte pomiędzy trzy geograficzne centra danych.

2 modele spójności w D-DB:
- Eventual Consistency Reads (domyślny) - najlepsza wydajność odczytu
- Strongly COnsistency Reads - także do zapisywania

Właściwości D-DB:
- tabele
- items (rząd w tabeli)
- attributes (kolumna)
- wsparcie dla klucz-wartość: klucz - nazwa, wartość - dane
- dokumenty mogą być zapisywane w JSON, HTML, XML
- przechowanie i odtwarzanie danych bazuje na kluczu PrimaryKey
- 2 typy PrimaryKey:
* PartitionKey - unikalny atrybut np. ID użytkownika
* wartością PartitionKey jest wejście na wewnętrzną hash funkcję, która określa partycję lub fizyczną lokalizajcę, w której dane się znajdują

Używając klucza partycji jako PartitionKey na dwóch itemach będzie ten sam PartitionKey.


Kontrola dostępu do D-DB zarządzana jest przez IAM:
- użytkownik z pozwoleniami
- rola
- warunek specjalny IAMConditions do zastrzeżenia dostępu dla wybranych rekordów/tabel np. warunek (Condition) dodany do zasad IAM Policy na udostępnienie użytkownikowi dostępu tylko do danych, gdzie PartitionKey pasuje do jego ID.
- tzw. parametr Fine Grained Access Control: 'dynamodb: LeadingKeys'


Przyspieszenie D-DB poprzez indexowanie:
- Local Secondary Index
- Global Secondary Index

Local Secondary Index:
- może zostać stworzony tylko podczas tworzenia tabeli
- nie można go dodać, usunąć, zmienić później
- ma ten sam PartitionKey co oryginalna tabela
- ma inny klucz SortKey
- daje możliwość różnego spojrzenia na dane, zorganizowania ich poprzez inne SortKeye
- każde zapytanie (queries)bazujące na SortKey jest dużo szybsze używając indexu niż tabeli głównej
- *** dobry PartitionKey np. User ID, SortKey - data utworzenia konta

Global Secondary Index:
- może być utworzony w dowolnym czasie
- inny klucz PartitionKey niż w tabeli głównej
- inne klucze SortKey

Indexy umożliwiają szybkie wyszukiwanie na wybranych kolumnach danych, dają inny punkt widzenia na dane, oparty na alternatywnych kluczach PartiotionKey i SortKey.


Query

Operacja znajdywania itemów w tabeli bazującej na PrimaryKey i odrębnych wartościach do znalezienia np. znajdź atrybuty należące do user id = 212. Odpowiedź: first name, surname, email etc.

Opcjonalnie można użyć SortKey dla zawężenia wyników. Np. jeśli SortKeyem jest data możesz zawęzić wyszukiwanie do 7 dni.


Projection Expression

Domyślnie zapytanie query zwraca wszystkie atrybuty dla itemów, ale można użyć Projection Expression, parametru który zwróci tylko niektóre, wybrane atrybuty. Np. chcąc zobaczyć same emaile.

Query:
- wyniki zawsze są sortowane wg SortKey
- kolejność numeryczna - rosnąca (asc 1,2,3,4...)
- można zmienić kolejność poprzez parametr 'ScanIndexForward' na False
- kodowanie ASCII
- domyślnie queries są Eventual Consistency
- należy wyraźnie zaznaczyć jeśli ma być Strongly Cons.


Scan

Operacja scan bada każdy item w tabeli, domyślnie zwraca wszystkie atrybuty (można użyć Project Expression do zawężenia).

Query bardziej wydajne niż scan:
- scan zrzuca całą tabelę, następnie filtruje wartość do dostarczenia pożądanego wyniku - usuwając/odcinając niepotrzebne dane
- dodatkowy krok działania w postaci usuwania danych w locie
- im więcej danych tym scan dłużej trwa
- scan dużej tabeli zużywa przewidzianą zaporę (provisioned throughput) dla tabeli w pojedyńczej operacji

Domyślnie - scan przetwarza dane sekwencyjnie, po 1 MB, wzrasta z wielkością tabeli.

Można wykonywać scan jednej tabeli w danym czasie.

Można skonfigurować D-DB do użycia skanów róznoległych (parallel scans) zamiast logicznego dzielenia tabeli na indexy i skanować każdy segment rownolegle.

Najlepiej jednak unikać skanów równoległych jeśli tabela lub index mają duże obciążenie odczytu/zapisu aktywności z innych aplikacji.


Scan vs. Query:

- Query znajduje items używając tylko PrimaryKey
- scan bada każdy item
- domyślnie wszystkie atrybuty są zwracane
- użycie Projection Expression do oczyszczenia wyników (refine)
- query - wyniki posortowane wg SortKey
- domyślne sortowanie rosnące
- tylko przy queries 'ScanIndexForward' na False
- query bardziej wydajne

Przyspieszanie:

- redukcja dizłania query i scanów poprzez ustawienie mniejszej wielkości strony używając kilku operacji odczytu
- izolacja operacji skanowania do wybranych tabel i segregacja ich do ruchu krytycznego aplikacji
- skanowanie równoległe bardziej niż domyślne sekwencyjne
- unikanie skanowania jeśli jest możliwość zaprojektowania tabel z użyciem query, get, BatchGetItem API


D-DB: Read and Write Capacity Units

Zapora Provisioned Throughput jest mierzona w Capacity Units

Podczas tworzenia tabeli definiujesz Read C.U. i Write C.U.

1 write Capacity Unit
	= 1 x 1 KB/s
1 read Capacity Unit
	= 1 x 4 KB/s dla Strongly Consistency Read
	= 2 x 4 KB/s dla Eventually Consistency Read

Np.
Zapis:
100 x 512 b/s
512/1 = 0.5 ~ 1 x 100 = 100 -> potrzebne 100 write C.U.

Odczyt:
80 x 3 KB/s
3/4 = 0.75 ~ 1 x 80 = 80 -> potrzebne 80 Strongly C.U. lub 40 Event. C.U.


DAX - przyspieszenie D-DB
- clustered
- managed
- in-memory cache

Dostarcza do 10x szybszej wydajności. Mikrosekundowa wydajność dla milionów RPS.

Idealny dla wysoko-odczytywalnych lub obciążonych aplikacji
- aukcyjne
- gamingowe
- sklepy podczas black friday

DAX działa jako cache danych. Przeniesienie do cache w tym samym czasie co w oryginalnej instancji.

Punkty dostępu to tzw. DAX Cluster.

Jeśli żądane dane są w cache DAX dostarcza je do aplikacji.

DAX nie pasuje do:
- aplikacji z Strongly Cons. Reads (zaspokaja Eventually Cons. Reads)
- aplikacji dużo zapisujących
- aplikacji nie wymagających wielu operacji odczytu
- nie wymagających mikrosekundowego czasu odpowiedzi

