source: API/Alerts.md

### `get_alert`

Get details of an alert

Route: `/api/v0/alerts/:id`

  - id is the alert id, you can obtain a list of alert ids from [`list_alerts`](#function-list_alerts).

Input:

  -

Example:
```curl
curl -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/alerts/1
```

Output:
```json
{
 "status": "ok",
 "err-msg": "",
 "count": 7,
 "alerts": [
  {
   "hostname": "localhost",
   "id": "1",
   "device_id": "1",
   "rule_id": "1",
   "state": "1",
   "alerted": "1",
   "open": "1",
   "timestamp": "2014-12-11 14:40:02"
  },
}
```

### `ack_alert`

Acknowledge an alert

Route: `/api/v0/alerts/:id`

  - id is the alert id, you can obtain a list of alert ids from [`list_alerts`](#function-list_alerts).

Input:

  -

Example:
```curl
curl -X PUT -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/alerts/1
```

Output:
```json
{
 "status": "ok",
 "err-msg": "",
 "message": "Alert has been acknowledged"
}
```

### `unmute_alert`

Unmute an alert

Route: `/api/v0/alerts/unmute/:id`

  - id is the alert id, you can obtain a list of alert ids from [`list_alerts`](#function-list_alerts).

Input:

  -

Example:
```curl
curl -X PUT -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/alerts/unmute/1
```

Output:
```json
{
 "status": "ok",
 "err-msg": "",
 "message": "Alert has been unmuted"
}
```


### `list_alerts`

List all alerts

Route: `/api/v0/alerts`

Input:

  - state: Filter the alerts by state, 0 = ok, 1 = alert, 2 = ack

Example:
```curl
curl -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/alerts?state=1
```

Output:
```json
{
 "status": "ok",
 "err-msg": "",
 "count": 1,
 "alerts": [
  {
   "id": "1",
   "device_id": "1",
   "rule_id": "1",
   "state": "1",
   "alerted": "1",
   "open": "1",
   "timestamp": "2014-12-11 14:40:02"
  }
}
```

## Rules

### `get_alert_rule`

Get the alert rule details.

Route: `/api/v0/rules/:id`

  - id is the rule id.

Input:

  -

Example:
```curl
curl -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/rules/1
```

Output:
```json
{
 "status": "ok",
 "err-msg": "",
 "count": 1,
 "rules": [
  {
   "id": "1",
   "device_id": "1",
   "rule": "%devices.os != \"Juniper\"",
   "severity": "warning",
   "extra": "{\"mute\":true,\"count\":\"15\",\"delay\":null,\"invert\":false}",
   "disabled": "0",
   "name": "A test rule"
  }
 ]
}
```

### `delete_rule`

Delete an alert rule by id

Route: `/api/v0/rules/:id`

  - id is the rule id.

Input:

  -

Example:
```curl
curl -X DELETE -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/rules/1
```

Output:
```json
{
 "status": "ok",
 "err-msg": "",
 "message": "Alert rule has been removed"
}
```

### `list_alert_rules`

List the alert rules.

Route: `/api/v0/rules`

  -

Input:

  -

Example:
```curl
curl -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/rules
```

Output:
```json
{
 "status": "ok",
 "err-msg": "",
 "count": 1,
 "rules": [
  {
   "id": "1",
   "device_id": "-1",
   "rule": "%devices.os != \"Juniper\"",
   "severity": "critical",
   "extra": "{\"mute\":false,\"count\":\"15\",\"delay\":\"300\",\"invert\":false}",
   "disabled": "0",
   "name": "A test rule"
  },
}
```

### `add_rule`

Add a new alert rule.

Route: `/api/v0/rules`

  -

Input (JSON):

  - device_id: This is either the device id or -1 for a global rule
  - rule: The rule which should be in the format %entity $condition $value (i.e %devices.status != 0 for devices marked as down).
  - severity: The severity level the alert will be raised against, Ok, Warning, Critical.
  - disabled: Whether the rule will be disabled or not, 0 = enabled, 1 = disabled
  - count: This is how many polling runs before an alert will trigger and the frequency.
  - delay: Delay is when to start alerting and how frequently. The value is stored in seconds but you can specify minutes, hours or days by doing 5 m, 5 h, 5 d for each one.
  - mute: If mute is enabled then an alert will never be sent but will show up in the Web UI (true or false).
  - invert: This would invert the rules check.
  - name: This is the name of the rule and is mandatory.

Example:
```curl
curl -X POST -d '{"device_id":"-1", "rule":"%devices.os != \"Cisco\"","severity": "critical","count":15,"delay":"5 m","mute":false}' -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/rules
```

Output:
```json
rules
{
 "status": "ok",
 "err-msg": ""
}
```

### `edit_rule`

Edit an existing alert rule

Route: `/api/v0/rules`

  -

Input (JSON):

  - rule_id: You must specify the rule_id to edit an existing rule, if this is absent then a new rule will be created.
  - device_id: This is either the device id or -1 for a global rule
  - rule: The rule which should be in the format %entity $condition $value (i.e %devices.status != 0 for devices marked as down).
  - severity: The severity level the alert will be raised against, Ok, Warning, Critical.
  - disabled: Whether the rule will be disabled or not, 0 = enabled, 1 = disabled
  - count: This is how many polling runs before an alert will trigger and the frequency.
  - delay: Delay is when to start alerting and how frequently. The value is stored in seconds but you can specify minutes, hours or days by doing 5 m, 5 h, 5 d for each one.
  - mute: If mute is enabled then an alert will never be sent but will show up in the Web UI (true or false).
  - invert: This would invert the rules check.
  - name: This is the name of the rule and is mandatory.

Example:
```curl
curl -X PUT -d '{"rule_id":1,"device_id":"-1", "rule":"%devices.os != \"Cisco\"","severity": "critical","count":15,"delay":"5 m","mute":false}' -H 'X-Auth-Token: YOURAPITOKENHERE' https://librenms.org/api/v0/rules
```

Output:
```json
rules
{
 "status": "ok",
 "err-msg": ""
}
```