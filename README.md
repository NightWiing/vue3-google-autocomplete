# Vue3 Google Places Autocomplete Widget

This is simple google places autocomplete address widget for your use in Vu3 applications. This is basically style-less component so you can provide styling/classes as per your need.

![Demo GIF](https://i.imgur.com/lYXgEm8.gif)

## Prerequisites

* Node.js (@16.13.0 or later)
* NPM (@6.13.7 X or later)

## Installation

```
npm install vue3-google-autocomplete
```
or

```
yarn add vue3-google-autocomplete
```

## Usage
Here is the example on how to use it inside your Vue component.

```Javascript
<template>
  <GoogleAutocomplete
    v-model="value"
    api-key="process.env.VITE_APP_GAPI_KEY"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { GoogleAutocomplete } from 'vue3-google-autocomplete'

const value = ref()
</script>
```

Beside this we have one `@set` event on which you can get your google places api payload and with v-model you will get place name so it will be easier for you to integrate this inside your form for address purpose

```Javascript
<template>
  <GoogleAutocomplete
    v-model="value"
    api-key="process.env.VITE_APP_GAPI_KEY"
    @set="getPayload($event)"
  />
</template>
```

By default you will get payload like this.

Eg.

```Javascript
{
    "name": "The White House",
    "city": "Washington",
    "state": "District of Columbia",
    "country": "United States",
    "latitude": 38.8976763,
    "longitude": -77.0365298
}
```

There is one prop option `isFullPayload` which is `false` by default but if you pass `isFullPayload: true` prop to `GoogleAutocomplete` component then you will get whole google places api payload.

Eg.

```Javascript
<template>
  <GoogleAutocomplete
    v-model="value"
    api-key="process.env.VITE_APP_GAPI_KEY"
    :isFullPayload="true"
    @set="getPayload($event)"
  />
</template>
```
In this case payload will look like this.

```Javascript
{
    "address_components": [
        {
            "long_name": "1600",
            "short_name": "1600",
            "types": [
                "street_number"
            ]
        },
        {
            "long_name": "Pennsylvania Avenue Northwest",
            "short_name": "Pennsylvania Avenue NW",
            "types": [
                "route"
            ]
        },
        {
            "long_name": "Northwest Washington",
            "short_name": "Northwest Washington",
            "types": [
                "neighborhood",
                "political"
            ]
        },
        {
            "long_name": "Washington",
            "short_name": "Washington",
            "types": [
                "locality",
                "political"
            ]
        },
        {
            "long_name": "District of Columbia",
            "short_name": "DC",
            "types": [
                "administrative_area_level_1",
                "political"
            ]
        },
        {
            "long_name": "United States",
            "short_name": "US",
            "types": [
                "country",
                "political"
            ]
        },
        {
            "long_name": "20500",
            "short_name": "20500",
            "types": [
                "postal_code"
            ]
        }
    ],
    "formatted_address": "1600 Pennsylvania Avenue NW, Washington, DC 20500, USA",
    "geometry": {
        "location": {
            "lat": 38.8976763,
            "lng": -77.0365298
        },
        "viewport": {
            "south": 38.89639945,
            "west": -77.0379004802915,
            "north": 38.90150685,
            "east": -77.03520251970849
        }
    },
    "name": "The White House",
    "html_attributions": []
}
```


## Contribution

Suggestions and pull requests are welcome after discussing the issue
