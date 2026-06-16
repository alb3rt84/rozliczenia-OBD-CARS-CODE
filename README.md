# rozliczenia-OBD-CARS-CODE

## ESPHome: bilansowanie godzinowe grzałki

Plik `grzalka_bojlera_bilans_godzinowy.yaml` zawiera gotową konfigurację ESPHome dla grzałki bojlera z bilansowaniem godzinowym.

Założenia:

- liczniki importu i eksportu energii aktualizują się co około 30 sekund,
- na początku każdej godziny zapamiętywany jest stan liczników,
- dodatni `Bilans godzinowy energia` oznacza nadwyżkę oddaną do sieci w aktualnej godzinie,
- licznik bilansowania działa także przy wyłączonym trybie AUTO oraz w trybie MANUAL,
- tryb AUTO dobiera moc grzałki z bieżącego bilansu sieci oraz banku godzinowego, żeby nie zwiększać mocy przy aktualnym poborze z sieci,
- częstsze odczyty PV korygują ostatni pomiar mocy sieci o zmianę PV i mocy grzałki; szybka reakcja PV służy do cięcia mocy tylko przy większym niedoborze, a zwiększanie mocy zostaje w głównej pętli bilansowania,
- zwiększanie mocy w AUTO jest limitowane krokami, żeby wolniejszy odczyt licznika sieci zdążył potwierdzić realny wpływ grzałki,
- kolejne zwiększenie mocy jest wstrzymywane do czasu odczytu mocy sieci nowszego niż ostatnia zmiana grzałki oraz 60 sekund stabilizacji po tej zmianie,
- osiągnięcie albo przekroczenie zadanej temperatury CWU oraz obniżenie nastawy poniżej aktualnej temperatury natychmiast wyłącza grzałkę oraz tryby AUTO/MANUAL,
- spadek temperatury CWU poniżej nastawy z suwaka automatycznie włącza tryb AUTO tylko wtedy, gdy AUTO zostało wcześniej wyłączone przez termostat,
- tryb AUTO, MANUAL i ręczne ustawienie mocy grzałki są blokowane przez termostat oraz brak potwierdzonego, rzeczywistego odczytu temperatury,
- konfiguracja próbuje odczytać temperaturę CWU z kilku popularnych `entity_id`; działający odczyt jest widoczny w logu `TERMOSTAT`,
- bufor energii przed pierwszym startem w godzinie zbiera się także przy wyłączonym AUTO; pierwszy start wymaga około `70 Wh` dodatniego bilansu (`60 Wh` bufora + `10 Wh` rezerwy eksportu),
- regulacja AUTO zostawia docelowo około `300 W` eksportu jako margines bezpieczeństwa, zamiast dążyć do zera bilansu,
- sterowanie działa pod autokonsumpcję i godzinowy bilans energii: eksport oraz import są liczone energetycznie w Wh,
- uproszczone nazwy sensorów bilansu: `Bilans godziny`, `Moc chwilowa 30s`, `Dostępna moc z bufora` oraz tekstowy `Status bilansu grzałki`,
- `Bilans godzinowy moc banku` to moc, którą sterownik może dodatkowo wykorzystać, żeby do końca godziny zejść z bilansem możliwie blisko zera.

Przed wgraniem dodaj do `secrets.yaml`:

```yaml
grzalka_bojlera_api_key: "..."
grzalka_bojlera_ota_password: "..."
grzalka_bojlera_ap_password: "..."
wifi_ssid: "..."
wifi_password: "..."
```

Jeżeli Twoje encje w Home Assistant mają inne nazwy, podmień `entity_id` przy sensorach `grid_power_ha`, `licznik_import_ha`, `licznik_export_ha`, `pv_power_ha` i `temp_cwu_ha`.
