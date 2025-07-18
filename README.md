# pyroutes
Open source python-based alternative to the Google Routes Api. Scrapes open-source data such as OpenStreetMap and region-based transport APIs (ie. TransportNSW API, Transport for London Journey API) to provide travel data for the specified mode of travel (drive, transit, walk).

## Features (Planned)
- `GET /lat-lng`: returns coordinates given a place query
- `GET /directions`: point-to-point driving, walking, transit directions
- `POST /optimize-route`: multi-point route optimization, with the ability to specify start and end points
- `GET /distance-matrix`: estimate travel time and distance between points
- Modular backend with pluggable routing engines (e.g. OSRM, GraphHopper, custom Dijkstra/A*)

## Tech Stack
- **Backend**: FastAPI / Flask
- **Routing engine**: Custom graph logic, or external via wrappers
- **Data**: OpenStreetMap and region-based transport APIs (list in progress)
- **Testing**: pytest

## Getting Started
```bash
git clone https://github.com/yourusername/openroutes-py
cd pyroutes
pip install -r requirements.txt
uvicorn app.main:app --reload
```

## Usage

### Directions
**Request**
```
GET http://localhost:8000/directions?origin=-33.8675,151.2070&destination=-33.8688,151.2093&mode=DRIVE
```
**curl**
```
curl "http://localhost:8000/directions?origin=-33.8675,151.2070&destination=-33.8688,151.2093&mode=DRIVE"
```
**Response**
```json
{
  "origin": "-33.8675,151.2070",
  "destination": "-33.8688,151.2093",
  "mode": "DRIVE",
  "duration_minutes": 2,
  "distance_km": 0.3,
  "steps": [
    "Head north on George St",
    "Turn right onto Park St",
    "Turn left onto Elizabeth St",
    "Destination will be on the right"
  ]
}
```

### Optimise Route
**Request**
```
POST http://localhost:8000/optimize-route
Content-Type: application/json

{
  "locations": [
    "-33.8675,151.2070",
    "-33.8688,151.2093",
    "-33.8700,151.2100"
  ],
  "mode": "WALK",
  "start": 0,
  "end": 1
}
```
**Response**
```json
{
  "optimized_order": [
    "-33.8675,151.2070",
    "-33.8700,151.2100",
    "-33.8688,151.2093"
  ],
  "total_distance_km": 1.0,
  "estimated_time_min": 20
}
```
