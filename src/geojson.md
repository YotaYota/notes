# GeoJson

GeoJSON is a format for encoding a variety of geographic data structures.

```
{
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [125.6, 10.1]
  },
  "properties": {
    "name": "Dinagat Islands"
  },
  bbox?: [number, ...] // optional
}
```

Geometry types supported:

- `Point`
- `LineString`
- `Polygon`
- `MultiPoint`
- `MultiLineString`
- `MultiPolygon`

Geometric objects with additional properties are `Feature` objects. Sets of
features are contained by `FeatureCollection` objects.

```
{
  "type":"FeatureCollection",
  "features":[
    {
      "type":"Feature",
      "geometry":{
        "type":"Point",
        "coordinates":[ 102.0, 0.5 ]
      },
      "properties":{
        "prop0":"value0"
      }
    },
    {
      "type":"Feature",
      "geometry":{
        "type":"LineString",
        "coordinates":[
          [ 102.0, 0.0 ],
          [ 103.0, 1.0 ],
          [ 104.0, 0.0 ],
          [ 105.0, 1.0 ]
        ]
      },
      "properties":{
        "prop0":"value0",
        "prop1":0.0
      }
    },
    {
      "type":"Feature",
      "geometry":{
        "type":"Polygon",
        "coordinates":[
          [
            [ 100.0, 0.0 ],
            [ 101.0, 0.0 ],
            [ 101.0, 1.0 ],
            [ 100.0, 1.0 ],
            [ 100.0, 0.0 ]
          ]
        ]
      },
      "properties":{
        "prop0":"value0",
        "prop1":{
          "this":"that"
        }
      }
    }
  ]
}
```

## LineString

```
{
  "type": "LineString",
  "coordinates": [
    [longitude1, latitude1],
    [longitude2, latitude2],
    [longitude3, latitude3],
    ...
  ],
  "bbox": [west, south, east, north],
  "properties": {...},
  "id": "..."
}
```
