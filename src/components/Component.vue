<template>
  <node-view-wrapper class="vue-component">
    <div class="content">
      <input
        v-if="!src"
        type="search"
        placeholder="Search Images"
        v-model="search"
      />
      <button v-if="!src" @click="searchMeme" class="btn">
        <i class="fas fa-search"></i>
      </button>

      <img v-if="src" :src="src" height="250px" width="250px" /><br />
      <button v-if="src" @click="src = ''" class="btn">
        <i class="far fa-times-circle"></i>
      </button>
    </div>
  </node-view-wrapper>
</template>

<script>
import { NodeViewWrapper, nodeViewProps } from "@tiptap/vue-2";
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);
export default {
  components: {
    NodeViewWrapper,
  },

  props: nodeViewProps,
  data() {
    return {
      search: "",
      src: "",
    };
  },
  methods: {
    searchMeme() {
      Vue.axios
        .get(
          "https://api.unsplash.com/search/photos/?client_id=KX3FHd3z3Wy_RDO9MRmRjnU3gwtJqluw9XH1arlEsNI&query=" +
            this.search
        )
        .then((response) => {
          this.src = response.data.results[0].urls.full;
          //console.log(response.data.results[0].urls.full);
        });
    },
  },
};
</script>
