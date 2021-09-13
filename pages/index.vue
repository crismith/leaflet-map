<template>
  <div style="height: 650px; width: 100%">
    <l-map 
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
      :zoom="zoom" 
      :center="center">
      <l-tile-layer :url="url" :attribution="attribution" />
      <l-geo-json
        v-if="show"
        :geojson="geojson"
        :options="options"
        :options-style="styleFunction"
        @click="openData($event)"
      >
      <l-popup>
        <div @click="innerClick">
          <div class="d-flex">
            <div class="d-flex flex-column">
              <label for=""> Muestras
                <input type="number" class="form-control">
              </label>
              <label for=""> Radio
                <input type="number" class="form-control">
              </label>
            </div>
            <div class="d-flex align-items-center">
              <img src="https://img.icons8.com/external-kiranshastry-lineal-kiranshastry/64/000000/external-delete-miscellaneous-kiranshastry-lineal-kiranshastry.png" width="30" height="30" />
            </div>
          </div>
        </div>
      </l-popup>
      </l-geo-json>
      <l-marker v-if="showMarker" :lat-lng="marker" />
    </l-map>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";

import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LGeoJson, LPopup } from "vue2-leaflet";
import { Icon } from "leaflet";
import geoJsonData from "@/geojson";

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
    LMarker,
    LPopup,
  },
  data() {
    return {
      showParagraph: true,
      loading: false,
      show: true,
      enableTooltip: true,
      zoom: 17,
      center: [-14.00943, -75.80271],
      geojson: geoJsonData,
      fillColor: "#555555",
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: latLng(-14.00943, -75.80271),
      showMarker: false,
      options: {
        onEachFeature: function () {}
      }
    };
  },
  computed: {
    styleFunction() {
      const fillColor = this.fillColor;
      return () => {
        return {
          weight: 2,
          color: "#555555",
          opacity: 0.5,
          fillColor: fillColor,
          // fillOpacity: 1,
          zoom: 10,
          stroke: 2
        };
      };
    },
  },
  methods: {
    innerClick() {
      console.log("-------- Click");
    },
    openData(event) {
      const {
        latlng: { lat, lng },
      } = event;
      console.log('event', event)
      this.marker = latLng(lat, lng);
      this.showMarker = true;
      this.options.onEachFeature = this.onEachFeatureFunction(event.propagatedFrom.feature, event.layer);
    },
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    onEachFeatureFunction(feature, layer){
      console.log('layer DC', layer)
      console.log('feature DC', feature)
      layer.bindPopup(`
        <div @click="innerClick">
          <div class="d-flex">
            <div class="d-flex flex-column">
              <label for=""> Muestras
                <input type="number" class="form-control">
              </label>
              <label for=""> Radio
                <input type="number" class="form-control">
              </label>
            </div>
            <div class="d-flex align-items-center">
              <img src="https://img.icons8.com/external-kiranshastry-lineal-kiranshastry/64/000000/external-delete-miscellaneous-kiranshastry-lineal-kiranshastry.png" width="30" height="30" />
            </div>
          </div>
        </div>
      `);
    }
  },
};
</script>
