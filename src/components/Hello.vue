<template>
  <div class="hello">
    <!-- <h1>{{ msg }}</h1> -->
    <div class="row">
      <div class="col-md-12">
        <gmap-map style="width: 100%; height: 450px;"
            :center="{lat: 41.8781, lng: -87.629798}"
            :zoom="12" :options="{styles: mapStyles}"
        >
        <gmap-info-window :options="infoOptions" :position="infoWindowPos"v-if="infoWindowPos" :opened="infoWindowPos" :content="infoContent" @closeclick="infoWinOpen=false"></gmap-info-window>
        <gmap-marker  :key="i" v-for="(m,i) in markers" :position="m.position" @click="infoWindowPos = m.position, infoContent = m.type + ': ' + m.description +  '<br/><br/>'+ ' Date: ' + m.date" ></gmap-marker>
        </gmap-map>
      </div>
    </div>
    <div class="row">
      <div class="col-md-6">

      </div>
    </div>
  </div>
</template>

<script>
import * as VueGoogleMaps from 'vue2-google-maps'
import moment from 'moment'
import axios from 'axios'
import Vue from 'vue'

  Vue.use(VueGoogleMaps, {
    load: {
      key: 'AIzaSyAeRd_fOOHn4GCQSeVhgm7ioUjXypqj2C8',
      // libraries: 'places', //// If you need to use place input
    }
  })

export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      markers: [],
      infoContent: '',
      infoWindowPos: false,
      infoWinOpen: false,
      currentMidx: null,
      //optional: offset infowindow so it visually sits nicely on top of our marker
      infoOptions: {
      pixelOffset: {
        width: 0,
        height: -35
      }
    }
    }
  },
  created() {
    var lastDate = moment().subtract('8', 'days').format("YYYY-MM-DD") + "T23:59:59"
    var firstDate = moment().subtract('15', 'days').format('YYYY-MM-DD') +"T00:00:00"
    axios.get('https://data.cityofchicago.org/resource/6zsd-86xi.json?$where=date between ' + '"' + firstDate + '"' + ' and ' + '"' + lastDate + '"')
    .then(response =>  {
      console.log(response)
      var newData = response.data.map(function(e) {
        return { 
          position: {lat: parseFloat(e.latitude) , lng: parseFloat(e.longitude)},
          type: e.primary_type,
          description: e.description,
          date: e.date.split('T').splice(0, 1)
        }
      })
      
      this.markers = newData
  
    })
    .catch(function(err) {
      console.error(err)
    })
  },
  computed: { 
    mapStyles() {
      return [{
          stylers: [{
            hue: '#890000'
          }, {
            visibility: 'on'
          }, {
            gamma: 0.5
          }, {
            weight: 0.5
          }]
        }, {
          elementType: 'labels',
          stylers: [{
            visibility: 'off'
          }]
        }, {
          featureType: 'water',
          stylers: [{
            color: '#890000'
          }]
        }];
    }
  },
  methods: {
   
  }
}
Vue.component('google-map', VueGoogleMaps.Map);
Vue.component('google-marker', VueGoogleMaps.Marker);
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
