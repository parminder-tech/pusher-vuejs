<template>
  <div>
    <gmap-map :center="center" :zoom="12" style="width: 100%; height: 100vh">
      <gmap-marker :position="location" :animation="1"></gmap-marker>
    </gmap-map>
  </div>
</template>

<script>
import Pusher from "pusher-js";

export default {
  name: "DrawGoogleMap",
  created() {
    // ...
    this.subscribe();
  },
  data() {
    return {
      center: {
        lat: 37.7768006,
        lng: -122.4187928,
      },
      location: {
        lat: 39.7837304,
        lng: -100.4458825,
        label: "United States",
      },
      currentLocation: null,
    };
  },

  methods: {
    subscribe() {
      let pusher = new Pusher("7e89464fbe97a05e77c3", {
        cluster: "ap2",
      });
      pusher.subscribe("pusherMap");
      pusher.bind("locationChanged", (data) => {
        var deltalat = (data.latitude - this.location.lat) / 100;
        var deltalng = (data.longitude - this.location.lng) / 100;
        for (var i = 0; i < 100; i++) {
          (function (ind) {
            setTimeout(function () {
              this.location.lat += deltalat;
              this.location.lng += deltalng;
            }, 10 * ind);
          })(i);

          this.location.lat = data.latitude;
          this.location.lng = data.longitude;
        }

        // this.mockReviews.unshift(data.review);
      });
    },
    setPlace(loc) {
      this.currentLocation = loc;
    },
  },
};
</script>
