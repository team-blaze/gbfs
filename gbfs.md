# General Bikeshare Feed Specification (GBFS)

This document explains the types of files and data that comprise the General Bikeshare Feed Specification (GBFS) used by Beryl and defines the fields used in all of those files.

# Reference version

This documentation refers to **v2.1-RC (release candidate)**.

## Table of Contents

- [Introduction](#introduction)
- [Active Systems (Beryl)](#active-systems-beryl)
- [Term Definitions](#term-definitions)
- [Files](#files)
- [File Requirements](#file-requirements)
- [Field Types](#field-types)
  - [vehicle](#vehicle)
  - [gbfs.json](#gbfsjson)
  - [system_information.json](#system_informationjson)
  - [vehicle_types.json](#vehicle_typesjson)
  - [station_information.json](#station_informationjson)
  - [station_status.json](#station_statusjson)
  - [free_bike_status.json](#free_bike_statusjson)
  - [system_regions.json](#system_regionsjson)
  - [system_pricing_plans.json](#system_pricing_plansjson)
  - [geofencing_zones.json](#geofencing_zonesjson) _(TBC)_

## Introduction

This specification has been designed with the following concepts in mind:

- Provide the status of the system at this moment
- Do not provide information whose primary purpose is historical

The specification supports real-time travel advice in GBFS-consuming applications.

## Active Systems (Beryl)

The table below lists the active systems Beryl maintains.

| System Name   | Location                             | URL                                                                                                  |
| ------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| BCP           | Bournemouth, Christchurch, Poole, GB | [https://gbfs.beryl.cc/v2/BCP/gbfs.json](https://gbfs.beryl.cc/v2/BCP/gbfs.json)                     |
| Hereford      | Hereford, GB                         | [https://gbfs.beryl.cc/v2/Hereford/gbfs.json](https://gbfs.beryl.cc/v2/Hereford/gbfs.json)           |
| Isle of Wight | Isle of Wight, GB                    | [https://gbfs.beryl.cc/v2/Isle_of_Wight/gbfs.json](https://gbfs.beryl.cc/v2/Isle_of_Wight/gbfs.json) |
| London        | London, GB                           | [https://gbfs.beryl.cc/v2/London/gbfs.json](https://gbfs.beryl.cc/v2/London/gbfs.json)               |
| Norwich       | Norwich, GB                          | [https://gbfs.beryl.cc/v2/Norwich/gbfs.json](https://gbfs.beryl.cc/v2/Norwich/gbfs.json)             |
| Watford       | Watford, GB                          | [https://gbfs.beryl.cc/v2/Watford/gbfs.json](https://gbfs.beryl.cc/v2/Watford/gbfs.json)             |

As new systems get added, this list may not always reflect the most up-to-date information, so please refer to the main GBFS index located at this url: [https://gbfs.beryl.cc/v2/gbfs.json](https://gbfs.beryl.cc/v2/gbfs.json) for an accurate list of currently active systems.

## Term Definitions

This section defines terms that are used throughout this document.

- JSON - (JavaScript Object Notation) is a lightweight format for storing and transporting data. This document uses many terms defined by the JSON standard, including field, array, and object. (https://www.w3schools.com/js/js_json_datatypes.asp)
- Field - In JSON, a name/value pair consists of a field name (in double quotes), followed by a colon, followed by a value. (https://www.w3schools.com/js/js_json_syntax.asp)
- GeoJSON - GeoJSON is a format for encoding a variety of geographic data structures. (https://geojson.org/)

## Files

| File Name                   | Defines                                                                                             |
| --------------------------- | --------------------------------------------------------------------------------------------------- |
| `gbfs.json`                 | Auto-discovery file that links to all of the other files published by the system.                   |
| `system_information.json`   | Details including system operator, system location, year implemented, URL, contact info, time zone. |
| `vehicle_types.json`        | Describes the types of vehicles that Beryl has available for rent.                                  |
| `station_information.json`  | List of all stations, their capacities and locations.                                               |
| `station_status.json`       | Number of available vehicles and docks at each station and station availability.                    |
| `free_bike_status.json`     | Vehicles that are not at a station and are not currently in the middle of an active ride.           |
| `system_regions.json`       | Regions for a system that is broken up by geographic or political region.                           |
| `system_pricing_plans.json` | Lists the available pricing plans for the system.                                                   |
| `geofencing_zones.json`     | Geofencing zones and their associated rules and attributes.                                         |

## File Requirements

- All files should be valid JSON
- All files in the spec may be published at a URL path or with an alternate name (e.g., `station_info` instead of `station_information.json`).
- All data should be UTF-8 encoded
- Line breaks should be represented by unix newline characters only (\n)
- Pagination is not supported.

### File Distribution

- If the publisher intends to distribute as individual HTTP endpoints then:
  - Required files must not 404. They should return a properly formatted JSON file as defined in [Output Format](#output-format).
  - Optional files may 404. A 404 of an optional file should not be considered an error.
- Auto-Discovery:
  - This specification supports auto-discovery.
  - The location of the auto-discovery file will be provided in the HTML area of the shared mobility landing page hosted at the URL specified in the URL field of the system_infomation.json file.
  - This is referenced via a _link_ tag with the following format:
    - `<link rel="gbfs" type="application/json" href="https://www.example.com/data/gbfs.json" />`
  - References:
    - http://microformats.org/wiki/existing-rel-values
    - http://microformats.org/wiki/rel-faq#How_is_rel_used

### Localization

- Each set of data files should be distributed in a single language as defined in system_information.json.
- A system that wants to publish feeds in multiple languages should do so by publishing multiple distributions, such as:
  - `https://www.example.com/data/en/system_information.json`
  - `https://www.example.com/data/fr/system_information.json`

## Field Types

- Array - A JSON element consisting of an ordered sequence of zero or more values.
- Object - A JSON element consisting of key-value pairs (fields).
- Boolean - One of two possible values, `true`or `false`. Boolean values must be JSON booleans, not strings (i.e. `true` or `false`, not `"true"` or `"false"`).
- Date - Service day in the YYYY-MM-DD format. Example: `2019-09-13` for September 13th, 2019.
- Time - Service time in the HH:MM:SS format for the time zone indicated in system_information.json (00:00:00 - 47:59:59). Time can stretch up to one additional day in the future to accommodate situations where, for example, a system was open from 11:30pm - 11pm the next day (i.e. 23:30:00-47:00:00).
- Email - An email address. Example: `example@example.com`
- Enum (Enumerable values) - An option from a set of predefined constants in the "Defines" column.
  Example: The `rental_methods` field contains values `CREDITCARD`, `PAYPASS`, etc...
- Timestamp - Timestamp fields must be represented as integers in POSIX time. (e.g., the number of seconds since January 1st 1970 00:00:00 UTC)
- ID - Should be represented as a string that identifies that particular entity. An ID:
  - must be unique within like fields (e.g. `station_id` must be unique among stations)
  - does not have to be globally unique, unless otherwise specified
  - must not contain spaces
  - should be persistent for a given entity (station, plan, etc). An exception is floating bike `bike_id`, which should not be persistent for privacy reasons (see `free_bike_status.json`).
- String - Can only contain text. Strings must not contain any formatting codes (including HTML) other than newlines.
- Language - An IETF BCP 47 language code. For an introduction to IETF BCP 47, refer to http://www.rfc-editor.org/rfc/bcp/bcp47.txt and http://www.w3.org/International/articles/language-tags/.
  Examples: `en` for English, `en-US` for American English, or `de` for German.
- Latitude - WGS84 latitude in decimal degrees. The value must be greater than or equal to -90.0 and less than or equal to 90.0.
  Example: `41.890169` for the Colosseum in Rome.
- Longitude - WGS84 longitude in decimal degrees. The value must be greater than or equal to -180.0 and less than or equal to 180.0.
  Example: `12.492269` for the Colosseum in Rome.
- Non-negative Integer - An integer greater than or equal to 0.
- Non-negative Float - A floating point number greater than or equal to 0.
- Timezone - TZ timezone from the https://www.iana.org/time-zones. Timezone names never contain the space character but may contain an underscore. Refer to http://en.wikipedia.org/wiki/List_of_tz_zones for a list of valid values.
  Example: `Asia/Tokyo`, `America/Los_Angeles` or `Africa/Cairo`.
- URL - A fully qualified URL that includes `http://` or `https://`, and any special characters in the URL must be correctly escaped. See the following http://www.w3.org/Addressing/URL/4_URI_Recommentations.html for a description of how to create fully qualified URL values.
- GeoJSON FeatureCollection - A FeatureCollection as described by the IETF RFC 7946 https://tools.ietf.org/html/rfc7946#section-3.3.
- GeoJSON Multipolygon - A Geometry Object as described by the IETF RFC https://tools.ietf.org/html/rfc7946#section-3.1.7.

### vehicle

The following definition is for a singular vehicle object, it is usually returned as part of the `vehicles` array property for [station_status.json](#station_statusjson), or as part of the `bikes` array property for [free_bike_status.json](#free_bike_statusjson). When returned as part of [free_bike_status.json](#free_bike_statusjson), the `lat` and `lon` fields are also populated with the vehicle's current location.

| Field Name                           | Type               | Defines                                                                                                                                                                                                                                                              |
| ------------------------------------ | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`bike_id` <br/>              | ID                 | Identifier of a vehicle, rotated to a random string, at minimum, after each trip to protect privacy. Note: Persistent bike_id, published publicly, could pose a threat to individual traveler privacy.                                                               |
| \-&nbsp;`is_reserved`                | Boolean            | Is the vehicle currently reserved? <br /><br /> `true` - Vehicle is currently reserved. <br /> `false` - Vehicle is not currently reserved.                                                                                                                          |
| \-&nbsp;`is_disabled`                | Boolean            | Is the vehicle currently disabled (broken)? <br /><br /> `true` - Vehicle is currently disabled. <br /> `false` - Vehicle is not currently disabled.                                                                                                                 |
| \-&nbsp;`vehicle_type_id` <br/>      | ID                 | The vehicle_type_id of this vehicle as described in [vehicle_types.json](#vehicle_typesjson).                                                                                                                                                                        |
| \-&nbsp;`lat`                        | Latitude           | If the vehicle is returned as part of [free_bike_status.json](#free_bike_statusjson), then this field is present. Latitude of the vehicle.                                                                                                                           |
| \-&nbsp;`lon`                        | Longitude          | If the vehicle is returned as part of [free_bike_status.json](#free_bike_statusjson), then this field is present. Longitude of the vehicle.                                                                                                                          |
| \-&nbsp;`current_range_meters` <br/> | Non-negative float | If the corresponding vehicle_type definition for this vehicle has a motor, then this field is required. This value represents the furthest distance in meters that the vehicle can travel without recharging or refueling with the vehicle's current charge or fuel. |

### Output Format

Every JSON file presented in this specification contains the same common header information at the top level of the JSON response object:

| Field Name      | Type                 | Defines                                                                                                         |
| --------------- | -------------------- | --------------------------------------------------------------------------------------------------------------- |
| `last_updated`  | Timestamp            | Last time the data in the feed was updated.                                                                     |
| `ttl`           | Non-negative integer | Number of seconds before the data in the feed will be updated again (0 if the data should always be refreshed). |
| `version` <br/> | String               | GBFS version number to which the feed confirms, according to the versioning framework.                          |
| `data`          | Object               | Response data in the form of `{ name: value }` pairs.                                                           |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 3600,
  "version": "2.1-RC",
  "data": {
    "name": "Beryl",
    "system_id": "beryl_bcp"
  }
}
```

### gbfs.json

Auto-discovery file that links to all of the other files published by the system.

_Note:_ Beryl maintains a `gbfs.json` index for listing active systems, in addition to a system level `gbfs.json` index that describes the files within the given system.

Example index url: [https://gbfs.beryl.cc/v2/gbfs.json](https://gbfs.beryl.cc/v2/gbfs.json)

Example system index url: [https://gbfs.beryl.cc/v2/BCP/gbfs.json](https://gbfs.beryl.cc/v2/BCP/gbfs.json)

| Field Name      | Type          | Defines                                                                                                                                                  |
| --------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `language`      | Language      | The language that will be used throughout the rest of the files. It must match the value in the [system_information.json](#system_informationjson) file. |
| \-&nbsp;`feeds` | Array\<feed\> | An array of all of the feeds that are published by this auto-discovery file. Each element in the array is an object with the keys below.                 |

#### feed

| Field Name     | Type   | Defines                                                                                                                                                                                                                                                |
| -------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| \-&nbsp;`name` | String | Key identifying the type of feed this is. The key must be the base file name defined in the spec for the corresponding feed type (`system_information` for `system_information.json` file, `station_information` for `station_information.json` file). |
| \-&nbsp;`url`  | URL    | URL for the feed. Note that the actual feed endpoints (urls) may not be defined in the `file_name.json` format. For example, a valid feed endpoint could end with `station_info` instead of `station_information.json`.                                |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "en": {
      "feeds": [
        {
          "name": "system_information",
          "url": "https://www.example.com/gbfs/1/en/system_information"
        },
        {
          "name": "station_information",
          "url": "https://www.example.com/gbfs/1/en/station_information"
        }
      ]
    },
    "fr": {
      "feeds": [
        {
          "name": "system_information",
          "url": "https://www.example.com/gbfs/1/fr/system_information"
        },
        {
          "name": "station_information",
          "url": "https://www.example.com/gbfs/1/fr/station_information"
        }
      ]
    }
  }
}
```

### system_information.json

Details including system operator, system location, year implemented, URL, contact info, time zone.

Example url: [https://gbfs.beryl.cc/v2/BCP/system_information.json](https://gbfs.beryl.cc/v2/BCP/system_information.json)

The following fields are all attributes within the main "data" object for this feed.

| Field Name     | Type         | Defines                                                                                                                                                                                                                                                                                                                                                       |
| -------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `system_id`    | ID           | Identifier for this vehicle share system. This should be globally unique (even between different systems) - for example, `bcycle_austin` or `biketown_pdx`. It is up to the publisher of the feed to guarantee uniqueness. This value is intended to remain the same over the life of the system.                                                             |
| `language`     | Language     | The language that will be used throughout the rest of the files. It must match the value in the [gbfs.json](#gbfsjson) file.                                                                                                                                                                                                                                  |
| `name`         | String       | Name of the system to be displayed to customers.                                                                                                                                                                                                                                                                                                              |
| `operator`     | String       | Name of the operator.                                                                                                                                                                                                                                                                                                                                         |
| `url`          | URL          | The URL of the vehicle share system.                                                                                                                                                                                                                                                                                                                          |
| `phone_number` | Phone Number | A single voice telephone number for the specified system that presents the telephone number as typical for the system's service area. It can and should contain punctuation marks to group the digits of the number. Dialable text (for example, Capital Bikeshare’s "877-430-BIKE") is permitted, but the field must not contain any other descriptive text. |
| `email`        | Email        | Email address actively monitored by the operator’s customer service department. This email address should be a direct contact point where riders can reach a customer service representative.                                                                                                                                                                 |
| `timezone`     | Timezone     | The time zone where the system is located.                                                                                                                                                                                                                                                                                                                    |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "system_id": "example_1",
    "language": "en",
    "name": "example_name",
    "operator": "example_operator",
    "url": "http://example.com",
    "phone_number": "02081111111",
    "email": "example@example.com",
    "timezone": "Europe/London"
  }
}
```

### vehicle_types.json

Describes the types of vehicles that Beryl has available for rent.

Example url: [https://gbfs.beryl.cc/v2/BCP/vehicle_types.json](https://gbfs.beryl.cc/v2/BCP/vehicle_types.json)

The following fields are all attributes within the main "data" object for this feed.

| Field Name      | Type                  | Defines                                                                         |
| --------------- | --------------------- | ------------------------------------------------------------------------------- |
| `vehicle_types` | Array\<vehicle_type\> | Array that contains one object per vehicle type in the system as defined below. |

#### vehicle_type

| Field Name                 | Type               | Defines                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`vehicle_type_id`  | ID                 | Unique identifier of a vehicle type. See [Field Types](#field-types) above for ID field requirements.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| \-&nbsp;`form_factor`      | Enum               | The vehicle's general form factor. <br /><br />Current valid values are:<br /><ul><li>`bicycle`</li><li>`car`</li><li>`moped`</li><li>`other`</li><li>`scooter`</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| \-&nbsp;`propulsion_type`  | Enum               | The primary propulsion type of the vehicle. <br /><br />Current valid values are:<br /><ul><li>`human` _(Pedal or foot propulsion)_</li><li>`electric_assist` _(Provides power only alongside human propulsion)_</li><li>`electric` _(Contains throttle mode with a battery-powered motor)_</li><li>`combustion` _(Contains throttle mode with a gas engine-powered motor)_</li></ul> This field was inspired by, but differs from the propulsion types field described in the [Open Mobility Foundation Mobility Data Specification](https://github.com/openmobilityfoundation/mobility-data-specification/blob/master/provider/README.md#propulsion-types). |
| \-&nbsp;`max_range_meters` | Non-negative float | If the vehicle has a motor (as indicated by having a value other than `human` in the `propulsion_type` field), this field is required. This represents the furthest distance in meters that the vehicle can travel without recharging or refuelling when it has the maximum amount of energy potential (for example, a full battery or full tank of gas).                                                                                                                                                                                                                                                                                                     |
| \-&nbsp;`name`             | String             | The public name of this vehicle type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "vehicle_types": [
      {
        "vehicle_type_id": "abc123",
        "form_factor": "bicycle",
        "propulsion_type": "human",
        "name": "Example Basic Bike"
      },
      {
        "vehicle_type_id": "def456",
        "form_factor": "scooter",
        "propulsion_type": "electric",
        "name": "Example E-scooter V2",
        "max_range_meters": 12345
      },
      {
        "vehicle_type_id": "car1",
        "form_factor": "car",
        "propulsion_type": "combustion",
        "name": "Foor-door Sedan",
        "max_range_meters": 523992
      }
    ]
  }
}
```

### station_information.json

List of all stations, their capacities and locations.

Example url: [https://gbfs.beryl.cc/v2/BCP/station_information.json](https://gbfs.beryl.cc/v2/BCP/station_information.json)

| Field Name | Type                         | Defines                                                      |
| ---------- | ---------------------------- | ------------------------------------------------------------ |
| `stations` | Array\<station_information\> | Array that contains one object per station as defined below. |

#### station_information

| Field Name           | Type                 | Defines                                                                                                                                                                                  |
| -------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`station_id` | ID                   | Identifier of a station.                                                                                                                                                                 |
| \-&nbsp;`name`       | String               | Public name of the station.                                                                                                                                                              |
| \-&nbsp;`lat`        | Latitude             | The latitude of station.                                                                                                                                                                 |
| \-&nbsp;`lon`        | Longitude            | The longitude of station.                                                                                                                                                                |
| \-&nbsp;`capacity`   | Non-negative integer | Number of total docking points installed at this station, both available and unavailable, regardless of what vehicle types are allowed at each dock. Empty indicates unlimited capacity. |

Example output:

```json
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "stations": [
      {
        "station_id": "123",
        "name": "Parking garage A",
        "lat": 12.34,
        "lon": 45.67,
        "capacity": 8
      }
    ]
  }
}
```

### station_status.json

Number of available vehicles and docks at each station and station availability.

Example url: [https://gbfs.beryl.cc/v2/BCP/station_status.json](https://gbfs.beryl.cc/v2/BCP/station_status.json)

| Field Name | Type                    | Defines                                                                    |
| ---------- | ----------------------- | -------------------------------------------------------------------------- |
| `stations` | Array\<station_status\> | Array that contains one object per station in the system as defined below. |

#### station_status

| Field Name                    | Type                         | Defines                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`station_id`          | ID                           | Identifier of a station see [station_information.json](#station_informationjson).                                                                                                                                                                                                                                                                                                                                                                                                  |
| \-&nbsp;`num_bikes_available` | Non-negative integer         | Number of vehicles of any type available for rental. Number of functional vehicles physically at the station. To know if the vehicles are available for rental, see `is_renting`.                                                                                                                                                                                                                                                                                                  |
| \-&nbsp;`num_docks_available` | Non-negative integer         | Required except for stations that have unlimited docking capacity (e.g. virtual stations). Number of functional docks physically at the station. To know if the docks are accepting vehicle returns, see `is_returning`.                                                                                                                                                                                                                                                           |
| \-&nbsp;`is_installed`        | Boolean                      | Is the station currently on the street? <br /><br />`true` - Station is installed on the street.<br />`false` - Station is not installed on the street.                                                                                                                                                                                                                                                                                                                            |
| \-&nbsp;`is_renting`          | Boolean                      | Is the station currently renting vehicles? <br /><br />`true` - Station is renting vehicles. Even if the station is empty, if it is set to allow rentals this value should be `true`.<br /> `false` - Station is not renting vehicles.                                                                                                                                                                                                                                             |
| \-&nbsp;`is_returning`        | Boolean                      | Is the station accepting vehicle returns? <br /><br />`true` - Station is accepting vehicle returns. If a station is full but would allow a return if it was not full, then this value should be `true`.<br /> `false` - Station is not accepting vehicle returns.                                                                                                                                                                                                                 |
| \-&nbsp;`last_reported`       | Timestamp                    | The last time this station reported its status to the operator's backend.                                                                                                                                                                                                                                                                                                                                                                                                          |
| \-&nbsp;`vehicles` <br/>      | Array\<[vehicle](#vehicle)\> | This field's value is an array of [vehicle](#vehicle) objects. Each object contains data about a specific vehicle that is currently present at the docking station. Each of these vehicles is assumed to be rentable unless otherwise indicated with the is_reserved or is_disabled flags. All of the remaining fields in this table represent key/values for each vehicle object in this array. The length of this array must equal the value of the `num_bikes_available` field. |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "stations": [
      {
        "station_id": "station 1",
        "is_installed": true,
        "is_renting": true,
        "is_returning": true,
        "last_reported": 1434054678,
        "num_docks_available": 3,
        "vehicles": [
          {
            "bike_id": "mno345",
            "is_reserved": false,
            "is_disabled": false,
            "vehicle_type_id": "abc123"
          },
          {
            "bike_id": "pqr678",
            "is_reserved": false,
            "is_disabled": false,
            "vehicle_type_id": "def456",
            "current_range_meters": 5432
          }
        ]
      },
      {
        "station_id": "station 2",
        "is_installed": true,
        "is_renting": true,
        "is_returning": true,
        "last_reported": 1434054678,
        "num_docks_available": 8,
        "vehicles": [
          {
            "bike_id": "stu901",
            "is_reserved": false,
            "is_disabled": false,
            "vehicle_type_id": "abc123"
          },
          {
            "bike_id": "vwx234",
            "is_reserved": false,
            "is_disabled": false,
            "vehicle_type_id": "def456",
            "current_range_meters": 4321
          }
        ]
      }
    ]
  }
}
```

### free_bike_status.json

Vehicles that are not at a station and are not currently in the middle of an active ride.

Example url: [https://gbfs.beryl.cc/v2/BCP/free_bike_status.json](https://gbfs.beryl.cc/v2/BCP/free_bike_status.json)

| Field Name | Type                         | Defines                                                                           |
| ---------- | ---------------------------- | --------------------------------------------------------------------------------- |
| `bikes`    | Array\<[vehicle](#vehicle)\> | Array that contains one object per [vehicle](#vehicle) that is currently stopped. |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "bikes": [
      {
        "bike_id": "ghi789",
        "is_reserved": false,
        "is_disabled": false,
        "vehicle_type_id": "abc123",
        "lat": 12.34,
        "lon": 56.78
      },
      {
        "bike_id": "jkl012",
        "is_reserved": false,
        "is_disabled": false,
        "vehicle_type_id": "def456",
        "lat": 12.34,
        "lon": 56.78,
        "current_range_meters": 6543
      }
    ]
  }
}
```

### system_regions.json

Regions for a system that is broken up by geographic or political region.

Example url: [https://gbfs.beryl.cc/v2/BCP/system_regions.json](https://gbfs.beryl.cc/v2/BCP/system_regions.json)

| Field Name | Type            | Defines                            |
| ---------- | --------------- | ---------------------------------- |
| `regions`  | Array\<region\> | Array of objects as defined below. |

#### region

| Field Name          | Type   | Defines                      |
| ------------------- | ------ | ---------------------------- |
| \-&nbsp;`region_id` | ID     | Identifier for the region.   |
| \-&nbsp;`name`      | String | Public name for this region. |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "regions": [
      {
        "region_id": "region_1",
        "name": "region_name"
      }
    ]
  }
}
```

### system_pricing_plans.json

Lists the available pricing plans for the system.

Example url: [https://gbfs.beryl.cc/v2/BCP/system_pricing_plans.json](https://gbfs.beryl.cc/v2/BCP/system_pricing_plans.json)

| Field Name | Type          | Defines                                 |
| ---------- | ------------- | --------------------------------------- |
| `plans`    | Array\<plan\> | Array of plan objects as defined below. |

#### plan

| Field Name                       | Type                           | Defines                                                                                                                                                                                                     |
| -------------------------------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`plan_id`                | ID                             | Identifier for a pricing plan in the system.                                                                                                                                                                |
| \-&nbsp;`type`                   | Enum                           | The type of pricing plan.<br /><br />The current values are:<br /><ul><li>`payg` - Pay as you go.</li><li>`bundle` - Minute bundle.</li><li>`day_pass` - Day pass.</li></ul>                                |
| \-&nbsp;`name`                   | String                         | Name of this pricing plan.                                                                                                                                                                                  |
| \-&nbsp;`currency`               | String                         | Currency used to pay the fare. <br /><br /> This pricing is in ISO 4217 code: http://en.wikipedia.org/wiki/ISO_4217 <br />(e.g. `CAD` for Canadian dollars, `EUR` for euros, or `JPY` for Japanese yen.)    |
| \-&nbsp;`is_taxable`             | Boolean                        | Will additional tax be added to the base price?<br /><br />`true` - Yes.<br /> `false` - No. <br /><br />`false` may be used to indicate that tax is not charged or that tax is included in the base price. |
| \-&nbsp;`description`            | String                         | Customer-readable description of the pricing plan. This should include the duration, price, conditions, etc. that the publisher would like users to see.                                                    |
| \-&nbsp;`price_rates_per_minute` | Array\<price_rate_per_minute\> | Customer-readable description of the pricing plan. This should include the duration, price, conditions, etc. that the publisher would like users to see.                                                    |
| \-&nbsp;`additional_fees`        | Array\<additional_fee\>        | Customer-readable description of the pricing plan. This should include the duration, price, conditions, etc. that the publisher would like users to see.                                                    |

#### price_rate_per_minute

| Field Name                | Type                 | Defines                                                                                                                                                                          |
| ------------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`from_minute`     | Non-negative integer | How many minutes can elapse before the price rate per minute is applied (e.g. If this value is 15, it means the first 15 minutes of the ride are free).                          |
| \-&nbsp;`minute_price`    | Non-negative float   | The monetary unit of currency charged for a single minute (e.g. For a plan with currency "GBP", a `minute_price` of 0.05 would equate to 5 British pence per minute).            |
| \-&nbsp;`vehicle_type_id` | ID                   | The `vehicle_type_id` of this vehicle as described in [vehicle_types.json](#vehicle_typesjson).                                                                                  |
| \-&nbsp;`reset_price`     | Non-negative float   | The monetary unit of currency charged for starting a ride (e.g. For a plan with currency "GBP", a `reset_price` of 2.0 would equate to £2 (Pound sterling) for starting a ride). |

#### additional_fee

The description for each type of additional fee is as follows:

- `purchase_price` - This type of fee is present if the plan requires an initial purchase.
- `out_of_bundle_minute_increment_cost` - This type of fee is present if the plan includes a minute overflow charge (e.g. For a 100 minute plan, and a ride with a duration of 120 minutes would include an additional 20x the price of this fee). This fee typically only applies to plans of type `bundle`.
- `out_of_service_area_parking_charge` - This type of fee is present if the plan includes an additional charge for parking a vehicle outside of the region's geofence zones.
- `out_of_zone_parking_charge` - This type of fee is present if the plan includes an additional charge for parking a vehicle outside of an available station.

| Field Name                | Type               | Defines                                                                                                                                                                                                                                                                           |
| ------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`fee`             | Enum               | The type of additional fee that can be applied with this pricing plan.<br /><br />The current values are:<br /><ul><li>`purchase_price`</li><li>`out_of_bundle_minute_increment_cost`</li><li>`out_of_service_area_parking_charge`</li><li>`out_of_zone_parking_charge`</li></ul> |
| \-&nbsp;`price`           | Non-negative float | The monetary unit of currency charged for the additional fee (e.g. For a plan with currency "GBP", a `price` of 1.5 would equate to £1.50).                                                                                                                                       |
| \-&nbsp;`vehicle_type_id` | ID                 | The `vehicle_type_id` of this vehicle as described in [vehicle_types.json](#vehicle_typesjson).                                                                                                                                                                                   |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "plans": [
      {
        "plan_id": "123",
        "type": "payg",
        "name": "Pay As You Ride",
        "description": "Pay As You Ride",
        "currency": "GBP",
        "is_taxable": false,
        "price_rates_per_minute": [
          {
            "from_minute": 0,
            "minute_price": 0.05,
            "vehicle_type_id": "beryl_bike",
            "reset_price": 1.0
          },
          {
            "from_minute": 0,
            "minute_price": 0.1,
            "vehicle_type_id": "bbe",
            "reset_price": 1.5
          }
        ],
        "additional_fees": [
          {
            "fee": "purchase_price",
            "price": 0.0,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "fee": "out_of_service_area_parking_charge",
            "price": 5.0,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "fee": "out_of_zone_parking_charge",
            "price": 1.0,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "fee": "purchase_price",
            "price": 0.0,
            "vehicle_type_id": "bbe"
          },
          {
            "fee": "out_of_service_area_parking_charge",
            "price": 5.0,
            "vehicle_type_id": "bbe"
          },
          {
            "fee": "out_of_zone_parking_charge",
            "price": 1.0,
            "vehicle_type_id": "bbe"
          }
        ]
      },
      {
        "plan_id": "321",
        "type": "bundle",
        "name": "100 mins",
        "description": "100 mins",
        "currency": "GBP",
        "is_taxable": false,
        "price_rates_per_minute": [
          {
            "from_minute": 0,
            "minute_price": 0.0,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "from_minute": 0,
            "minute_price": 0.0,
            "vehicle_type_id": "bbe",
            "reset_price": 1.5
          }
        ],
        "additional_fees": [
          {
            "fee": "purchase_price",
            "price": 5.0,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "fee": "out_of_bundle_minute_increment_cost",
            "price": 0.05,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "fee": "out_of_service_area_parking_charge",
            "price": 5.0,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "fee": "out_of_zone_parking_charge",
            "price": 1.0,
            "vehicle_type_id": "beryl_bike"
          },
          {
            "fee": "purchase_price",
            "price": 5.0,
            "vehicle_type_id": "bbe"
          },
          {
            "fee": "out_of_bundle_minute_increment_cost",
            "price": 0.1,
            "vehicle_type_id": "bbe"
          },
          {
            "fee": "out_of_service_area_parking_charge",
            "price": 5.0,
            "vehicle_type_id": "bbe"
          },
          {
            "fee": "out_of_zone_parking_charge",
            "price": 1.0,
            "vehicle_type_id": "bbe"
          }
        ]
      }
    ]
  }
}
```

### geofencing_zones.json

**Note:** _The following section is still in progress._

Geofencing zones and their associated rules and attributes.

By default, no restrictions apply everywhere. Geofencing zones should be modelled according to restrictions rather than allowance. An operational area (outside of which vehicles cannot be used) should be defined with a counterclockwise polygon, and a limitation area (in which vehicles can be used under certain restrictions) should be defined with a clockwise polygon.

Example url: [TBC](https://beryl.cc/)

| Field Name         | Type                      | Defines                                                                                                                         |
| ------------------ | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `geofencing_zones` | GeoJSON FeatureCollection | Each geofenced zone and its associated rules and attributes is described as an object within the array of features, as follows. |

#### GeoJSON FeatureCollection

| Field Name         | Type                     | Defines                                                                                        |
| ------------------ | ------------------------ | ---------------------------------------------------------------------------------------------- |
| \-&nbsp;`type`     | String                   | “FeatureCollection” (as per IETF [RFC 7946](https://tools.ietf.org/html/rfc7946#section-3.3)). |
| \-&nbsp;`features` | Array\<GeoJSON Feature\> | Array of objects as defined below.                                                             |

#### GeoJSON Feature

| Field Name           | Type                 | Defines                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`type`       | String               | “Feature” (as per IETF [RFC 7946](https://tools.ietf.org/html/rfc7946#section-3.3)).                                                                                                                                                                                                                                                                                                                                                      |
| \-&nbsp;`geometry`   | GeoJSON Multipolygon | A polygon that describes where rides might not be able to start, end, go through, or have other limitations. A clockwise arrangement of points defines the area enclosed by the polygon, while a counterclockwise order defines the area outside the polygon ([right-hand rule](https://tools.ietf.org/html/rfc7946#section-3.1.6)). All geofencing zones contained in this list are public (i.e., can be shown on a map for public use). |
| \-&nbsp;`properties` | geofence_properties  | Properties: As defined below, describing travel allowances and limitations.                                                                                                                                                                                                                                                                                                                                                               |

#### geofence_properties

| Field Name      | Type                   | Defines                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| \-&nbsp;`name`  | String                 | Public name of the geofencing zone.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| \-&nbsp;`rules` | Array\<geofence_rule\> | Array that contains one object per rule as defined below. <br /><br /> In the event of colliding rules within the same polygon, the earlier rule (in order of the JSON file) takes precedence. <br> In the case of overlapping polygons, the combined set of rules associated with the overlapping polygons applies to the union of the polygons. In the event of colliding rules in this set, the earlier rule (in order of the JSON file) also takes precedence. |

#### geofence_rule

| Field Name                     | Type                 | Defines                                                                                                                                                                                                                                              |
| ------------------------------ | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \-&nbsp;`vehicle_type_id`      | Array\<ID\>          | Array of IDs of vehicle types for which any restrictions should be applied (see vehicle type definitions in [PR #136](https://github.com/NABSA/gbfs/pull/136)). If vehicle_type_ids are not specified, then restrictions apply to all vehicle types. |
| \-&nbsp;`ride_allowed`         | Boolean              | Is the undocked (“free bike”) ride allowed to start and end in this zone? <br /><br /> `true` - Undocked (“free bike”) ride can start and end in this zone. <br /> `false` - Undocked (“free bike”) ride cannot start and end in this zone.          |
| \-&nbsp;`ride_through_allowed` | Boolean              | Is the ride allowed to travel through this zone? <br /><br /> `true` - Ride can travel through this zone. <br /> `false` - Ride cannot travel through this zone.                                                                                     |
| \-&nbsp;`maximum_speed_kph`    | Non-negative Integer | What is the maximum speed allowed, in kilometers per hour? <br /><br /> If there is no maximum speed to observe, this can be omitted.                                                                                                                |

Example output:

```jsonc
{
  "last_updated": 1434054678,
  "ttl": 0,
  "version": "2.1-RC",
  "data": {
    "geofencing_zones": {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [100.0, 0.0],
              [101.0, 0.0],
              [101.0, 1.0],
              [100.0, 1.0],
              [100.0, 0.0]
            ]
          },
          "properties": {
            "name": "main_region",
            "rules": [
              {
                "vehicle_type_id": "def456",
                "ride_allowed": true,
                "ride_through_allowed": true,
                "maximum_speed_kph": 15
              }
            ]
          }
        }
      ]
    }
  }
}
```

## Disclaimers

_Apple Pay, PayPass and other third-party product and service names are trademarks or registered trademarks of their respective owners._

## License

Except as otherwise noted, the content of this page is licensed under the [Creative Commons Attribution 3.0 License](http://creativecommons.org/licenses/by/3.0/).
