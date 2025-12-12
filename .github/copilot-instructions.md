# GDP Dashboard - Copilot Instructions

## Architektur-Übersicht
Einfache Streamlit-basierte Datenvisualisierungsanwendung für weltweite GDP-Daten (1960-2022).

**Core-Komponenten:**
- `streamlit_app.py`: Haupt-Anwendung mit Streamlit-UI
- `data/gdp_data.csv`: GDP-Rohdaten (Country Code, Years 1960-2022)
- `requirements.txt`: Abhängigkeiten (streamlit, pandas)

## Datenfluss & Transformationen

**CSV → DataFrame → Pivot:**
```python
# Rohdaten: Country Code | 1960 | 1961 | ... | 2022
# Transformiert zu: Country Code | Year | GDP

gdp_df = raw_gdp_df.melt(
    ['Country Code'],
    [str(x) for x in range(MIN_YEAR, MAX_YEAR + 1)],
    'Year',
    'GDP'
)
```

**Caching-Pattern:**
```python
@st.cache_data  # Vermeidet wiederholtes CSV-Lesen
def get_gdp_data():
    DATA_FILENAME = Path(__file__).parent/'data/gdp_data.csv'
    return pd.read_csv(DATA_FILENAME)
```

## Entwickler-Workflows

**Lokales Setup:**
```bash
pip install -r requirements.txt
streamlit run streamlit_app.py
```

**App-Struktur:**
1. Page Config (Titel, Icon)
2. Datenladen mit Caching
3. User-Inputs (Country-Select, Year-Slider)
4. Datenfilterung & Aggregation
5. Visualisierung (Charts, Metrics)

## Konventionen

**Streamlit Best Practices:**
- `@st.cache_data` für teure Operationen
- `st.set_page_config()` am Anfang der Datei
- Emoji-Shortcodes für Icons (`:earth_americas:`)
- `Path(__file__).parent` für relative Dateipfade

**Datenverarbeitung:**
- pandas `melt()` für Pivot-Transformationen
- MIN_YEAR/MAX_YEAR als Konstanten
- Country Code als primärer Identifier

**Deployment:**
- Streamlit Community Cloud kompatibel
- Badge im README verlinkt auf gehostete App

## Integration Points

**Data Source:** CSV-Datei lokal, kann durch API-Endpoint ersetzt werden:
```python
# Aktuell:
DATA_FILENAME = Path(__file__).parent/'data/gdp_data.csv'
raw_gdp_df = pd.read_csv(DATA_FILENAME)

# API-Alternative:
# @st.cache_data(ttl='1d')
# def get_gdp_data():
#     return pd.read_json('https://api.example.com/gdp')
```

**Erweiterungsmöglichkeiten:**
- Zusätzliche Metriken (Inflation, Arbeitslosenquote)
- Mehrere Länder-Vergleiche gleichzeitig
- Interaktive Maps (plotly, folium)
- Export-Funktionen (CSV, Excel)
