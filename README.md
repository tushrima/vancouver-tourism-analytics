# Vancouver Tourism and Accommodation Analysis

A Python data analysis project exploring nearby accommodation, attractions, restaurants, and cuisines in the Vancouver area. It combines BC tourism datasets with OpenStreetMap amenities and uses GPS metadata from photographs to make location-based recommendations.

## Features

- **Nearby hotels:** Find three nearby accommodations based on a geotagged photo and display their locations on an interactive Folium map.
- **Places to visit:** List nearby food establishments and attractions.
- **Cuisine exploration:** Analyze cuisines in the surrounding area.
- **Restaurant density:** Generate maps comparing locations of chain and non-chain restaurants.
- **Route visualization:** Explore the tour path in a Jupyter notebook.

## Data and workflow

The project uses the included `hellobc-attractions-listing-20221205.csv`, `hellobc-accommodation-listing-20221205.csv`, and `amenities-vancouver.json` datasets. The cleaning scripts select records in a geographic bounding box around Vancouver, and `Combine_data.py` merges the processed records.

The recommendation scripts calculate geographic distances using a Haversine-based function in `Distance.py`. Image coordinates are read from EXIF GPS metadata by `Img_coord.py`.

## Requirements

Python 3 with the following third-party libraries:

```bash
python -m pip install pandas numpy Pillow folium plotly matplotlib seaborn scikit-learn notebook kaleido
```

Some plotting and data-processing code uses older library APIs. Compatibility with current library versions has not been verified.

## Running the analysis

Run commands from the repository root.

### 1. Prepare the datasets

```bash
python tourism_data_clean.py hellobc-attractions-listing-20221205.csv tourism_data.json
python Hotel_data_clean.py hellobc-accommodation-listing-20221205.csv hotel_data.json
python Combine_data.py
```

These scripts generate or refresh `tourism_data.json`, `hotel_data.json`, and `combined_data.json`.

**Compatibility note:** `Combine_data.py` uses the older `DataFrame.append()` API, which was removed in pandas 2.0. It may need a code update to run with newer pandas versions.

### 2. Recommend hotels and attractions

```bash
python Best_hotel.py IMG_0520.jpg
```

The input photo must contain GPS EXIF metadata. The script prints nearby hotel and attraction recommendations and saves an interactive map as `hotels.html`.

### 3. Explore cuisines

```bash
python cuisine.py IMG_0520.jpg
```

This script prints cuisine recommendations based on the photo's location. Its current implementation uses a 0.5 km distance threshold.

### 4. Generate restaurant density maps

```bash
python heat.py
```

The script attempts to write `Chain_rest.svg` and `NonChain_rest.svg`. Exporting static Plotly images may require compatible plotting dependencies and a working map tile source.

### 5. Explore the route notebook

Start Jupyter Notebook and open `path.ipynb`:

```bash
jupyter notebook
```

Run the notebook cells in order, checking any file paths and dependencies referenced within the notebook.

## Key files

| File | Purpose |
| --- | --- |
| `tourism_data_clean.py` | Clean and geographically filter attractions |
| `Hotel_data_clean.py` | Clean and geographically filter accommodation listings |
| `Combine_data.py` | Merge tourism, accommodation, and OSM amenity data |
| `Best_hotel.py` | Recommend hotels and nearby places; generate `hotels.html` |
| `Img_coord.py` | Read coordinates from photo EXIF metadata |
| `Distance.py` | Calculate Haversine distance |
| `cuisine.py` | Explore nearby cuisine categories |
| `heat.py` | Produce restaurant density maps |
| `path.ipynb` | Notebook for route visualization |
| `353 Report.pdf` | Project report |

## Limitations

This is a course project rather than a maintained travel-planning service. Results depend on the included historical datasets and the availability of photo GPS metadata. The EXIF reader assumes a particular GPS format, and several scripts may require updates for newer versions of pandas or Plotly. The recommendations are based on geographic proximity and dataset categories; they do not verify business hours, live availability, or actual travel times.
