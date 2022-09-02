<template>
  <div>
    <div class="header-margins">
      Google Maps Polygon |
      <small>www.nightprogrammer.com</small>
    </div>
    <div id="data-layers-map" class="map-margins"></div>
  </div>
</template>

<script>
import loadGoogleMapsApi from "load-google-maps-api";
import { gMapsApiKey } from "./../constants";

export default {
  name: "AnimatedMarkerMap",
  data() {
    return {
      map: null,
    };
  },
  mounted() {
    loadGoogleMapsApi({
      key: gMapsApiKey,
      libraries: ["drawing", "geometry"],
    }).then(async () => {
      const mapZoom = 14;
      const { google } = window;
      const mapOptions = {
        zoom: mapZoom,
        mapTypeId: google.maps.MapTypeId.HYBRID,
        center: new google.maps.LatLng({ lat: 23, lng: 57 }),
        mapTypeControl: true,
        streetViewControl: false,
        mapTypeControlOptions: {
          position: google.maps.ControlPosition.BOTTOM_LEFT,
        },
      };
      this.map = new google.maps.Map(
        document.getElementById("data-layers-map"),
        mapOptions
      );

      const polygonCoords = [
        { lat: 28.4173106, lng: 77.0406078 },
        { lat: 28.4014569, lng: 77.0450281 },
        { lat: 28.406553, lng: 77.0587181 },
        { lat: 28.4208963, lng: 77.0517658 },
        { lat: 28.4173106, lng: 77.0406078 },
      ];

      const tempBounds = new google.maps.LatLngBounds();

      for (let j = 0; j < polygonCoords.length; j++) {
        const x = {
          lat: polygonCoords[j].lat,
          lng: polygonCoords[j].lng,
        };
        const BoundLatLng = new google.maps.LatLng({
          lat: parseFloat(x.lat),
          lng: parseFloat(x.lng),
        });
        tempBounds.extend(BoundLatLng);
      }
      const centroid = tempBounds.getCenter();

      this.map.data.add({
        geometry: new google.maps.Data.Polygon([polygonCoords]),
      });

      this.map.setCenter(centroid);
    });
  },
};
</script>

<style scoped>
.header-margins {
  margin-left: 40px;
  margin-top: 20px;
}

.map-margins {
  height: 400px;
  width: 600px;
  margin: 30px 40px;
}
</style>