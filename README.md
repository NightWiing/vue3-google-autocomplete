# Vue3 Google Places Autocomplete Widget

This is simple google places autocomplete address widget for your use in Vu3 applications. This is basically style-less component so you can provide styling/classes as per your need.

![Demo GIF](https://i.imgur.com/lYXgEm8.gif)

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
Here on `@set` event you can get your google places api payload
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

There is one prop `isFullPayload` which is `false` by default but if you pass `isFullPayload: true` as shown below you will get full (default) google places api payload.

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

## Contribution

Suggestions and pull requests are welcome after discussing the issue
