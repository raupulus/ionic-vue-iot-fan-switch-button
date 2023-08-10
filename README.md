# ionic-vue-iot-fan-switch-button

## Preparar variables

Antes de comenzar copiar el archivo de variables de entorno:

```bash
cp src/environment.example.ts src/environment.ts
```

## Construir para android

```bash
npx cap add android
ionic capacitor build android --prod
```

## Abrir aplicación con android studio

```bash
npx cap open android --prod
```

## Endponts

## Pedir estado de temperatura

http://172.18.0.2:8123/api/states/sensor.esphome_web_827e67_bme280_temperature

Devuelve:

```json
{
  "entity_id": "sensor.esphome_web_827e67_bme280_temperature",
  "state": "31.4",
  "attributes": {
    "state_class": "measurement",
    "unit_of_measurement": "°C",
    "device_class": "temperature",
    "friendly_name": "esp8266-test BME280 Temperature"
  },
  "last_changed": "2023-08-06T19:20:46.193142+00:00",
  "last_updated": "2023-08-06T19:20:46.193142+00:00",
  "context": {
    "id": "01H764PYVH9NVT18PBND9WMF0Q",
    "parent_id": null,
    "user_id": null
  }
}
```

## Pedir estado de presión

http://172.18.0.2:8123/api/states/sensor.esphome_web_827e67_bme280_pressure

Devuelve:

```json
{
  "entity_id": "sensor.esphome_web_827e67_bme280_pressure",
  "state": "1012.4",
  "attributes": {
    "state_class": "measurement",
    "unit_of_measurement": "hPa",
    "device_class": "pressure",
    "friendly_name": "esp8266-test BME280 Pressure"
  },
  "last_changed": "2023-08-06T19:58:36.208384+00:00",
  "last_updated": "2023-08-06T19:58:36.208384+00:00",
  "context": {
    "id": "01H766W7NGG7XAAYZ2WFGXZSF6",
    "parent_id": null,
    "user_id": null
  }
}
```

## Pedir estado del relé (GET)

http://172.18.0.2:8123/api/states/switch.esphome_web_827e67_rel_0

Devuelve:

```json
{
  "entity_id": "switch.esphome_web_827e67_rel_0",
  "state": "off",
  "attributes": {
    "friendly_name": "esp8266-test Relé (0)"
  },
  "last_changed": "2023-08-06T17:07:46.318161+00:00",
  "last_updated": "2023-08-06T17:07:46.318161+00:00",
  "context": {
    "id": "01H75X3DZZKS91STS7DB4F4TAR",
    "parent_id": null,
    "user_id": "03bc082291ba43bfb3487bd6d16fad19"
  }
}
```

## Apagar relé (POST)

http://172.18.0.2:8123/api/services/switch/turn_off
{"entity_id": "switch.esphome_web_827e67_rel_0"}

## Encender relé (POST)

http://172.18.0.2:8123/api/services/switch/turn_on
{"entity_id": "switch.esphome_web_827e67_rel_0"}
