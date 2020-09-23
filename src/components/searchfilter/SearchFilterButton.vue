<template>
    <v-badge overlap color="blue">
          <template v-slot:badge v-if="selectedFeatures.length > 0">
            <span>{{selectedFeatures.length}}</span>
          </template>
          <v-menu 
            :close-on-content-click="false" 
            :close-on-click="true"
            :nudge-left="250"  offset-x offset-y>
              <v-btn icon dark slot="activator">
              <v-icon medium>{{icon}}</v-icon>
              </v-btn>
              <wgu-searchfiltercard v-on:searchFilterItemsChanged="updateBadgeCounter"> </wgu-searchfiltercard>
          </v-menu>
    </v-badge>
</template>

<script>
import SearchFilterCard from './SearchFilterCard'
import { WguEventBus } from '../../WguEventBus'

export default {
  name: 'wgu-searchfilter-btn',
  components: {
    'wgu-searchfiltercard': SearchFilterCard
  },
  props: {
    icon: {type: String, required: false, default: 'filter_alt'}
  },
  data () {
    return {
      selectedFeatures: []
    }
  },
  created () {
    var me = this;
    WguEventBus.$on('ol-map-mounted', olMap => {
      me.map = olMap;
    });
  },
  methods: {
    updateBadgeCounter (selectedObjects) {
      let me = this;
      me.selectedFeatures = selectedObjects;
      me.$emit('filterupdatedtomain', selectedObjects);
    }
  }
}
</script>