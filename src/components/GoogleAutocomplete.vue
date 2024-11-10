<template>
  <input
    ref="origin"
    type="text"
    :class="class"
    :placeholder="placeholder"
  />
</template>

<script setup lang="ts">
import { onMounted, ref, nextTick, onBeforeUnmount } from 'vue'

const emit = defineEmits(['update:modelValue', 'set'])
const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  },
  apiKey: {
    type: String,
    required: true
  },
  placeholder: {
    type: String,
    default: ''
  },
  types: {
    type: Array,
    default: () => [],
  },
  isFullPayload: {
    type: Boolean,
    default: false
  },
  class: {
    type: String,
    default: ''
  }
})

const origin = ref<any>();
const place = ref<any>()
const isLoaded = ref(false)

const loadApi = () => {
  return new Promise<void>((resolve, reject) => {
    if (window.google && window.google.maps && window.google.maps.places) {
      resolve();
    } else {
      if (!isLoaded.value) {
        isLoaded.value = true
        const gScript = document.createElement("script");
        gScript.setAttribute(
          "src",
          `https://maps.googleapis.com/maps/api/js?key=${props.apiKey}&libraries=places&v=weekly&callback=initMap`
        );

        (window as any).initMap = () => {
          resolve()
        }

        gScript.onerror = async (err) => {
          reject(err);
        }

        document.head.appendChild(gScript);
      }
    }
  })
}

const setPlacesListener = () => {
  if (origin.value) {
    const places = google.maps.places
    const autocompleteInstance = new places.Autocomplete(origin.value, {
      fields: ['formatted_address', 'address_components', 'geometry', 'name'],
      types: props.types,
      strictBounds: false
    })

    autocompleteInstance.addListener('place_changed', async () => {
      place.value = await autocompleteInstance.getPlace()
      const latitude = await place.value.geometry.location.lat()
      const longitude = await place.value.geometry.location.lng()
      let city = ''
      let state = ''
      let country = ''
      for (const component of place.value?.address_components) {
        if (component.types.includes('locality')) {
          city = await component.long_name
        } else if (component.types.includes('administrative_area_level_1')) {
          state = await component.long_name
        } else if (component.types.includes('country')) {
          country = await component.long_name
        }
      }
      const payload: Object = {
        name: place.value?.name,
        city: city,
        state: state,
        country: country,
        latitude: latitude,
        longitude: longitude
      }
      emit('update:modelValue', place.value?.name)
      if (props.isFullPayload)
        emit('set', place.value)
      else
        emit('set', payload)
    })
  }
}

onMounted(async () => {
  try {
    await loadApi()
    await nextTick()
    setPlacesListener()
  } catch (error) {
    console.error("Failed to load Google Maps API", error);
  }
})

onBeforeUnmount(() => {
  delete (window as any).initMap;
});
</script>
