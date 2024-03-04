<template>
  <div class="map-wrap">
    <div class="map" ref="mapContainer"></div>
  </div>
</template>

<script>
import { Map,Marker, } from 'maplibre-gl';
import { shallowRef, onMounted, onUnmounted, markRaw } from 'vue';
import "@maptiler/sdk/dist/maptiler-sdk.css";
import * as maptilersdk from '@maptiler/sdk';

export default {
  name: "Map",
  setup () {
    const mapContainer = shallowRef(null);
    const map = shallowRef(null);
    
    onMounted(() => {
      maptilersdk.config.apiKey = 'hNe46iLsUTvFaXNqBNne';


      const initialState = {  lng: 78.8718, lat: 21.7679, zoom: 10};


      map.value = markRaw(new Map({
        container: mapContainer.value,
        style: `https://api.maptiler.com/maps/streets-v2/style.json?key=hNe46iLsUTvFaXNqBNne`,
        center: [initialState.lng, initialState.lat],
        zoom: initialState.zoom
      }));
map.value.on('style.load', async function() {
    const geojson = await maptilersdk.data.get('ce8dee2a-c379-4764-b514-f447fa5d72e4');
    map.value.addSource('Geodata', {
        type: 'geojson',
        data: geojson
    });
    map.value.addLayer({
        'id': 'Geodata',
        'type': 'fill',
        'source': 'Geodata',
        'layout': {},
        'paint': {
            'fill-color': '#8B4513',
            'fill-opacity': 0.8
        }
    });

    // Now that the style is loaded, you can add markers or perform other map operations here
    var popup = new maptilersdk.Popup({ offset: 25 }).setText(
        'Construction of IT park.'
    );

    new Marker({color: "#FF0000"})
    .setLngLat([ 76.98574000,8.60399000])
    .setPopup(popup)
    .addTo(map.value);

    new Marker({color: "#FF0000",
    draggable: true})
    .setLngLat([76.32640000,9.49004000])
    .addTo(map.value);
});

    })

    return {
      map, mapContainer
    };
  }
};
</script>


<style scoped>
.map-wrap {
  position: relative;
  width: 1500px;
  height: 1400px;
}

.map {
  position: absolute;
  width: 100%;
  height: 100%;
}

</style>