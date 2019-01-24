# AWS-Descriptions

Knowledge base about basics and standards in Amazon Web Services (in Polish).


1. [IAM - Identity and Access Management](#question1)
2. [EC2 - Elastic Compute Cloud](#question2)
3. [RDS - Relational Database Service](#question3)
4. [S3 - Simple Storage Service](#question4)
5. [CloudFront](#question5)
6. [API Getaway](#question6)
7. [AWS Lambda](#question7)
8. [AWS Step Functions](#question8)
9. [AWS X-Ray](#question9)
10. [DynamoDB](#question10)
11. [KMS - Key Management Service](#question11)
12. [SQS - Simple Queue Service](#question12)
13. [SNS - Simple Notification Service](#question13)
14. [SES - Simple Email Service](#question14)
15. [Elastic Beanstalk](#question15)
16. [Kinesis](#question16)
17. [AWS Cognito](#question17)
18. [Continous Integration / Continous Deployment Services](#question18)
19. [CloudFormation](#question19)


---
<a name="question1"></a>
## 1. IAM - Identity and Access Management

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


3 typy IAM Policies:
- Managed Policies - zarządzane przez AWS zasady domyślne
- Customer Managed Policies - zarządzane przez ciebie
- Inline Policies - zarządzane przez ciebie zszyte z pojedyńczym użytk, grupą, rolą


Managed Polcies - zasady tworzone i administrowane przez AWS.

AWS dostarcza MP dla większości użytkowników w przypadkach opartych na funckji użytkownika np. dostęp do Dynamo DB, code commit itd.

Te zasady pozwalają na przypisanie właściwych uprawnień do użytkowników, grup czy ról bez konieczności pisania tych uprawnień.

Pojedyńcza zasada MP może być dołączona do wielu użytkowników, grup czy ról z tym samym kontem AWS, lub poprzez różne konta.

Nie można zmienić uprawnień zdefiniowanych w AWS MP.


Inline Policies - zasady zszyte z kontem wybranego użytkownika, grupą lub rolą, na którą zostały zaaplikowane. Relacja 1:1 pomiędzy jednostką a zasadą.

Podczas usunięcia użytkownika, grupy czy roli zszyta zasada także zostanie usunięta.

W większości przypadków AWS poleca używanie Managed Policies.

Zasady Inline są funkcjonalne kiedy chcesz być pewien, że określone zezwolenia nie są dostępne żadnemu innemu żytkownikowi, grupie, czy roli poza tą w której zostały użyte.

---
<a name="question2"></a>
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
<a name="question3"></a>
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

Elasticache używane do odciążania DB opartych na odczycie danych read-heavy stress-load, raczej nie do zapisów np.
- Social Networking
- Gaming media sharing
- Q&A portals

Często odkładane dane są przechowywane w pamięci dla mało opóźnionego dlaszego dostępu.

Dobre także dla obliczeń np. silniki rekomendacji.

Może być używane do przechowania wyników wymiany informacji wyjście/wejście intensywnie obciążonych baz danych z zapytaniami lub wyjścia (output) innych intensywnych obliczeń.


Elasticache Lazy Loading:
- tylko żądane dane są cacheowane
- unika cacheowania bezużytecznymi danymi
- najpierw żądanie inicjujące, następnie żądanie do bazy, następnie zapisanie w cache
- nieświeże dane - dane w cache aktualizowane tylko kiedy ich brakuje, nie uzupełniają się automatycznie ze zmianami w bazie danych

LL Time To Live (TTL):
- specyfikacja sekund dopóki klucze (danych) się nie przeterminują do uniknięcia nieświeżych danych trzymanych w cache
- LL traktuje przeterminowany klucz jako brak pamięci i przyczynia się do wznowienia wyjęcia przez aplikację danych z bazy zapisując je z nowym TTL
- nie eliminuje nieświeżych danych, ale pomaga ich uniknąć


Elasticache Write Through:
- dodaje lub aktualizuje dane do cache podczas zapisu do bazy danych
- dane w cache nigdy nie są nieświeże
- kara za zapis: każdy zapis pociąga za sobą zapis do cache 
- użytkownicy bardziej tolerancyjni na ew. opóźnienia podczas aktualizacji niż podczas kolejnego odczytania
- jeśłi łańcuch danych zawiedzie dane są tracowne do ponownego zapisu w bazie (może pomóc integracja z Lazy Loading)


- Cache są pomiędzy bazą danych a aplikacją.
- Dwie strategie cacheowania LL i WT.
- LL cachuje tylko podczas żądań danych
- ELasticache Node zawodzi nie fatal, ale usuwając cache
- Uniknięcie nieświeżości danych poprzez implementację TTL


RedShift dobre do odciążania baz danych związanych z analizą OLAP

---
<a name="question4"></a>
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
<a name="question5"></a>
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
<a name="question6"></a>
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
<a name="question7"></a>
## 7. AWS Lambda

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
<a name="question8"></a>
## 8. AWS Step Functions

AWS SF pozwalają wizualizować i testować bezserwerowe aplikacje.

Dają możliwość graficznego wykazu i wizualizacji jak działąją poszczególne komponenty aplikacji w serii kroków. Czyni to prostym stawianie tego rodzaju wielo-krokowych aplikacji.

Step Functions automatycznie wyłapują i śledzą każdy krok i każdą próbę błędu, aplikacja jest sprawdzana w przewidzianej kolejności.

SF zapisuję log stanu każdego kroku dlatego można szybko zdiagnozować i debugować ew. błedy.

---
<a name="question9"></a>
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
<a name="question10"></a>
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

---
<a name="question11"></a>
## 11. Key Management Service

AWS KMS - usługa do zarządzania ułatwiająca tworzenie i kontrolę nad kluczami szyfrowania używanymi do szyfrowania danych. Zintegrowana z innymi usługami jak EBS, S3, Redshift, RDS i innymi.

The Customer Master Key - klucz używany do rozszyfrowania danych w kluczu ('envelope key'). Envelope Key używany do rozszyfrowania danych.

The Customer Master Key:
CMK:
- alias
- data utworzenia
- opis
- stan klucza
- KEY MATERIAL (dostarczony przez klienta czy przez AWS)

Nie powinno być nigdy eksportowane.

Ustalanie CMK:
- utwórz alias i opis
- wybierz materiał
- zdefiniuj Key Administrative Permissions: IAM users/roles, nie poprzez KMS API
- zdefiniuj Key Usage PermissionsL IAM users/roles mogą żywać klucza do szyfrowania i rozszyfrowania danych

Key Material Options:
- Use KMS generated key material
- your own material

Polecenia KMS API:
- aws kms encrypt
- aws kms decrypt
- aws kms re-encrypt
- aws kms enable-key-rotation

---
<a name="question12"></a>
## 12. Simple Queue Service

SQS - Simple Queue Service

SQS - usługa dająca dostęp do kolejki wiadomości, które mogą być użyteczne w przedstawianiu wiadomości podczas oczekiwania na wykonanie obliczeń.
System dystrybucji kolejki który daje możliwości szybkiego wysłania kolejki wiadomości z jednego komponentu aplikacji gdzie są one generowane do innego w którym są konsumowane. Kolejka jest roboczym repozytorium dla wiadmości, które oczekują przetworzenia.

Dzięki SQS możesz rozdzielić (decouple) komponenty aplikacji tak żeby działały niezależnie, komunikując się zarządzanymi wiadomościami.

Każdy komponent aplikacji może umieścić wiadomość w kolejce. Wiadomości mogą mieć do 256 KB teksu lub każdego innego formatu. Każdy komponent może następnie ponownie odczytać wiadomości , co da się zaprogramować używając SQS API.

Kolejka działa jako bufor pomiędzy komponentami produkującymi i zapisującymi dane, a komponenty odczytują dane do przetworzenia. Tzn. że kolejka załatiwa problem związany z szybszą produkcją danych niż możliwościami ich konsumpcji lub ich przetworzenia, lub jeśli producent lub konsument są połączone do sieci z przerwami.

2 typy kolejek:
- Standard Queues (domyślna)
- FIFO Queues (First-In-First-Out)

Std queue daje niemal nielimitowaną ilość transakcji na sekundę. Gwarantuje że wiadomości zostaną dostarczone przynajmniej raz (at least once). W każdym razie, czasami może zdarzyć się że jedna kopia wiadomości zostanie dostarczona dwukrotnie (ze względu na architekturę wysokiej dystrubucji pozwalającą na wysoką wydajność). Kolejność w Std. Q. jest najmniej wysiłkowa i zazwyczaj wiadomości są dostarczane w kolejności wysłania.

Fifo uzupełnia std. q. Najważniejszym udogodnieniem jest dostarczanie wiadomości dokładnie w tej kolejności w której zostały wyłane. Wiadmość jest odbierana jeden raz i jest dostępna do jej przetworzenia i usunięcia. Duplikatu nie są włączane do kolejki. FIFO wspiera też wiadomości do grup poprzez multiple ordered message groups w jednej kolejce. FIFO ma limit do 300 transakcji na sekundę, objętość jak w std. Dobre np. dla transakcji bankowych, gdzie zdarzenia muszą mieć ścisłą kolejność.

SQS:
- pull-based (wciąganie)
- wiadomości 256 KB
- wiadomości mogą być trzymane w kolejce od 1 min. do 14 dni
- domyślny okres przechowania ustawiony na 4 dni
- gwarancja przetworzenia wiadomości przynajmniej raz

SQS - Visiblity Timeout - Limit czasu widoczności:
- ilość czasu kiedy wiadomość jest niewidoczna w kolejce dopóki czytelnik jej nie podniesie. Dostarczone zadanie jest przetwarzane przed upływem limitu widoczności, następnie wiadomość będzie usunięta z kolejki. Jeśli zadanie nie zostanie przetworzone w tym czasie, wiadomość znów będzie widoczna i kolejny odczytujący przetworzy zadanie. Wynikiem tego wiadomośc może być dosatrczona dwukrotnie.

Domyślny czas widoczności mija po 30 sekundach.
Nalezy zwiększyć czas jeżeli zadanie zajmuje więcej. Max to 12 godzin.

SQS Long Polling - sposób na otrzymywanie wiadomości z kolejki SQS. Podczas gdy regularne wiadomości dochodzą w trybie short polling - natychmiastowo, long polling nie zwraca odpowiedzi dopóki wiadomość nie odtrze do kolejki, lub skończy się czas long poll. Long poll może zaoszczędzić pieniądze.

---
<a name="question13"></a>
## 13. Simple Notification Service

SNS - Simple Notification Service - usługa dająca łatwe do skonfigurowania, operowania, wysyłania powiadomienia z chmury.

Przynosi programistom wysoko skalowalne, elastyczne, dobre kosztowo, pojemności do publikowania wiadomości z aplikacji, natychmiastowo dostarczając je do subskrybentów lub innych aplikacji.

Wypychanie powiadomień (push) do urządzeń Apple, Google, Fire OS, Windows podobnie jak urządzenia Android w Chinach z Baidu Cloud Push.

Poza wypychaniem (pushing) powiadomień z chmury bezpośrednio do urządzeń mobilnych SNS także dostarcza powiadomienia poprzez SMSy lub emaile do kolejki SQS lub do każdego endpointa HTTP.

Powiadomienia SNS mogą zaangażować funkcje Lambda - kiedy wiadomość jest publikowana do tematu SNS (SNS topic) a lambda jest na liście subskrypcji, funkcja może zostać wywołana z ładunkiem, który zawira publikowana wiadomość. Lambda otrzymuje ładunek, zawartość wiadomości jako parametr wejściowy, nast następnie może manipulować informacją zawartą we wiadomości i opublikować wynik jako kolejny temat SNS, lub wysłać wiadomość do innych usług AWS.

SNS zezwala na grupowanie różnych odbiorców tematów. Temat jest 'access pointem' zezwalającym odbiorcom na dynamiczne subskrybowanie na identyczną kopię tych samych powiadomień.

Jeden temat (topic) może wesprzeć dostarczenie do różnych typów endopintów np. można zgrupować razem użytkowników iOS, Androida, czy odbiorców SMS. Publikujkąc raz temat, SNS dostarczy dobrze sformatowane pod każde urządzenie kopię tej samej wiadomości do każdego subskrybenta.

Do zapobieżenia zgubienia wiadomości publikowanych za pomocą SNS, są one przechowywane w różnych strefach Av. Zones.

SNS:
- push-based (wypychanie)
- publication/subscription model (pub/sub)
- proste API i integracja z aplikacjami
- elastyczne dostarczanie wiadomości ponad różnymi protokołami transportu
- płatność za wykorzystaną usługę bez wcześniejszych kosztów
- oparte na sieci zarządzanie przez AWS Management Console


SNS vs. SQS
- obie usługi dot. wysyłania wiaodmości
- SNS - push
- SQS - pull


Uzytkownik płaci za:
- $ 0,50 za milion żądań SNS
- $ 0,06 za 100 tys. powiadomień dostarczonych przez HTTP
- $ 0,75 za 100 powiadomień SMS
- $ 2,00 za 100 tys. powiadomień email

SNS - fan out messages (rozwijanie) do dużej liczbny odbiorców

---
<a name="question14"></a>
## 14. Simple Email Service

SES - emaile - skalowalna, wysoko dostęp na usługa wysyłania emaili zaprojektowana do pomocy zespołom marketingowym i aplikacjom wysyłającym dane marketingowe, powiadomienia, emaile dot. transakcji, dla użytkowników płacących za wykorzystanie usługi.

Może być też używana do odbierania maili: wiaodmości przychodzące odbierane będą dostarczane do bucketa S3.

Wiadomości przychodzące moga zostać przechwycone też przez Lambdę lub powiadomienia SNS.

SES - przykłady użycia:
- automatyzacja emaili
- potwierdzenia zakupu, powiadomoienia o nowościach, zmianie statusu zamówienia
- komunikacja markjetingowa, newslettery, reklamy

Wymagany jest email do rozpoczęcia wysyłania (nie oparte na subsrypcji).

---
<a name="question15"></a>
## 15. Elastic Beanstalk

Elastic Beanstalk - usługa do wdrażania i skalowania aplikacji webowych napisanych w językach: Java, .NET, PHP, Node.js, Python, Ruby, Go, także kontenerów Dockera i innych serwerowych platform jak Apache Tomcat, Ngin, Passenger czy IIS. Dzięki EB programiści nie muszą przejmować się infrastrukturą potrzebną do uruchomienia aplikacji.

Po wgraniu kodu EB zajmie się wdrożeniem, zabezpiecznieniem pojemności, równowagą obciążenia, auto-skalowaniem i stanem aplikacji.

Zachowujesz pełną kontrolę usług bazowych AWS, które zostaną użyte do uruchomienia aplikacji, płacąc tylko za wymagane zasoby jak instancje EC2, czy buckety S3.

Elastic Beanstalk:
- najszybszy i najproszty sposób do wdrożenia aplikacji w AWS
- automatyczne skalowanie (up and down)
- można wybrać optymalną dla siebie instancję EC2
-pełna kontrola administracyjna nad usługami potrzebnymi do uruchomienia aplikacji
- zarządzana platforma aktualizacji zawierająca aktualizacje automatyczne do OS i języków programowania
- monitorowanie i zarządzanie stanem aplikacji przez pulpit
- integracja z CloudWatch i X-Ray dla wydajności danych i analiz

Jeśłi wymagany jest serwer EB stworzy odpowiednią instancję. 

Typy wdrażania poprzez Elastic Beanstalk:
- all at once
- rolling
- rolling with additional batch
- immutable

Dwa ostatnie typy zapewniają pełne utrzymanie aplikacji podczas wdrożenia.

All at once:
- wdrażanie nowej wersji do wszystkich instancji symultaniczniel
- wszystkie instancje są wyłączone dopóki trwa wdrażanie
- wrażenie postoju podczas wdrażania
- nie najlepsze dla systemów z misją krytyczną
- jeśli aktualizacja zawiedzie należy wykonać cofnięcie (roll back) zmian poprzez ponowne wdrożenie kodu oryginalnej wersji do wszystkich instancji

Rolling (cofnięcie):
- wdrażanie nowej wersji partiami (batches)
- każda partia instancji jest wyłączana podczas wdrażania
- objętość środowiska będzie zredukowana do numeru instancji w partiach dopóki trwa wdrożenie
- nie najlepsze opcja dla systemów wrażliwych
- jeśli aktualizacja zawiedzie należy wykonać dodatkowe uruchomienie aktualizacji do cofnięcia zmian

Immutable:
- wdrożenie nowej wersji do nowych instancji w nowych autoskalowanych grupach
- kiedy nowe instancje zostaną uruchomione poprawnie ich stan zostanie sprawdzony, zostaną one przeniesione w miesjce istniejących grup autoskalowania, finalnie zastępując stare instancje które zostaną wyłączone
- utrzymuje pełne działanie aplikacji podczas wdrażania
- wpływ nieudanych aktualizacji jest znikomy, należy jedynie wyłączyć nowe grupy autoskalowania
- preferowany typ wdrożenia dla aplikacji misji krytycznych produkcji


Konfigruacja Elastic Beanstalk przeprowadzana jest za pomocą plików konfiguracyjnych (np. można zdefiniować co należy zainstalować, stworzyć środowisko Linuxa, odpalić komendy shell, wyspecjalizować usługi czy skonfigurować load ballancer).

Plik konfiguracyjny w formacie YAML lub JSON. Opcjonalna może być nazwa pliku ale musi mieć rozszerzenie '.config' i zostać zapisany w folderze '.ebextensions'. Ten z kolei folder należy zapisać w katalogu głównym, w kórym znajduje się kod całej aplikacji.


RDS i Elastic Beanstalk

Żeby zezwolić aplikacji wdrożonej przez EB na dostęp do relacyjnych baz RDS należy wykonać dwa dodatkowe kroki podczas konfiguracji:
- dodatkowa Security Group musi zostać dodana do środowiska grupy auto-skalowania
- potrzebne jest także dostarczenie 'connection string configuration information' do serwera twojej aplikacji (endpoint, lub hasło).

2 typy uruchomienia RDS z EB:
Razem:
- podczas wyłączenia środowiska EB baza zostanie także wyłączona
-szybkie i łatwe uruchomienie bazy
- odpowiednie dla środowisk deweloperskich i testowych

Osobno:
- dodatkowy krok konfiguracyjny z użyciem Security Group i Connection Information
- odpowiednie dla środowisk produkcyjnych, bardziej elastyczne
- zezwala na połączenie z różnych środowisk do jednej bazy, można wyłączyć aplikację bez wpływu na bazę

---
<a name="question16"></a>
## 16. Kinesis

Amazon Kinesis - platforma AWS do wysyłania danych streamingowych. Kinesis sprawia, że łatwo analizować dane streamingowe, daje też możliwość do zbudowania własnej aplikacji.

Streaming - dane generowane bez przerwy poprzez tysiące zasobów, zazwyczaj wysyłane jako 'nagrania' symultanicznie w małych wielkościach.

3 typy Kinesis:
- Streams
- Firehose
- Analitycs

Kinesis Streams:
- Video Streams - bezpieczne wysyłanie syteamingu video z urządzeń podłączonych do AWS dla analiz i uczenia maszynowego
- Data Streams - budowa aplikacji przetwarzających dane w czasie rzeczywistym

Kinesis Firehose:
- przechwytywanie, transformacja, wgranie danych streamingowanych do przechowania na zbliżone do czasu rzeczywistego anazlizy z urzyciem BI tools

Można skonfigurować Lambdę do subskrybcji Kinesis Sreams i wykonania funkcji przed wysłaniem przetworzonych danych do finałowego miejsca przeznaczenia.

---
<a name="question17"></a>
## 17. AWS Cognito

Web Identity Federation - daje użytkownikom aplikacji w AWS dostęp do ich zasobów po tym jak pomyślnie zostaną zalogowani (authenticated) do popularnych serwisów jak Amazon, Facebook czy Google.

Po udanej weryfikacji użytkownik otrzymuje kod autoryzacyjny z Web ID Provider który zostanie zamieniony z robocze (temporary) AWS security credentials.

Amazon Cognito dostarcza WIF:
- zalogowanie i wylogowanie dla aplikacji
- dostęp dla gości
- pełni rolę Identity Broker pomiędzy aplikacją a Web ID providers bez żadnego dodatkowego kodu
- Synchronizacja danych użytkownika na różnych urządzeniach
- Usługa polecana na wszystkie aplikacje urządzeń mobilnych

Cognito to polecane podejście do WIF używając kont social mediów jak Facebook.

Broker COgnito pomiędzy aplikacją a kontami WIF przenosi roboczo uprawninia które zostaną zmapowane do roli IAM pozwalającej na dostęp do wymaganych zasobów.

Nie ma potrzeby do załączenia lub przechowania uprawnień AWS lokalnie w aplikacji, na urządzeniach.


Cognito User Pools

Katalogi użytkowników używane do zarządzania funkcjonalnościami logowania i rejestrowania dla aplikacji mobilnych.

Użytkownik może się zalogować:
- bezpośrednio do User Pool lub
- pośrednio przez providera jak facebook, Amazon czy Google.
Cognito działa jako Identity Broker pomiędzy ID provider a AWS. Pomyślna autoryzacja generuje numer JSON Web Tokens (JWTs).

Identity Pools

daje możliwość unikalnej identifikacji dla Twoich użytkowników i autoryzację ich przez identity providers. Z potwierdzoną tożsamością możesz uzyskać roboczo, limited-privilege AWS credentials do dostępu dp innych usług AWS.


Push Synchronization

Cognito śledzi związek pomiędzy tożsamością użytkownika a różnymi użądzeniami na których się zalogował.

W kolejności dostarczania użytkownikowi wrażenia ciągłego dostępu do aplikacji Cognito używa Push Synchronization do wypchnięcia (push) aktualizacji i zsynchronizowania danych użytkownika pomiędzy różne urządzenia.

SNS jest użyty do wysłania cichych powiadomień do wszystkich urządzeń związanych z daną tożsamością (User ID) podczas kiedy dane są przechowywane w chmurze.

---
<a name="question18"></a>
## 18. Continous Integration / Continous Deployment Services

Najlepsze praktyki CI & CD dla rozwoju i wdrożenia aplikacji pozwalają systematycznie wprowadzać zmiany w aplikacji podczas kiedy system i usługi są stabilnie utrzymane.

Firmy jak AWS, Netfilx, Google czy Facebook są pionierami w tym podejściu do aktualizacji, pomyślnie wprowadzając tysiące zmian dziennie.

Continous Integration:
- system testów uruchamia testy automatyczne na nowo zbudowanej aplikacji
- weryfikacja bugów, zapobiega wprowadzeniu ich do 'master code'
- CI skupia się na małych zmianach kodu które są sukcesywnie komitowane w repozytorium głównym, jednorazowo pomyślnie testowane
- aktualizacje przynajmniej raz na dzień pozwalają wielu programistom pracę nad tą samą aplikacją

Continous Deployment:
- przeniesienie idei automatyzacji na kroki wdrożeniowe, automatyczne wdrożenie nowego kodu zaraz za pomyślnymi testami, eliminując kroki ręczne
- kod jest automatycznie wdrażany najszybciej jak to możliwe po przejściu faz ustalonych przez ciebie w procesie wdrożeniowym (budowa, testowanie, pakowanie)
- małe zmiany są publikowane skucesywnie i wcześnie
- praktyki wymagają żeby budowa, testy i wdrożenie kodu były w pełni zautomatyzowane, CD dodatkowo automatyzuje proces publikacji

Narzędzia CI & CD:

- repozytoria kodu - kontrola źródeł - CodeCommit
- zarządzanie budową kodu - kompilacja kodu źródłowego, uruchamianie testów i paczek kodu - CodeBuild
- framework testowy
- spakowana aplikacja wdrożenia - automatyzacja wdrożenia do EC2, systemy wcześniej istniejące i Lambda - CodeDeploy
- środowiska

Całość - CodePipeline - pełna automatyka przepływu pracy całego procesu publikacji.


Code Commit

W pełni zarządzana usługa kontroli kodu zapewnijąca bezpieczne i wysoko skalowalne narzędzie do pracy z repozytoriami Git.

Użytkownicy tworzą kopie (branche) master repo, które aktualizują niezależnie od siebie bez wpływu na pracę innych programistów.

Zapisane zmiany kodu które są gotowe do wdrożenia zapisywane są jako commity.

Kiedy kod jest zaktualizowany, zmiany są przekładane (merged) do master repo. CC śledzi zmiany w kodzie.

CC daje wszystkie funkcjonalności Gita, można więc użyć Gita z local machine do komunikacji z CC.

Dane są szyforwane w ruchu i spoczynku.

CC oparte na Gicie, centralnym repo dla całego kodu, plików binarnych, grafiki czy bibliotek.

CC utrzymuje historię wersji.

CC aktualizuje kod z różnych zasobów i umożliwa współpracę.


CodePipeline

Narzędzie w pełni zarządzane dające efekt CI & CD.

CP może zorganizować budowę, testowanie i wdrożenie aplikacji za każdym razem gdy zmieni się kod - bazując na określonym przez użytkownika procesie publikacji.

Tradycjonalne podejście wdrożenia jest wolne i zawierające błędy, podczas gdy automatyzacja procesu pozwala programiście często dodawać nowe funkcje aplikacji czy sprawdzać bugi. Jest to szybki sposób na reakcję i zmianę bugów.

Prosty model: Aktualizacja kodu - Budowanie - Testy - Wdrożenie.

Definiujesz co będzie się działo w każdej fazie używając Pipeline GUI lub CLI.

Integracja CP z CodeCommit, CodeBuild, CodeDeploy, Lambda, Elastic Beanstalk, CloudFormation, Elastic Container Service a także narzędziami typu Git czy Jenkins.

Każda zmiana kodu wypychana do repo automatycznie włącza przepływ pracy CP, przechywtuje akcję zdefiniowaną dla każdej fazy.

Pipeline się zatrzyma jeśli jedna z faz zawiedzie. Jeśli np testy automatyczne zawiodą wszystko zostanie zatrzymane, tak aby bugi były możliwe do naprawienia jeszcze przed wdrożeniem.

CP automatyzuje wdrożenie end-to-end opierając się na zdefiniowanym przez użytkownika schemacie.

CP może być skonfigurowane automatycznie do wprowadzania zmian wykrytych w repo.


CodeDeploy

The AppSpec File - plik używany do zdefiniowania parametrów któ©e będą użyte we wdrożeniu za pomocą CodeDeploy.

Struktura pliku zależy od tego co wdrażasz do Lambdy, instancji EC2 czy wcześniej istniejących systemów.


1. AppSpec File - Lambda:
Dla Lambdy AppSpec File może być zapisany w formacie YAML lub JSON i zawierać następujące pola:
- version - zarezerwowane dla przyszłego użycia, aktualnie jedyna zezwolona wartość to 0.0
- resources - nazwa i właściwości fukcji Lambda do wdrożenia
- hooks - specyfikuje funkcję do uruchomienia jako set points w cyklu wdrożenia do zweryfikowania wdrożenia np. weryfikacja testów do uruchomienia przez zezwoleniem na ruch, do wysłania do nowowdrożonych instancji (parametry 'BeforeAllowTraffic' i 'AfterAllowTraffic')

BeforeAllowTraffic - hook używany do określenia zadań lub funkcji do wykonania przed przekierowaniem ruchu do nowo uruchomionych instancji wdrożenia Lambdy (np. weryfikacja poprawnej pracy funkcji).

AfterAllowTraffic - określenie zadań do wykonania po wdrożeniu (np. czy funkcja obsługuje ruch prawidłowo, zgodnie z przeiwdywaniem).


2. AppSpec File - EC2 i aplikacje wcześniej istniejące - napisany w formacie YAML:
- version - zarezerwowane dla przyszłego użycia, aktualnie jedyna zezwolona wartość to 0.0
- os - określenie OS i jego wersji
-files - lokalizacja plików potrzebnych do skopiowania (źródło i cel)
- hooks - cykl życia hooków dających specyfikację skryptów wymaganych do uruchomienia ste points w cyklu wdrożenia (np. rozpakowanie plików aplikacji przed wdrożeniem, uruchomienie testów na nowo wdrożonej aplikacji, rejestracja instancji czy load ballancerów)

- BeforeBlockTraffic - uruchamia zadania przed deregistered a load ballancer
- BlockTraffic - usuwa instancje z load ballancera
- AfterBlockTraffic - uruchamia zadania na instancji za deregistered a load ballancer
- Application Stop
- DownloadBundle - The CodeDeploy Agent kopije aplikację do środ. roboczego
- BeforeInstall - szczegóły i skrypty przedinstalacyjne: backup wersji dotychczasowej, deszyforwanie plików
- Install - The CodeDeploy Agent: z roboczego do właściwych instancji
- AfterInstall - konfiguracja zadań, zmiana uprawnień plików
- ApplicationStart - restart wszystkich usług zatrzymanych wcześniej
- ValidateService - szczegóły nt. sprawdzenia działania usług

Plik AppSpec defiuje wszystkie parametry potrzebne do wdrożenia.

Plik AppSpec musi być umieszczony w katalogu głównym.


CodeBuild - Docker

Elastic Container Service - w pełni zarządzana platforma klastrowa (clustered) pozwalająca na uruchomienie obrazów Dockera w chmurze.

CodeBuild - w pełni zarządzana usługa budowania uruchamiająca ustawienie komend, które zdefiniujesz np. kompilacja kodu uruchamiającego testy.

Komendy Dockera do zbudowania, tagowania i wypchnięcia obrazu do repozytorium ECR:
- docker build -t myimagerepo .
- docker tag myimagerepo:latest725350006743.dkr.ecr.eu-central-1.amazonaws.com/myimagerepo:latest725350006743
- docker push 725350006743.dkr.ecr.eu-central-1.amazonaws.com/myimagerepo:latest

---
<a name="question19"></a>
## 19. CloudFormation

CloudFormation - usługa daje zarządzanie, konfigurację i zabezpieczenie twojej infrastruktury AWS z kodu.

Zasoby definiowane są używając CloudFormation template.

CloudFormation interpretuje szablony i tworzy odpowiednie wywołania API to sworzenia zdefiniowanych zasobów.

2 formaty: YAML i JSON.

CloudFormation:
- infrastruktura zabezpieczona dokładnie, bez pomyłek
- mniejszy czas i wysiłek w konfiguracji ręcznej
- możliwość wersjonowania i parowego sprawdzania szablonu
- darmowa (opłaty za stworzone instancje, usługi bazowe)
- może być używana do aktualizacji, backupów, usunięć

Przykładowe parametry:

AWSTemplateFormatVersion, Description, Metadata, Resources, Parameters (input custom values), Conditions, Mappings (custom mappings like region), Transforms(reference code located in S3).


SAM - Serverless Application Model to rozszerzenie CloudFormation używane do zdefiniowania aplikacji bezserwerowej.

Uproszczona składnia dla zdefiniowania zasobów bezserwerowych: APIs, Lambda F., Dynamo DB Tables itd.

Można użyć SAM Command Line Interface do spakowania kodu do wdrożenia, wgranie na S3 i wdrożenie aplikacji.
- sam package
- sam deploy


AWS CI/CD:
- CodeCommit - usługa kontroli źródła Git
- CodeBuild - usługa kompilacji kodu, uruchamianie testów, pakowanie kodu
- CodeDeploy - automatyczne wdrażanie do instancji EC2, już działających systemów, Lambdy
- CodePipeline - narzędzie przepływu pracy, pełna automatyka całekgo procesu (budowa, testy, wdrożenie)


2 podejścia do wdrożeń:
- In-Place lub Rolling update - zatrzymujesz aplikację na każdym hoście i wgrywasz ostatni kod. Dot. tylko EC2 lub wcześniej istniejących systemów. Do cofnięcia zmian trzeba od nowa wgrać poprzednią wersję kodu.
- Blue/Green - nowe instancje są zabezpieczane i nowa aplikacja jest wdrażana na nowe. Ruch jest kierowany do nowych instancji wg własnego planu. Wspiera EC2, wcześniej istniejące, i Lambdy. Cofnięcie zmian wymaga przekierowania ruchu na oryginalne instancje. Blue - aktywne środowisko, green - nowe.
