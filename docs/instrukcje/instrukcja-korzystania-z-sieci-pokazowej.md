---
title: Instrukcja korzystania z sieci pokazowej
---
Opis stanowiska: [sieć pokazowa](../stanowiska/siec-pokazowa)

Wszystkie akcje należy wykonywać będąc podłączonym do sieci WiFi lab340.

## Sterowanie urządzeniami i odczytywanie czujników

Odczytywanie pomiarów z czujników oraz sterowanie urządzeniami w sieci (rolety, żarówki) najłatwiej zrealizować przez ekran dotykowy znajdujący się w laboratorium lub za pomocą swojego laptopa/telefonu korzystając z dashboardu w Home Assistant (HA). W tym celu należy:

1. Wejść na adres: [http://lab340-c001.local:8123/](http://lab340-c001:8123/) - `lab340-c001.local` to nazwa głównego serwera Raspberry Pi (zamiast tego można użyć adresu IP), a `8123` to port, na którym znajduje się HA.
2. Zalogować się na konto studenta (login `student`, hasło `student`).
3. Wybrać z bocznego paska odpowiedni dashboard.

## Analiza danych

Dostęp do danych zbieranych w sieci pokazowej można uzyskać na 2 sposoby:

### Za pomocą MQTT (dane w czasie rzeczywistym)

Należy połączyć się z brokerem MQTT (np. za pomocą klienta MQTT Explorer lub skryptu w Pythonie) pod adresem `lab340-c001.local` na porcie 1883 z autoryzacją za pomocą loginu `student` i hasła `Student340Pass`. Przy każdym urządzeniu w pokoju 340 znajduje się temat MQTT, na który dane urządzenie publikuje wiadomości. Tematy MQTT poszczególnych urządzeń można także znaleźć na liście urządzeń - zakładka Wyposażenie laboratorium.

### Za pomocą bazy danych InfluxDB

Wszystkie pomiary zapisywane są do bazy danych InfluxDB na porcie 8086. Można je pobrać stamtąd bezpośrednio lub wykorzystać narzędzie Grafana na porcie 3001. Dostęp do tych danych wymaga konta administratora.

## Dodawanie nowych urządzeń

!!! danger "Uwaga: Dodawanie urządzeń do sieci pokazowej wymaga uprawnień administratora! Jeśli chcesz dodać nowe urządzenie skontaktuj się z administracją laboratorium. Jeśli chcesz samodzielnie nauczyć się dodawania nowych urządzeń wykorzystaj stanowisko [Home Assistant board](../stanowiska/home-assistant-board/)."

Przed dodaniem urządzenia do sieci pokazowej zapisz jego szczegóły w arkuszu dostępnym w zakładce Wyposażenie laboratorium. Po uzupełnieniu szczegółów zostanie wygenerowana jego nazwa.

Procedura dodawania urządzenia zależy od typu komunikacji bezprzewodowej, z której ono korzysta:

- Zigbee – wejść na [lab340-c001.local:8080](lab340-c001.local:8080) lub [192.168.0.102:8080](192.168.0.102:8080) (panel GUI Zigbee2MQTT) i przez GUI dodać urządzenie, a następnie zmienić jego nazwę (na taką jaka wychodzi w arkuszu).
- MQTT – urządzenie musi się połączyć z wifi oraz z brokerem pod adresem `lab340-C001.local:1883` (lub `192.168.0.102:1883`, bo po nazwie nie zawsze chce działać)

## Architektura sieci (dla zaawansowanych)

