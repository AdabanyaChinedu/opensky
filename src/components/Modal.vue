<template>
  <md-dialog :md-active.sync="showDialog">
    <md-dialog-title
      ><span><img :src="logo" alt="logo"/></span><span>{{ airline }}</span
      ><span>{{ "ICAO - " + icao }}</span></md-dialog-title
    >

    <md-tabs md-dynamic-height>
      <md-tab md-label="Departed Flights" @click="setFlightType('departed')">
        <md-field>
          <label for="time">Flights in the past x-minutes</label>
          <md-select v-model="dateSelected" name="dtime" id="dtime">
            <md-option value="5">5 minutes</md-option>
            <md-option value="10">10 minutes</md-option>
            <md-option value="15">15 minutes</md-option>
            <md-option value="30">30 minutes</md-option>
            <md-option value="45">45 minutes</md-option>
            <md-option value="60">60 minutes</md-option>
          </md-select>
        </md-field>

        <md-table v-model="flights" md-card md-fixed-header>
          <md-table-toolbar>
            <h1 class="md-title">Departed Flight Info</h1>
          </md-table-toolbar>

          <template v-if="flights.length < 0">
            <md-table-row slot="md-table-row" slot-scope="{ flight }">
              <md-table-cell
                md-label="ICAO No"
                md-sort-by="icao24"
                md-numeric
                >{{ flight.icao24 }}</md-table-cell
              >
              <md-table-cell md-label="First Seen" md-sort-by="firstSeen">{{
                flight.firstSeen
              }}</md-table-cell>
              <md-table-cell md-label="Last Seen" md-sort-by="lastSeen">{{
                flight.lastSeen
              }}</md-table-cell>
              <md-table-cell md-label="Call Sign" md-sort-by="callsign">{{
                flight.callsign
              }}</md-table-cell>
              <md-table-cell
                md-label="Candidate Count"
                md-sort-by="candidateCount"
                >{{ flight.arrivalAirportCandidatesCount }}</md-table-cell
              >
            </md-table-row>
          </template>
          <template v-else>
            <md-table-row slot="md-table-row">
              <span>
                {{ airline }} has No Flight Info Data Available in the past <span>
                {{ dateSelected }}</span> minutes
              </span>
            </md-table-row>
          </template>
        </md-table>
      </md-tab>
      <md-tab md-label="Arrived Flights" @click="setFlightType('arrived')">
        <md-field>
          <label for="time'">Flights in the past x-minutes</label>
          <md-select v-model="dateSelected" name="atime" id="atime">
            <md-option value="5">5 minutes</md-option>
            <md-option value="10">10 minutes</md-option>
            <md-option value="15">15 minutes</md-option>
            <md-option value="30">30 minutes</md-option>
            <md-option value="45">45 minutes</md-option>
            <md-option value="60">60 minutes</md-option>
          </md-select>
        </md-field>

        <md-table v-model="flights" md-card>
          <md-table-toolbar>
            <h1 class="md-title">Arrived Flight Info</h1>
          </md-table-toolbar>

          <template v-if="flights.length > 0">
            <md-table-row slot="md-table-row" slot-scope="{ flight }">
              <md-table-cell
                md-label="ICAO No"
                md-sort-by="icao24"
                md-numeric
                >{{ flight.icao24 }}</md-table-cell
              >
              <md-table-cell md-label="First Seen" md-sort-by="firstSeen">{{
                flight.firstSeen
              }}</md-table-cell>
              <md-table-cell md-label="Last Seen" md-sort-by="lastSeen">{{
                flight.lastSeen
              }}</md-table-cell>
              <md-table-cell md-label="Call Sign" md-sort-by="callsign">{{
                flight.callsign
              }}</md-table-cell>
              <md-table-cell
                md-label="Candidate Count"
                md-sort-by="candidateCount"
                >{{ flight.arrivalAirportCandidatesCount }}</md-table-cell
              >
            </md-table-row>
          </template>
          <template v-else>
            <md-table-row slot="md-table-row">
              <span>
                {{ airline }} has No Flight Info Data Available in the past <span>
                {{ dateSelected }}</span> minutes
             </span>
            </md-table-row>
          </template>
        </md-table>
      </md-tab>
    </md-tabs>

    <md-dialog-actions>
      <md-button class="md-primary" @click="showDialog = false"
        >Close</md-button
      >
    </md-dialog-actions>
  </md-dialog>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "Modal",
  data: function() {
    return {
      output: [],
      dateSelected: "",
      airline: "",
      logo: "",
      icao: "",
      flightType: "",
      begin: "",
      end: "",
      baseUrl: "https://opensky-network.org/api/flights/",
      showDialog: false,
      modalStatus: false,
      menuVisible: false,
      flights: [
        {
          icao24: 1,
          firstSeen: "123234",
          lastSeen: "4242424242",
          callsign: "Male",
          arrivalAirportCandidatesCount: "Media Planner"
        },
        {
          icao24: 2,
          firstSeen: "57875856",
          lastSeen: "2433455465",
          callsign: "Male",
          arrivalAirportCandidatesCount: "Assistant  Planner"
        },
        {
          icao24: 3,
          firstSeen: "246555",
          lastSeen: "65275767676",
          callsign: "Male",
          arrivalAirportCandidatesCount: "Assistant Media"
        }
      ]
    };
  },
  mounted() {
    // let vm = this;
    // axios.get(vm.url).then(function(response) {
    // vm.output = response.data[0].icao24;
    // });
  },

  computed: {
    url: function() {
      return (
        this.baseUrl +
        this.basepath +
        this.airportparam +
        this.beginparam +
        this.endparam
      );
    },
    basepath: function() {
      return this.flightType == "arrived" ? "arrival?" : "departure?";
    },
    airportparam: function() {
      return "airport=" + this.icao;
    },

    beginparam: function() {
      return "&begin=" + this.begin;
    },
    endparam: function() {
      return "&end=" + this.end;
    }
  },

  methods: {
    toggleModal: function() {
      this.showDialog = true;
      this.modalStatus = !this.modalStatus;
    },
    toggleMenu() {
      this.menuVisible = !this.menuVisible;
    },
    setFlightType(flightType) {
      this.flightType = flightType;
      this.dateSelected = "5";
    },
    getFlightInfo(begin) {
      this.begin = moment()
        .subtract(begin, "minutes")
        .unix();
      this.end = moment().unix();
      let vm = this;
      axios.get(vm.url).then(function(response) {
        // vm.flights = response.data;
        alert(response.data);
      });
    }
  },
  watch: {
    dateSelected: function() {
      this.getFlightInfo(this.dateSelected);
    }
  }
};
</script>
