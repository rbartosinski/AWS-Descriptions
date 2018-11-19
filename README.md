# AWS-Descriptions

Knowledge base about basics and standards in Amazon Web Services.
Baza wiedzy nt. Amazon Web Services.
---

##1.IAM

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

IAM
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

##2. EC2

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

##3. RDS
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

##4. S3 * Simple Storage Service

Zapisywanie obiektów:
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
