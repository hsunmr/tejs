<template>
   <button type="button" class="ui green button" style="float:right" v-on:click="download">下載 Excel</button>
</template>

<script>
import {
  host,
  model,
  id,
  apiPath
} from './../config';
import qs from "qs";
export default {
  props: ['routes','params', 'queryObj'],

  methods: {
    download: function () {
      let query = qs.stringify(this.queryObj, {skipNulls: true});
      let apiUrl = apiPath(false)+this.routes;
      axios.post(apiUrl + '/excelExport?'+ query)
        .then((response) => {
          let content_disposition = response.headers['content-disposition'];
          let filename = content_disposition.split('filename="')[1].split('"')[0];

          const url  = window.URL.createObjectURL(new Blob(["\ufeff" + response.data]));
          const link = document.createElement('a');

          link.href = url;
          link.setAttribute('download', filename);
          document.body.appendChild(link);
          link.click();

          window.URL.revokeObjectURL(url);
        })
    }
  }
}
</script>