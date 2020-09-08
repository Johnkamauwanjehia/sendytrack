<template>
  <div>
    <h3>Driver Tracking Page</h3>
    <p>
      <span>Kagure Delivers</span>
      <button @click.prevent="animateMovement" class="ml-3" type="button">Animate</button>
    </p>
    <gmap-map ref="mapRef" :center="center" :zoom="15" style="width:100%;  height: 600px;">
      <gmap-marker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        :clickable="true"
        :draggable="true"
        :animation="2"
        :icon="icon"
        @click="openWindow"
      />
      <gmap-info-window
        @closeclick="window_open=false"
        :opened="window_open"
        :position="infowindow"
        :options="{
              pixelOffset: {
                width: 0,
                height: -35
              }
            }"
      >
        <h3>Driver Info</h3>
        JOHN PHIL: {{ driverInfo.name }}
      </gmap-info-window>
    </gmap-map>
  </div>
</template>

<script>
export default {
  name: "GoogleMap",
  data() {
    return {
      //Driver information
      driverInfo: {
        name: "Driver A",
        location: { lat: -1.300355, lng: 36.77385 },
      },
      info_marker: null,
      infowindow: { lat: -1.300355, lng: 36.77385 },
      window_open: false,
      // Nairobi coordinates
      center: { lat: -1.300355, lng: 36.77385 },
      /**
       * Marker icon
       * Setting it to: https://images.sendyit.com/web_platform/vendor_type/top/2.svg does not appear
       * So left default
       * Changing to any image works!
       */
      icon: null,
      locs: [
        { lat: -1.298982, lng: 36.776811 },
        { lat: -1.297459, lng: 36.776747 },
        { lat: -1.296193, lng: 36.776726 },
        { lat: -1.296097, lng: 36.779236 },
        { lat: -1.296151, lng: 36.777637 },
        { lat: -1.296215, lng: 36.776693 },
        { lat: -1.294252, lng: 36.776586 },
        { lat: -1.294048, lng: 36.77679 },
        { lat: -1.293973, lng: 36.779118 },
        { lat: -1.292622, lng: 36.779075 },
        { lat: -1.291844, lng: 36.779049 },
      ],
      markers: [],
      places: [],
      currentPlace: null,
    };
  },

  mounted() {
    this.addMarker(this.driverInfo.location);
  },

  methods: {
    // receives a place object via the autocomplete component
    setPlace(place) {
      this.currentPlace = place;
    },
    openWindow() {
      console.log(this);
      this.window_open = true;
    },
    addMarker(loc) {
      const marker = {
        lat: loc.lat,
        lng: loc.lng,
      };
      this.markers.push({ position: marker, label: "Moving" });
      this.infowindow = marker;
      this.places.push(this.currentPlace);
      this.center = marker;
      this.currentPlace = null;
    },
    animateMovement() {
      this.markers = [];
      for (let i = 0; i < this.locs.length; i++) {
        const loc = this.locs[i];
        setTimeout(() => {
          this.addMarker(loc);
        }, i*1000);
      }
    }
  },
};
</script>