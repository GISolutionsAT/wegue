<template>  
  <v-card :id='feature.values_.OBJECTID' v-on:passingfilteditems>
    <v-card-title class="teal 100">
      <span class="headline white--text"> {{feature.values_.ADRESSE}}</span>
    </v-card-title>
    <!-- <v-img
      src="https://unsplash.com/t/nature"
      height="100px"></v-img> -->

  </v-card>
</template>

<script>
import Overlay from 'ol/Overlay';
import { WguEventBus } from '../../WguEventBus'
export default {
  name: 'map-overlay',
  props: {
    feature: null
  },
  created () {
    var me = this;
    WguEventBus.$on('ol-map-mounted', olMap => {
      me.map = olMap;
    });
  },
  mounted () {
    let me = this;
    // create a map overlay of this item
    console.log('in myFunction');
    console.log(me.feature);
    let popupForSelectedObjects = new Overlay({id: me.feature.values_.OBJECTID,
      element: document.getElementById(me.feature.values_.OBJECTID)
    });
    let selectedObjectsCoordinates = me.feature.values_.geometry.getCoordinates();
    popupForSelectedObjects.setPosition(selectedObjectsCoordinates);
    me.$map.addOverlay(popupForSelectedObjects);
  }
}
</script>

<style>

</style>