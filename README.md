# rozliczenia-OBD-CARS-CODE

## ESPHome: bilansowanie godzinowe grzałki

Plik `grzalka_bojlera_bilans_godzinowy.yaml` zawiera gotową konfigurację ESPHome dla grzałki bojlera z bilansowaniem godzinowym.

Założenia:

- liczniki importu i eksportu energii aktualizują się co około 30 sekund,
- na początku każdej godziny zapamiętywany jest stan liczników,
- dodatni `Bilans godzinowy energia` oznacza nadwyżkę oddaną do sieci w aktualnej godzinie,
- licznik bilansowania działa także przy wyłączonym trybie AUTO oraz w trybie MANUAL,
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
