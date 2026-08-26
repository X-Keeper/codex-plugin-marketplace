---
name: xkeeper-read-devices
description: "Чтение данных X-Keeper через MCP: устройства, настройки, история, статусы, геозоны, группы, списки, объекты и отчёты. Использовать для запросов про devices-api и устройства X-Keeper. Не использовать для изменения или удаления данных."
---

# Чтение устройств X-Keeper

## Авторизация

MCP авторизуется один раз при подключении плагина. Не запрашивать у пользователя код доступа MCP, сервисный ключ, raw-secret или пароль устройства.

Не запрашивать `apiKey` для:

- `devices_get`;
- `devices_filter_by_total_move_time`;
- `geozones_report_entry_one`;
- `motionless_report_get`;
- `raw_history_get`;
- `settings_get`;
- `silent_report_get`;
- `status_get`.

Пользовательский `apiKey` требуется только инструментам, в схеме которых есть обязательное поле `apiKey`: `devices_get_inverted_in_motion`, `geozones_get`, `groups_get`, `lists_get` и `panel_get_objects`. Передавать ключ без изменений, не повторять в ответе и не сохранять.

## Выбор инструмента

- `devices_get` — сведения об устройствах по SIM.
- `status_get` — актуальный статус по SIM.
- `settings_get` — настройки устройства.
- `raw_history_get` — сырые события xkdb.
- `xkdb_fields_reference` — расшифровка полей сырой истории.
- `geozones_get` и `geozones_report_entry_one` — геозоны и нахождение устройств в них.
- `groups_get`, `lists_get`, `panel_get_objects` — кабинетные группы, списки и объекты.
- `silent_report_get`, `motionless_report_get`, `devices_get_inverted_in_motion`, `devices_filter_by_total_move_time` — соответствующие отчёты.

`raw_history_get` и `settings_get` позволяют искать устройство по `device_hash`, `sim_primary`, `sim_second`, `imei`, `wifi_imei` или совместимой комбинации этих полей.

Для `raw_history_get` всегда передавать обе границы `filter.device_dt` в формате `YYYY-MM-DD HH:mm:ss`. Незнакомые поля ответа расшифровывать через `xkdb_fields_reference`.

## Ограничения

- Использовать только инструменты MCP X-Keeper, когда они подходят для запроса.
- Не вызывать изменяющие и удаляющие методы.
- Запрашивать только необходимые устройства и поля.
- Не скрывать поля ответа, включая пароль устройства, если пользователь явно запросил эти данные.
- При `401` или `403` сообщить об отказе доступа, не пытаться его обходить.
