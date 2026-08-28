# Weather-agent-
"Python-based weather &amp; air quality agent using free APIs (Nominatim, Open-Meteo, OpenAQ) with CSV export."

# 🌦️ Weather Forecasting Agent

A simple Python agent that fetches **real-time weather, location, and air quality data** for any city using free, no-API-key-required public APIs — and saves the results to a CSV file.

---

## ✨ Features

- 🌍 **City → Coordinates**: Converts any city name into latitude/longitude
- 🌡️ **Live Weather**: Fetches current temperature, humidity, and wind speed
- 🌫️ **Air Quality**: Fetches PM2.5 air quality data
- 📊 **Data Combination**: Merges all data into a single clean record per city
- 💾 **CSV Export**: Automatically saves results to `weather_data.csv`

---

## ⚙️ How It Works

The notebook builds the pipeline step by step:

1. **`get_coordinates(city_name)`** — Uses the **Nominatim (OpenStreetMap)** API to convert a city name into latitude/longitude.
2. **`get_weather(city_name)`** — Uses **Open-Meteo** with those coordinates to fetch temperature, humidity, and wind speed.
3. **`get_air_quality(city_name)`** — Uses **OpenAQ** to fetch PM2.5 air quality data.
4. **`get_all_data(city_name)`** — Combines the results of the above three functions into one dictionary.
5. **Batch loop** — Runs `get_all_data()` across a list of cities and saves everything to `weather_data.csv`.

---

## 📁 Project Structure

```
├── agent_for_weather_forecasting.ipynb   # Main notebook with all logic
├── weather_data.csv                       # Sample output data
├── requirements.txt                       # Python dependencies
├── _env                                    # Environment variables
└── README.md                              # Project documentation
```

---

## 🔌 APIs Used

| API | Purpose | API Key Required |
|---|---|---|
| [Nominatim](https://nominatim.openstreetmap.org/) (OpenStreetMap) | City name → coordinates | ❌ No |
| [Open-Meteo](https://open-meteo.com/) | Temperature, humidity, wind speed | ❌ No |
| [OpenAQ](https://openaq.org/) | Air quality (PM2.5) | ❌ No |

> **Note:** An `OPENWEATHER_API_KEY` variable exists in the `.env` file for potential future use (e.g. switching to OpenWeatherMap), but it is **not currently used** anywhere in the notebook.

---

## 🚀 Installation

1. Clone or download this project.
2. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. (Optional) Set up your own `.env` file if you plan to add API-key-based services later.

---

## ▶️ Usage

1. Open `agent_for_weather_forecasting.ipynb` in Jupyter Notebook / JupyterLab.
2. Run all cells in order.
3. Edit the `cities` list to check different locations:
   ```python
   cities = ["London", "Tokyo", "Lahore", "Paris"]
   ```
4. Results are saved automatically to `weather_data.csv`.

---

## 📄 Sample Output

| city | temperature | humidity | wind_speed | pm25 |
|---|---|---|---|---|
| Greater London, England, UK | 19.4 | 78 | 16.9 | N/A |
| 東京都, 日本 (Tokyo) | 24.6 | 81 | 2.6 | N/A |
| Lahore, Punjab, Pakistan | 32.4 | 63 | 4.1 | N/A |
| Paris, Île-de-France, France | 21.7 | 56 | 7.5 | N/A |

---

## ⚠️ Known Limitations

- **PM2.5 returns "N/A"** for all cities currently — the OpenAQ `/v1/latest` endpoint used here is deprecated. Consider migrating to the **OpenAQ v3 API** (requires a free API key) for live air-quality readings.
- **Nominatim rate limit**: max 1 request/second — add a small delay (`time.sleep(1)`) between calls if checking many cities.
- No retry/error-handling logic yet if an API call fails mid-loop.

---

## 🔒 Security Note

Your `_env` file currently contains a **live-looking API key** in plain text. Before uploading this project anywhere public (e.g. GitHub):
- Add `_env` / `.env` to your `.gitignore` file so it's never committed.
- If this key has already been shared or pushed publicly, **regenerate/rotate it** on the provider's dashboard.
- Never hardcode real keys directly into notebooks or commit history.

---

## 🔮 Future Improvements

- [ ] Add retry logic and better error handling
- [ ] Migrate to OpenAQ v3 for real PM2.5 values
- [ ] Add scheduling (e.g. `cron` / `APScheduler`) to auto-run daily
- [ ] Visualize trends with `matplotlib` / `plotly`
- [ ] Turn this into a CLI tool or simple web dashboard

---

## 🛠️ Tech Stack

- Python 3
- `requests` — API calls
- `pandas` — data handling & CSV export
- Jupyter Notebook
