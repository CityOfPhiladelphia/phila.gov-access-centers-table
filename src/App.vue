<template>
  <div id="vue-app">
    <div
      data-swiftype-index="true"
      class="entry-content"
    >
      <!-- Site finder -->
      <section class="mtl">
        <div class="large-24 columns">
          <SiteFinder
            :document-table="documentTables"
            :query="query"
            title-hash="Access centers"
          />
        </div>
      </section>
      <!-- Site finder -->
    </div> <!-- End .entry-content -->
  </div>
</template>

<script>
import Vue from 'vue';
import axios from 'axios';
import SiteFinder from './components/SiteFinder.vue';
import { ClientTable } from 'vue-tables-2';
// import { _ } from 'vue-underscore';

Vue.use(ClientTable, {
  options: {}, 
  useVuex: false,
  theme: 'bootstrap3',
  template: 'default',
});

export default {
  name: 'App',
  components: {
    SiteFinder,
  },
  data () {
    return {
      api: {
        getDocumentTables: 'https://staging-admin.phila.gov/wp-json/documents/v1/document',
      },
      documentTables: [],
      query: null,
    };
  },
  created () {
    let vm = this;
    vm.init();
  },
  methods: {
    getDocumentTables () {
      let vm = this;
      let slug = window.location.pathname;
      slug = 'https://services.arcgis.com/fLeGjb7u4uXqeF9q/arcgis/rest/services/Digital_Access_Centers_2020_PUBLICVIEW/FeatureServer/0/query?where=1%3D1&outFields=*&f=pjson';
      return axios.get(slug).then(async (result) => {
        
        let tempData = result.data.features.map(item => {
          return item.attributes;
        });

      tempData.forEach(function(site) {
          site['Site name'] = site['Asset_Name'];
          delete site['Asset_Name'];

          site['ZIP Code'] = site['CODE'];
          delete site['CODE'];
        });

     
        vm.documentTables = tempData;
        return true;
      }).catch(() => {
        return false;
      });
    },
    async parseQuery () {
      let vm = this;
      vm.query = vm.$route.hash;
    },
    async init () {
      let vm = this;
      await vm.parseQuery();
      await vm.getDocumentTables();
      await vm.scrollToDiv(); 
    },
    async scrollToDiv() {
      let vm = this;
      let div = vm.$route.hash.split('#')[1];
      if ( div ) {
        let distance = document.getElementById(div).offsetTop;
        window.scrollTo({
          top: distance,
          behavior: 'smooth',
        });
      }
    },
  },
};
</script>