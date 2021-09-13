<template>
  <div style="height: 350px; width: 100%">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-polygon :lat-lngs="polygon.latlngs" :color="polygon.color" @click="clickBtn($event)">
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
      </l-polygon>
      <l-marker v-if="showMarker" :lat-lng="marker" />
    </l-map>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import { latLng } from "leaflet";
import { LMap, LTileLayer, LPolygon, LPopup, LMarker } from "vue2-leaflet";
import { Icon } from "leaflet";
import geoJsonData  from "@/geojson";

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  components: {
    LMap,
    LTileLayer,
    LPolygon,
    LPopup,
    LMarker
  },
  data() {
    return {
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 8,
      center: [47.31322, -1.319482],
      polygon: {
        latlngs: [
          [47.2263299, -1.6222],
          [47.21024000000001, -1.6270065],
          [47.1969447, -1.6136169],
          [47.18527929999999, -1.6143036],
          [47.1794457, -1.6098404],
          [47.1775788, -1.5985107],
          [47.1676598, -1.5753365],
          [47.1593731, -1.5521622],
          [47.1593731, -1.5319061],
          [47.1722111, -1.5143967],
          [47.1960115, -1.4841843],
          [47.2095404, -1.4848709],
          [47.2291277, -1.4683914],
          [47.2533687, -1.5116501],
          [47.2577961, -1.5531921],
          [47.26828069, -1.5621185],
          [47.2657179, -1.589241],
          [47.2589612, -1.6204834],
          [47.237287, -1.6266632],
          [47.2263299, -1.6222],
        ],
        color: "green",
      },
      showParagraph: false,
      geojson: geoJsonData,
      showMarker: false,
      marker: latLng(47.41322, -1.219482),

    };
  },
  methods: {
    clickBtn(event) {
      const {
        latlng: { lat, lng },
      } = event;
      this.marker = latLng(lat, lng);
      this.showMarker = true;
      console.log('-------- event', event)
    },
    innerClick() {
      console.log('-------- Click')
    }
  }
};
</script>
