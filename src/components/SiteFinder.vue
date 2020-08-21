<template>
  <div>
    <h2
      :id="titleId"
      class="bmn"
    >
      {{ title.charAt(0).toUpperCase() + title.slice(1) }}
    </h2>
    <div>
      <v-client-table
        :ref="titleId"
        :name="titleId"
        :data="documentTable"
        :columns="columns"
        :options="options"
      >
        <a
          :id="row.id"
          slot="title"
          slot-scope="{row}"
          :href="row.link"
        ><span class="title">{{ row.title }}</span></a>
        <a
          slot="format"
          slot-scope="{row}"
          class="text-uppercase"
          :href="row.link"
        >{{ row.format }} <i class="fas fa-download fa-fw" /></a>
        <div slot="afterFilter">
          <span><i class="fas fa-search fa-2x" /></span>
        </div>
      </v-client-table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SiteFinder',
  props: {
    documentTable: {
      type: Array,
      default: () => {
        return [];
      },
    },
    query: {
      type: String,
      default: () => {
        return null;
      },
    },
    titleHash: {
      type: String,
      default: () => {
        return null;
      },
    },
  },
  data: function() {
    return {
      columns: [ 'Asset_Name', 'Address' ],
      options: {
        texts: {
          filterPlaceholder: "Begin typing to filter by site name or address",
        },
        perPage: 400,
        perPageValues: [],
      },
      titleId: this.titleHash.split(' ').join('-').toLowerCase(),
      title: "document table",
    };
  },
};
</script>