# System Overview

## Back-End

A docker container, handles:
- __data processing__ - [Read More](#frame-processing)
- __processed data storage__ - [Read More](#processed-data-storage)
- __face id storage__ - [Read More](#face-id-storage)
    - Write Operation Only by __System Admin__
- __temp face id storage__ - [Read More](#face-id-storage)
    - Write Operation Only by both __Software__ and __System Admin__
- __camera id storage__ - [Read More](#camera-id-storage)
    - Write Operation Only by __System Admin__

## esp-camera

A Camera combined with mico-controller, handles

- __data collection__ - [Read More](#data-collection)
    - 
- __data transmission__ - [Read More](#data-transmission)

```json
{
    "data":"image",
    "time-stamp":"time-stamp",
    "camera-id":"a1na83",
}
```


<a id="frame-processing"></a>

# Frame Processing - Single frame

Frame Processing Flow : 

1. __Frame / Image Received__
2. __Face Detection__
3. __Face Indentification__ - finds __face-id__ of the face

    1. face-id not found - generated a [temp/ anonymous](##) face-id
4. __Data Storage__ 
```json
{
    "camera-id":"xxxxx",
    "time":"--x--x----x",
    "faces":[
        "face-id1",
        "face-id2",
        "face-id3",
        "face-id3",
        "--------",
        "--------",
        "--------",
        "face-idn",

    ]
}
```

<a id="processed-data-storage"></a>

# Procssed Data Storage

Data Storage - Schema

- Camera-ID's

```json
// camera
{
    "camera-id-1":{
        "name":"camera-name",
        "location":"camera-location"
    },
    "camera-id-2":{
        "name":"camera-name",
        "location":"camera-location"
    },
    "camera-id-n":{
        "name":"camera-name",
        "location":"camera-location"
    }
}
```

- Camera Process Face Storage
```json
{
    "time-stamp":[
        "face-id1",
        "face-id2",
        "face-id3",
        "face-id4",
        "--------", 
        "face-idn"
    ],
    "time-stamp2":[
        "face-id1",
        "face-id2",
        "face-id3",
        "face-id4",
        "--------", 
        "face-idn"
    ]
}
```

<a id="face-id-storage"></a>

# Face-ID storage

face id - [hexadecimal](##)

```json
// face-id storage
{
    "face-id1":{
        "name":"face-name",
        // other data
    },
    "face-id2":{
        "name":"face-name",
        // other data
    },
}
```

Temp Face Id - Software

```json
// temp-face-id storage
{
    "temp-face-id1":{
        "name":"anonymous",
        // other data
    },
    "temp-face-id2":{
        "name":"anomymous",
        // other data
    },
}
```


<a id="data-collection"></a>

# Data Collection

Records Video in - 

#### [@30 Frame](##) Per Second

#### [@60 Frame](##) Per Second
- face tracking

#### [@One Frame](##) Per Unit Time

it can be used in scenario like 

- class attendance, low motion application


```md
just for optimization can a algo which can detect if there is a diff between the current and next frame

if there is no/ minute difference then __discard the next frame__

algo when detects a motion records at higher fps
```


<a id="data-transmission"></a>

# Data Transmission

Sends the frame for [*processing*](##) : 

```json
{
    "data":"image:png/jpeg/etc",
    "time":"time-stamp",
    "camera-id":"a1na83",
}
```