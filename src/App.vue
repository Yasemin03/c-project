<style>
.pac-container {
  z-index: 9999999999 !important;
  position: relative;
}
.offcanvas-backdrop {
  display: none !important;
}
.offcanvas-size-l {
  --bs-offcanvas-width: min(95vw, 60vw) !important;
}
.offcanvas-size-md {
  --bs-offcanvas-width: min(95vw, 400px) !important;
}
</style>
<template style="height: 100%; margin: 0">
  <header>
    <div
      class="px-3 py-2 text-bg-dark"
      style="background-color: #ff5b15 !important"
    >
      <div class="container">
        <div
          class="d-flex flex-wrap align-items-center justify-content-center justify-content-lg-start"
        >
          <a
            href="/"
            class="d-flex align-items-center my-2 my-lg-0 me-lg-auto text-white text-decoration-none"
          >
            <h3>C-Project</h3>
          </a>
          <div class="d-flex col-lg-6" style="justify-content: space-between">
            <div class="form-group">
              <input
                style="
                  border: 1px solid rgb(92, 92, 178);
                  border-radius: 5px;
                  margin-right: 10px;
                "
                type="text"
                class="form-control"
                ref="from"
                placeholder="Origin Location"
                id="origin"
              />
            </div>
            <div class="form-group">
              <input
                style="border: 1px solid rgb(92, 92, 178); border-radius: 5px"
                type="text"
                class="form-control"
                ref="to"
                placeholder="Destination Location"
                id="destination"
              />
            </div>
            <button
              class="btn btn-light"
              ref="createRoute"
              @click.prevent="routeReady ? refreshAll() : calcRoute()"
            >
              Create Route
            </button>
          </div>
          <ul
            class="nav col-12 col-lg-auto my-2 justify-content-center my-md-0 text-small"
          >
            <li>
              <a
                href="#"
                data-bs-toggle="offcanvas"
                data-bs-target="#yolcuekle"
                aria-controls="yolcuekle"
                class="nav-link text-white"
              >
                <i class="bi bi-plus"></i>
                Add Passenger
              </a>
            </li>
            <li>
              <a
                data-bs-toggle="offcanvas"
                data-bs-target="#yolculist"
                aria-controls="yolculist"
                class="nav-link text-white"
              >
                <i class="bi bi-clipboard"></i>
                Passanger List
              </a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </header>
  <div style="height: 100%; font-family: Lucida Sans, sans-serif; margin: 0">
    <div
      class="offcanvas offcanvas-size-md offcanvas-start"
      tabindex="-1"
      id="yolcuekle"
      aria-labelledby="yolcuekleLabel"
    >
      <div class="offcanvas-header">
        <h5 class="offcanvas-title" id="yolcuekleLabel">Add Passenger</h5>
        <button
          type="button"
          class="btn-close text-reset"
          data-bs-dismiss="offcanvas"
          aria-label="Close"
        ></button>
      </div>
      <div class="offcanvas-body">
        <div class="container">
          <form>
            <div class="form-group">
              <label for="name">Passenger Name:</label>
              <input
                style="border: 1px solid rgb(92, 92, 178); border-radius: 5px"
                type="text"
                class="form-control"
                ref="passengerName"
                v-model="passengerName"
                placeholder="Passenger Name"
              />
            </div>
            <div class="form-group">
              <label for="from">From:</label>
              <input
                style="border: 1px solid rgb(92, 92, 178); border-radius: 5px"
                type="text"
                class="form-control"
                ref="pickup"
                placeholder="Origin Location"
                id="pickup"
              />
            </div>

            <button
              class="btn"
              @click.prevent="addPassanger"
              style="
                border: 1px solid white;
                border-radius: 10px;
                background-color: rgb(31, 87, 210);
                font-weight: bold;
                color: white;
                margin-top: 10px;
                width: 100%;
              "
            >
              Add Passenger
            </button>
          </form>
        </div>
      </div>
    </div>
    <div
      class="offcanvas offcanvas-size-l offcanvas-end"
      tabindex="-1"
      id="yolculist"
      aria-labelledby="yolculistLabel"
    >
      <div class="offcanvas-header">
        <button
          type="button"
          class="btn-close text-reset"
          data-bs-dismiss="offcanvas"
          aria-label="Close"
        ></button>
        <h5 class="offcanvas-title" id="yolculistLabel">Passenger List</h5>
      </div>
      <div class="offcanvas-body">
        <div class="container">
          <div class="container-fluid">
            <input type="text" v-model="search" placeholder="Search" />
            <hr />
            <div id="passengerTable">
              <table class="table table-bordered">
                <thead>
                  <tr>
                    <th>Passenger Name</th>
                    <th>PickUp Address</th>
                    <th>Duration</th>
                    <th>Order</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(passenger, index) in passengers" :key="index">
                    <td>{{ passenger.name }}</td>
                    <td>{{ passenger.address }}</td>
                    <td>
                      {{
                        new Date(passenger.tripDuration.totalDuration * 1000)
                          .toISOString()
                          .slice(11, 19)
                      }}
                    </td>
                    <td>{{ passenger.order }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div id="googleMap" style="width: 100%; height: calc(100vh - 57px)"></div>
    <div
      style="
        position: absolute;
        left: 0;
        bottom: 0;
        width: 45%;
        background-color: white;
      "
    >
      <div style="display: flex; justify-content: space-between; padding: 10px">
        <span
          >Total Km: <span>{{ this.totalKm.toFixed(2) }} km</span></span
        >
        <span
          >Total Duration:
          <span>{{
            new Date(this.tripDuration * 1000).toISOString().slice(11, 19)
          }}</span></span
        >
        <span
          >Total Passengers: <span>{{ this.passengers.length }}</span></span
        >
      </div>
    </div>
  </div>
</template>

<script>
import { Loader } from "@googlemaps/js-api-loader";
export default {
  data() {
    return {
      passengers: [],
      pickUps: [],
      google: null,
      routeReady: false,
      map: null,
      director: null,
      mapOptions: {
        center: { lat: 41.015137, lng: 28.97953 },
        zoom: 10,
        travelMode: "DRIVING",
        mapTypeControl: false,
        scaleControl: false,
        streetViewControl: false,
        rotateControl: false,
        fullscreenControl: false,
      },
      origin: null,
      destination: null,
      tripDuration: 0,
      totalKm: 0,
      markers: [],
      currentMarker: null,
      currentLocation: null,
      search: "",
      searchOld: "",
    };
  },
  watch: {
    search: function (val) {
      console.log(val);
      if (val != "")
        this.passengers = this.passengers.filter((e) =>
          e.name.toLowerCase().match(val.toLowerCase())
        );
      if (val == "") this.passengers = this.searchOld;
    },
  },
  methods: {
    removeAllMarkers() {
      return new Promise((resolve) => {
        for (let index = 0; index < this.markers.length; index++) {
          const element = this.markers[index].marker;
          element.visible = false;
          element.setMap(null);
          this.map.setCenter(this.mapOptions.center);
        }
        resolve(true);
      });
    },
    refreshAll() {
      window.location.reload();
    },
    async addPassanger() {
      if (!this.routeReady) {
        alert("Please create route first!");
      } else if (this.passengers.length >= 9) {
        alert("You can add 9 passengers at most!");
      } else if (this.currentLocation != "") {
        let duration = await this.calculateDistances(
          [],
          this.currentLocation.geometry.location
        );
        this.passengers.push({
          name: this.passengerName,
          pickupPoint: this.currentLocation.geometry.location,
          tripDuration: duration,
          order: this.passengers.length + 1,
          address: this.currentLocation.formatted_address,
        });
        this.searchOld = this.passengers;
        this.passengerName = "";
        this.currentMarker = null;
        this.currentLocation = null;
        this.$refs.pickup.value = "";
        await this.removeAllMarkers();
        this.displayRoute();
      } else {
        alert("Please fill all fields!");
      }
    },
    calcRoute() {
      if (this.passengers.length >= 9) {
        alert("You can add 9 passengers at most!");
        return "not ok";
      }
      const directionsService = new this.google.maps.DirectionsService();
      this.director = new this.google.maps.DirectionsRenderer();
      const map = this.map;
      this.director.setMap(map);
      const request = {
        origin: this.$refs.from.value,
        destination: this.$refs.to.value,
        travelMode: this.mapOptions.travelMode,
      };

      directionsService.route(request, async (result, status) => {
        if (status === "OK") {
          // distance duration hesaplama
          const legs = await this.makeCalculation(result.routes[0].legs);

          if (legs.totalDuration > 60 * 60 * 2) {
            await this.removeAllMarkers();
            alert("Driving time cannot exceed 2 hours!");
          } else {
            this.tripDuration = legs.totalDuration;
            this.totalKm = legs.totalKm / 1000;
            this.director.setDirections(result);
            this.$refs.from.disabled = true;
            this.$refs.to.disabled = true;
            this.$refs.createRoute.innerText = "Refresh";
            this.routeReady = true;
          }
        } else {
          console.log("Directions request failed due to " + status);
        }
      });
    },
    makeCalculation(rows) {
      return new Promise((resolve) => {
        let totalKm = 0;
        let totalDuration = 0;
        rows.forEach((element) => {
          totalDuration += element.duration.value;
          totalKm += element.distance.value;
        });
        resolve({ totalKm, totalDuration });
      });
    },
    calculateDistances(ways, origin) {
      return new Promise((resolve, reject) => {
        const directionsService = new this.google.maps.DirectionsService();
        const request = {
          origin: origin,
          destination: this.destination,
          waypoints: ways,
          optimizeWaypoints: true,
          travelMode: this.mapOptions.travelMode,
        };

        directionsService.route(request, async (result, status) => {
          if (status === "OK") {
            resolve(await this.makeCalculation(result.routes[0].legs));
          } else {
            console.log("Directions request failed due to " + status);
            reject(status);
          }
        });
      });
    },

    async displayRoute() {
      const map = this.map;
      let ways = this.passengers.map((passenger) => {
        return {
          location: passenger.address,
          stopover: true,
        };
      });
      console.log(ways);
      let distance = await this.calculateDistances(ways, this.origin);
      this.totalKm = distance.totalKm / 1000;
      this.tripDuration = distance.totalDuration;
      const waypoints = [];

      for (let i = 0; i < this.passengers.length; i++) {
        waypoints.push({
          location: this.passengers[i].address,
          stopover: true,
        });
      }

      const directionsService = new this.google.maps.DirectionsService();
      this.director.setMap(null);
      const request = {
        origin: this.origin,
        destination: this.destination,
        waypoints: waypoints,
        optimizeWaypoints: true,
        travelMode: this.mapOptions.travelMode,
      };
      console.log(request);
      directionsService.route(request, (result, status) => {
        console.log(result, status);
        if (status === "OK") {
          this.director.setMap(map);
          this.director.setDirections(result);
        } else {
          console.log("Directions request failed due to " + status);
        }
      });
    },

    async geocodeAddress(geocoder, address) {
      return new Promise((resolve, reject) => {
        geocoder.geocode({ address: address }, (results, status) => {
          if (status === "OK") {
            resolve(results[0].geometry.location);
          } else {
            reject(status);
          }
        });
      });
    },
  },

  mounted() {
    const loader = new Loader({
      apiKey: "AIzaSyBTsiAJPDg62Y7SHDMs963UeWmRQNQxrPw",
      version: "weekly",
      libraries: ["places"],
    });

    loader.load().then((google) => {
      this.google = google;
      const map = new google.maps.Map(
        document.getElementById("googleMap"),
        this.mapOptions
      );
      this.map = map;
      const autocompleteOrigin = new google.maps.places.Autocomplete(
        this.$refs.from
      );
      const autocompleteDestination = new google.maps.places.Autocomplete(
        this.$refs.to
      );
      const autocompletePickup = new google.maps.places.Autocomplete(
        this.$refs.pickup
      );

      autocompleteOrigin.setFields(["place_id", "geometry"]);
      autocompleteDestination.setFields(["place_id", "geometry"]);

      google.maps.event.addListener(autocompleteOrigin, "place_changed", () => {
        const place = autocompleteOrigin.getPlace();
        let available = this.markers.find((marker) => {
          return marker.name === "origin";
        });
        if (available) {
          available.marker.setPosition(place.geometry.location);
        } else {
          var marker = new google.maps.Marker({
            position: place.geometry.location,
            map: map,
            id: "origin",
          });
          this.markers.push({ marker: marker, name: "origin" });
        }

        map.setZoom(10);
        map.setCenter(place.geometry.location);
        let lat = place.geometry.location.lat().toString();
        let lng = place.geometry.location.lng().toString();
        this.origin = `${lat},${lng}`;
      });

      google.maps.event.addListener(
        autocompleteDestination,
        "place_changed",
        () => {
          const place = autocompleteDestination.getPlace();
          let available = this.markers.find((marker) => {
            return marker.name === "destination";
          });
          if (available) {
            available.marker.setPosition(place.geometry.location);
          } else {
            var marker = new google.maps.Marker({
              position: place.geometry.location,
              map: map,
              id: "destination",
            });
            this.markers.push({ marker: marker, name: "destination" });
          }
          map.setZoom(10);
          map.setCenter(place.geometry.location);
          let lat = place.geometry.location.lat().toString();
          let lng = place.geometry.location.lng().toString();
          this.destination = `${lat},${lng}`;
        }
      );
      google.maps.event.addListener(
        autocompletePickup,
        "place_changed",
        async () => {
          const place = autocompletePickup.getPlace();
          const destinations = this.passengers.map((passenger) => {
            return {
              location: passenger.address,
              stopover: true,
            };
          });
          destinations.push({
            location: place.formatted_address,
            stopover: true,
          });
          console.log(888, destinations);
          const distance = await this.calculateDistances(
            destinations,
            this.origin
          );
          console.log(888);
          if (distance.totalDuration > 60 * 60 * 2) {
            alert("Driving time cannot exceed 2 hours!");
            this.$refs.pickup.value = "";
          } else {
            if (this.currentMarker != null) {
              this.currentMarker.setPosition(place.geometry.location);
            } else {
              this.currentMarker = new google.maps.Marker({
                position: place.geometry.location,
                map: map,
                id: "passenger",
                labelContent:
                  "<div class='arrow'></div><div class='inner'>$425K</div>",
                labelAnchor: new google.maps.Point(0, 0),
                labelClass: "labels",
              });
              this.markers.push({
                name: "passenger",
                marker: this.currentMarker,
              });
            }
            map.setZoom(10);
            map.setCenter(place.geometry.location);
            this.currentLocation = place;
            this.passengerName = this.$refs.passengerName.value;
          }
        }
      );
    });
  },
};
</script>
