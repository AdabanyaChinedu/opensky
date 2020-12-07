<template>
  <div>
    <Grid ref="grid_comp" />
    <Modal ref="modal_comp" />
    
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import Grid from "./Grid.vue";
import Modal from "./Modal.vue";

export default {
  name: "base",
  components: { Grid, Modal },
  mounted() {
    this.$watch("$refs.grid_comp.modalStatus", () => {
      const gridRef = this.$refs.grid_comp;
      const modalRef = this.$refs.modal_comp;
      modalRef.showDialog = true;
      modalRef.icao = gridRef.icao;
      modalRef.airline = gridRef.airport;
      modalRef.logo = gridRef.logo;
      modalRef.dateSelected = "5";
      modalRef.getFlightInfo("5");
      //this.$refs.grid_comp.getEngagements();
      //this.$refs.grid_comp.getEngagements();
    });
  },
   
};
</script>

<style>
/* #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
 
} */
</style>
