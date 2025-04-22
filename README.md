# Smart Greeting Generator

A highly customizable Home Assistant script that creates dynamic, context-aware greeting messages for notifications.

## ✨ Features

- **Time-of-day greetings**: Morning, afternoon, evening, early morning, and late night.
- **Specific-day greetings**: Custom greetings for holidays or special dates (e.g. "12-25" for Christmas).
- **Weekday-specific messages**: Unique greetings for each day of the week.
- **"Been a while" detection**: Adds a special message if it’s been more than 48 hours since your last notification.
- **Cooldowns**: Prevents overuse of greeting types by defining time-based cooldowns.
- **Rotation tracking**: Keeps greetings fresh by avoiding repetition.
- **Randomization**: Introduces chance-based weekday messages so they're not always predictable.

---

## 🏗️ Helpers Required

You must create the following Helpers in Home Assistant:

### Text Helpers (`input_text`)
- `input_text.latest_notification_greeting`
- `input_text.used_base_greetings`
- `input_text.used_extra_greetings`
- `input_text.last_weekday_greeting_day`

### Date/Time Helper (`input_datetime`)
- `input_datetime.last_notification_time`

---

## ⚙️ Configuration

The script supports:

### Cooldown Settings (in seconds)
```yaml
base_greeting_cooldown: 21600   # 6 hours
extra_greeting_cooldown: 172800  # 48 hours
```

### Specific Day Greetings
Use date format `"MM-DD"` for key names.
```yaml
specific_day_greetings:
  "01-01":
    - "Happy New Year!"
    - "Wishing you a fresh start!"
  "12-25":
    - "Merry Christmas!"
    - "Hope your holidays are bright!"
```

### Weekday Greetings
```yaml
weekday_pools:
  monday:
    - "Happy Monday!"
    - "Let’s start the week strong!"
  ...
```

### Time-of-Day Pools
Five segments:
- Late Night (`00:00–03:59`)
- Early Morning (`04:00–07:59`)
- Morning (`08:00–11:59`)
- Afternoon (`12:00–17:59`)
- Evening (`18:00–23:59`)

---

## 🔔 Example Automation
Trigger the script and send a notification:
```yaml
alias: Morning Notification
trigger:
  - platform: time
    at: "08:00:00"
action:
  - service: script.smart_greeting_generator
  - delay: "00:01"
  - variables:
      greeting: "{{ states('input_text.latest_notification_greeting') | trim }}"
  - condition: template
    value_template: "{{ greeting != '' }}"
  - service: notify.mobile_app_YOUR_DEVICE_ID
    data:
      title: "Daily Update"
      message: "{{ greeting }}"
```

---

## 📁 File Overview

- `smart_greeting_generator.yaml` – The main script
- `smart_greeting_notify.yaml` – A helper script to streamline notifications
- `README.md` – You are here

---

## 📜 License
MIT — Free to use, modify, and share.



