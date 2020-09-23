<template>  
  <v-card 
    :id='feature.values_.OBJECTID' 
    v-on:passingfilteditems
    max-width="300"
    class="mx-auto"
  >
     <v-toolbar
      color="teal"
      dark
    >
      <v-toolbar-title class="white--text">
        Defibrillator: {{feature.values_.OBJECTID}}
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon @click="closeCard">
        <v-icon>highlight_off</v-icon>
      </v-btn>
    </v-toolbar>

     <v-list subheader>
       <v-subheader>Adresse</v-subheader>
      <v-list-tile>
        <v-list-tile-action>
              <v-icon>room</v-icon>
            </v-list-tile-action>
        <v-list-tile-content>
          <v-list-tile-title>
            {{feature.values_.STRASSE}}
          </v-list-tile-title>
        </v-list-tile-content>
      </v-list-tile>
      <v-list-tile>
        <v-list-tile-action>
              <v-icon>house</v-icon>
            </v-list-tile-action>
        <v-list-tile-content>
          <v-list-tile-title>
            {{feature.values_.GEBAEUDE}}
          </v-list-tile-title>
        </v-list-tile-content>
      </v-list-tile>

      <v-divider></v-divider>
      <v-subheader>Zugänglichkeit</v-subheader>
      <v-list-tile>
        <v-list-tile-content>
          <v-list-tile-sub-title 
            v-html="feature.values_.HINWEIS"
            class="wrap-text"
          >
          </v-list-tile-sub-title>
        </v-list-tile-content>
      </v-list-tile>

    </v-list>

    <v-divider></v-divider>


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
    let popupForSelectedObjects = new Overlay({id: me.feature.values_.OBJECTID,
      element: document.getElementById(me.feature.values_.OBJECTID)
    });
    let selectedObjectsCoordinates = me.feature.values_.geometry.getCoordinates();
    popupForSelectedObjects.setPosition(selectedObjectsCoordinates);
    me.$map.addOverlay(popupForSelectedObjects);
  },
  methods: {
    closeCard () {
      let me = this;
      let popupForSelectedObjects = new Overlay({id: me.feature.values_.OBJECTID,
        element: document.getElementById(me.feature.values_.OBJECTID)
      });
      popupForSelectedObjects.setPosition(undefined);
      return false;
    }
  }
}
</script>

<style scoped>
  .v-list__tile__content {
    justify-content: center;
    /* text-align: center; */
  }
  .wrap-text {
    -webkit-line-clamp: unset !important;
    white-space: normal;
    overflow: scroll;
  }
  .theme--light.v-subheader {
    color: teal;
  }
</style>