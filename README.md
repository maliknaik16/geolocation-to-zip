# US Zipcode Finder

A JavaScript script that efficiently determines the nearest US zipcode based on latitude and longitude coordinates using k-means clustering and optimization techniques.

## Overview

This library uses a two-step process to efficiently find the closest zipcode to any given geographical coordinate in the United States:

1. First, it determines which geographical cluster the coordinates belong to using pre-computed centroids
2. Then, it searches within that cluster's zipcodes to find the nearest match

This approach significantly reduces the search space and improves performance compared to searching through all US zipcodes.

## Installation

Include the JavaScript file in your HTML:

```html
<script src="zipcode-finder.js"></script>
```

## Usage

```javascript
// Using the Geolocation API
navigator.geolocation.getCurrentPosition(function(position) {
    var zipcode = whatIsMyZipcode(position);
    console.log("Your estimated zipcode is: " + zipcode);
});

// Or with known coordinates
var mockPosition = {
    coords: {
        latitude: 37.7749,
        longitude: -122.4194
    }
};
var zipcode = whatIsMyZipcode(mockPosition);
```

## API Reference

### `whatIsMyZipcode(position)`
Main function to get the estimated zipcode.
- **Parameters**: `position` - A Geolocation API position object containing coords.latitude and coords.longitude
- **Returns**: String - The estimated zipcode

### `getCluster(point)`
Determines which geographical cluster the coordinates belong to.
- **Parameters**: `point` - Array of [latitude, longitude]
- **Returns**: String - Cluster identifier

### `estimateZipcode(coords, cluster)`
Finds the nearest zipcode within a specific cluster.
- **Parameters**:
  - `coords` - Array of [latitude, longitude]
  - `cluster` - Cluster identifier string
- **Returns**: String - The nearest zipcode

### `computeDistance(x1, x2, y1, y2)`
Calculates the distance between two points using the Haversine formula.
- **Parameters**: Two sets of coordinates (x1,y1) and (x2,y2)
- **Returns**: Number - Distance in miles

## Data Format

The library expects zipcode data in the following JSON format:

```javascript
{
    "clusterID": {
        "zipcode": [latitude, longitude]
    }
}
```

Example:
```javascript
{
    "34": {
        "00601": [18.180555, -66.749961]
    }
}
```

## Technical Details

- Uses k-means clustering with 50 centroids across the US
- Distances are calculated using the Haversine formula
- Coordinates are in decimal degrees (DD) format
- Optimized for browser environments
- Includes special handling for non-continental areas (Alaska, Hawaii, Puerto Rico, Guam)

## Limitations

- Accuracy depends on the quality of the source data
- May not include all US territories
- Results are estimates based on geographical proximity
- Does not account for postal service boundaries
