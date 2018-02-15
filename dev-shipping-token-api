# dev-shipping-token-api

## What does this endpoint do?

Given a shipping token, the endpoint will return all data regarding the delivery location.

##Specification

The current root URL of the API is: `https://fv5s0y1520.execute-api.us-east-1.amazonaws.com/dev/`

### API KEYS

To get access to the endpoints described below, you need to have an Exaactly API KEY.
This key needs to be passed along with **every request** in the `x-api-key` header.

### Throttling / Rate Limiting

All demo users are subjected to the following limitations:

- 1 request per second
- 1000 requests per month


### `/delivery/{token}`

**Method:** `GET`

**URL Parameters:**

| name | type | required | description |
| ---- | ---- | -------- | ----------- |
| token | string | yes | the shipping token id |
 
 
#### Responses

##### 200 - Success

```
{
  "token": "",              // String   - The shipping token id
  "location": {
    "accessType": "",       // String   - Describes the type of access (stairs or lift)
    "notes": "",            // String   - Notes about any relevant information that helps to locate the entrance.
    "name": "",             // String   - Name for the location as given by the user.
    "floor": 0,             // Integer  - Floor level.
    "type": "",             // String   - Building type. It has three possible values: House/Flat/Commercial
    "traditionalAddress": { // Object   - The traditional address for the property
      "postCode": "",       // String   - Postcode for the location
      "line1": "",          // String   - Building number and street name
      "line2": "",          // String   - Organisation name and/or building name
      "city": ""            // String   - City
    },
    "coordinates": {        // Object   - Lat and Lng for the given location
      "lat": 0.0,           // Float    - The latitude of the location
      "lng": 0.0            // Float    - The longitude of the location
    },
    "streetView": {         // Object   - Data of the street view panorama as per Google's specification, collected by tracking user's selection of where the entrance to the property is located.
      "zoom": 0.0,          // Float    - The street view panorama zoom value
      "position": {         // Object   - Lat and Lng for the selected street view panorama
        "lng": 0.0,         // Float    - The latitude of the street view panorama location
        "lat": 0.0          // Float    - The longitude of the street view panorama location
      },
      "pov": {              // Object   - The point of view of the street view panorama
        "heading": 0.0,     // Float    - The heading of the PoV of the panorama
        "pitch": 0.0        // Float    - The pitch of the PoV of the panorama
      }
    }
  }
}
```

##### 403 - Forbidden
This happens when you are not authorised to view the endpoint. Most common mistake is that the `x-api-key` header has not been set correctly.

##### 404 - Not Found

```json
{
  "error": "resource not found"
}
```
