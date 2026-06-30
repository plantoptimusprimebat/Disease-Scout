# 🌾 Sugarcane Disease Scout

A Streamlit app for sugarcane farmers to identify leaf diseases using the PlantNet API, with automatic GPS location capture and weather data collection for building disease prediction models.

## Features

### For Farmers
- **📷 Take or upload photos** of sugarcane leaves showing disease symptoms
- **🦠 AI-powered disease identification** via PlantNet's Diseases & Pests model
- **📍 Automatic GPS location** capture (or paste a Google Maps link)
- **🌿 Organ selection** — specify which plant part is shown (leaf, fruit, flower, bark)

### For Analysts (Backend)
- **📊 Admin dashboard** in the sidebar with password protection
- **⬇️ CSV export** of all submissions including:
  - Disease identified + confidence score
  - GPS coordinates (lat/lon)
  - Full weather data for the submission date:
    - Temperature (mean, max, min)
    - Relative humidity (%)
    - Precipitation (mm) and probability
    - Wind speed and gust (km/h), direction
    - Atmospheric pressure (hPa)
    - Cloud cover (%), UV index
    - Dew point, visibility, conditions description

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/sugarcane-disease-scout.git
cd sugarcane-disease-scout
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# or
venv\Scripts\activate  # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure API keys

Copy the example environment file and add your keys:

```bash
cp .env.example .env
```

Edit `.env` with your actual keys:
- **PlantNet API key**: Sign up at [my.plantnet.org](https://my.plantnet.org/)
- **Visual Crossing API key**: Sign up at [visualcrossing.com](https://www.visualcrossing.com/sign-up)

Alternatively, for deployment on Streamlit Cloud, add keys to `.streamlit/secrets.toml`:

```toml
PLANTNET_API_KEY = "your_key_here"
VISUALCROSSING_API_KEY = "your_key_here"
ADMIN_PASSWORD = "your_admin_password"
```

### 5. Run the app

```bash
streamlit run app.py
```

## Deployment on Streamlit Cloud

1. Push this repo to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io/)
3. Connect your GitHub repo
4. Add your secrets in the Streamlit Cloud dashboard under **Settings > Secrets**:

```toml
PLANTNET_API_KEY = "your_key"
VISUALCROSSING_API_KEY = "your_key"
ADMIN_PASSWORD = "your_secure_password"
```

## Project Structure

```
sugarcane-disease-scout/
├── app.py                 # Main Streamlit application
├── requirements.txt       # Python dependencies
├── .env.example           # Template for environment variables
├── .gitignore             # Git ignore rules
├── README.md              # This file
└── data/
    └── submissions.csv    # Auto-generated submission records (gitignored)
```

## How the Data Collection Works

Each time a farmer submits a photo:

1. **Image → PlantNet API** — returns disease identification with confidence scores
2. **Location** — captured via browser GPS or extracted from Google Maps link
3. **Location + Date → Visual Crossing API** — returns weather conditions for that day
4. **All data → `data/submissions.csv`** — appended as a new row

This CSV accumulates over time and can be used to build disease risk models that correlate:
- Disease occurrence (type, frequency)
- Geographic distribution (lat/lon clustering)
- Weather conditions (temp, humidity, rainfall patterns)

## Future Enhancements

- [ ] Connect to a cloud database (PostgreSQL/Supabase) instead of CSV
- [ ] Add historical submission map view
- [ ] Disease risk model integration
- [ ] Push notifications when high-risk conditions detected
- [ ] Multi-language support (Zulu, Afrikaans)

## License

MIT
