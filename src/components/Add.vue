<template>
  <div class="add container">
     <Alert v-if="alert" v-bind:message="alert" />
      <h1 class="page-header">Add Stop</h1>
      <form v-on:submit="addStop">
         <div class="well">
            <h4>Stop Info</h4>
            <div class="form-group">
               <button type="submit" class="btn btn-success">Map</button>
            </div>
            <div class="form-group">
               <label>Latitude</label>
               <input type="text" class="form-control" placeholder="Latitude" v-model="stop.lat">
            </div>
            <div class="form-group">
               <label>Longitude</label>
               <input type="text" class="form-control" placeholder="Longitude" v-model="stop.long">
            </div>

            <div class="form-group">
               <label>Status</label>
               <br>
               <label><input type="radio" id="radio" name="status" value="true" v-model="stop.status"/>Active</label>
               <br>
               <label><input type="radio" id="radio" name="status" value="false" v-model="stop.status" />Not Active</label>
               <!-- <input type="text" class="form-control" placeholder="status" v-model="stop.status"> -->
            </div>
            <div class="form-group">
                  <label>Stop Number</label>
                  <input type="number" class="form-control" placeholder="Stop number" v-model="stop.num_stop">
            </div>
            <div class="form-group">
                  <label>Name</label>
                  <input type="text" class="form-control" placeholder="Name" v-model="stop.name">
            </div>
         </div>

         <button type="submit" class="btn btn-primary">Submit</button>
      </form>
   </div>
    
</template>

<script>

import Alert from './Alert'
import axios from 'axios'

const BASE_URL = 'http://ec2-18-219-95-88.us-east-2.compute.amazonaws.com:3000/';

export default {
  name: 'add',
  data() {
    return {
      stop: {
         lat: '',
         long: '',
         status: 'true',
         num_stop: '',
         name: ''
      },
      alert: ''
    }
  },
  methods: {
     addStop(e) {
         console.log(this.stop.status)
         if(!this.stop.lat || !this.stop.long || !this.stop.num_stop || !this.stop.name){
           this.alert = 'Please fill in all required fields';
         } else if(isNaN(this.stop.lat)) {
           this.alert = 'Please enter a valid number in Latitude';
         } else if(isNaN(this.stop.long)) {
           this.alert = 'Please enter a valid number in Longitude';
         } else if(!Number.isInteger(parseInt(this.stop.num_stop))) {
           this.alert = 'Please enter a valid number in Stop Number';
         } else if(parseInt(this.stop.num_stop) < 0) { 
           this.alert = 'Please enter a positive number in Stop Number';
        } else {
           //this.$http.post('http://localhost/stops/public/api/stops/add', newStop)
            axios({
               method: 'post',
               url:  `${BASE_URL}stops`,
               headers: {}, 
               data: {
                  lat: this.stop.lat,
                  long: this.stop.long,
                  eta_stop: this.stop.eta_stop,
                  long_stop: this.stop.long_stop,
                  status: this.stop.status,
                  num_stop: this.stop.num_stop,
                  name: this.stop.name
               }
            });
            this.$router.push({path:'/', query: {alert: 'Stop Added'}});
         e.preventDefault();
        }
        e.preventDefault();
     }
  },
  components: {
     Alert
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>