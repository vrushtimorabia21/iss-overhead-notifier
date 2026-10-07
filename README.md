# ISS Overhead Notifier

A Python script that checks every 60 seconds whether the International Space Station is within 5 degrees of your location and whether it's dark outside. It uses the Open-Notify and Sunrise-Sunset APIs via `requests`, and when both conditions are met, it emails you an alert using `smtplib` so you can look up.

## Features

- Fetches the live ISS position from the Open-Notify API
- Checks whether it's currently night at your location using the Sunrise-Sunset API
- Sends an email alert when the ISS is overhead and the sky is dark
- Runs continuously, checking once every 60 seconds

## Requirements

- Python 3.x
- [`requests`](https://pypi.org/project/requests/)
- A Gmail account with an [app password](https://support.google.com/accounts/answer/185833) (required for SMTP login)

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/iss-overhead-notifier.git
   cd iss-overhead-notifier
   ```

2. Install the dependency:

   ```bash
   pip install requests
   ```

3. Set your credentials as environment variables (never hard-code your password):

   **macOS / Linux**
   ```bash
   export MY_EMAIL="your_email@gmail.com"
   export MY_PASSWORD="your_app_password"
   ```

   **Windows (PowerShell)**
   ```powershell
   $env:MY_EMAIL="your_email@gmail.com"
   $env:MY_PASSWORD="your_app_password"
   ```

4. Update the script to read them:

   ```python
   import os

   MY_EMAIL = os.environ["MY_EMAIL"]
   MY_PASSWORD = os.environ["MY_PASSWORD"]
   ```

5. Set your own coordinates in the script:

   ```python
   MY_LAT = 51.507351   # your latitude
   MY_LONG = -0.127758  # your longitude
   ```

## Usage

Run the script:

```bash
python main.py
```

Leave it running. When the ISS passes overhead at night, you'll receive an email with the subject **"Look Up 👆"**.

## How It Works

1. `is_iss_overhead()` requests the ISS's current latitude and longitude and returns `True` if it's within ±5 degrees of your position.
2. `is_night()` requests today's sunrise and sunset times for your location and returns `True` if the current hour is outside daylight hours.
3. A `while True` loop runs both checks every 60 seconds.
4. If both are `True`, the script connects to Gmail's SMTP server over TLS and sends the alert email.

## Project Structure

```
iss-overhead-notifier/
├── main.py
└── README.md
```

## Notes

- Sunrise and sunset times from the API are returned in UTC, while `datetime.now()` uses your local time. If you're not in a UTC-equivalent timezone, the night check may be off by a few hours.
- Gmail requires an app password (with 2-step verification enabled) rather than your regular account password.

## Concepts Practiced

- Working with REST APIs using `requests`
- Parsing JSON responses
- Handling dates and times with `datetime`
- Sending email with `smtplib`
- Running a continuous background loop with `time.sleep`

## Credits

- ISS location data from [Open-Notify](http://open-notify.org/)
- Sunrise and sunset data from [Sunrise-Sunset.org](https://sunrise-sunset.org/api)

## License

This project is open source and available under the [MIT License](LICENSE).
