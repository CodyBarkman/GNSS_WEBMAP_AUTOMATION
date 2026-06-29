# GNSS\_WEBMAP\_AUTOMATION

Automated ETL pipeline integrating Azure Blob Storage, Python, and ArcGIS Online to manage and publish a real-time NPS GNSS monitoring station network. Includes coordinate conversion, spatial analysis, and photo gallery automation.



\# NPS GNSS Station Map Pipeline



Automated ETL pipeline that ingests real-time GNSS monitoring station data from Azure Blob Storage, performs coordinate transformation and spatial analysis, and publishes a live hosted feature layer to ArcGIS Online for use in an NPS public-facing Experience Builder web application.



\## Project Overview



The National Park Service maintains a network of GNSS (Global Navigation Satellite System) monitoring stations across DOI-managed lands. This pipeline automates the weekly ingestion, transformation, and publication of station data, including:



\- ECEF XYZ → WGS84 lat/lon coordinate conversion

\- Reference frame derivation from tectonic plate classification (NAD83(2011), NAD83(PA11), NAD83(MA11))

\- Spatial join against authoritative NPS park boundary data

\- Operational status tracking — preserving retired stations as historical records

\- Photo gallery automation via Azure Blob Storage



\## Architecture



\## Technologies



\- Python (arcgis, pandas, requests)

\- ArcGIS Online / Experience Builder

\- ArcGIS Pro Notebooks

\- Azure Blob Storage

\- NPS Authoritative Boundary Data (AGOL)



\## Notebooks



\### GNSS\_Station\_Update\_ETL.ipynb

Main ETL pipeline. Runs weekly to:

\- Fetch latest station CSV from Azure Blob

\- Parse and clean data

\- Convert ECEF coordinates to WGS84

\- Derive reference frame from tectonic plate

\- Run spatial join against NPS park boundaries

\- Add new stations, update existing, flag retired stations



\### Photo\_Blob\_Integration.ipynb

Photo management pipeline. Runs as needed when new photos are added:

\- Probes Azure Blob Storage for station photos using standardized naming convention (STATIONCODE01.jpg etc.)

\- Updates photo\_url, photo\_count, and photo\_html fields in feature layer

\- Pre-builds HTML gallery string for each station for direct popup rendering



\## Setup



1\. Install dependencies:



2\. Configure Cell 2 in each notebook:

\- `FEATURE\_LAYER\_ITEM\_ID` — your AGOL hosted feature layer item ID - Left blank will create a new feature service, Item ID will overwrite with update

\- `BLOB\_URL` / `BLOB\_BASE` — your Azure Blob Storage URL

\- `AGOL\_URL` — your ArcGIS Online org URL



3\. Run notebooks top to bottom in ArcGIS Pro or ArcGIS Notebooks



\## Photo Naming Convention



Station photos must follow this naming convention to be recognized by the pipeline:



EXAM01.jpg

EXAM02.jpg

EXAM03.jpg



Where `EXAM` is the 4-character station identifier matching `m\_StationCode` in the feature layer.



\## Notes



\- Retired stations are preserved in the feature layer with `operational\_status = "No Longer in Service"`

\- Park boundary spatial join uses the NPS Land Resources Division authoritative boundary service

\- Photo gallery is rendered via pre-built HTML stored in the `photo\_html` field — no AGOL storage credits consumed

\- Coordinate conversion uses WGS84 ellipsoid parameters with 10-iteration convergence

