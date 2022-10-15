<script setup>
import Home from "./components/Home.vue";
import Order from "./components/Order.vue";
</script>

<template>
  <div class="container grid justify-items-center bg-gray-100 rounded-lg p-1 md:p-10 w-full">
    <Order v-if="hasId" :order="order"/>
    <Home v-else/>
  </div>
</template>
<script>
  import axios from "axios"
  export default {
    name: "App",
    data() {
        return {
            hasId: false,
            order: null,
        };
    },
    created() {
        this.getComponent();
    },
    methods: {
      getComponent(){
        const queryString = window.location.search;
        const urlParams = new URLSearchParams(queryString);
        this.hasId = !!urlParams.get("id")
        if (this.hasId) {
          axios
              .get(`http://82.180.136.47:30000/orders/${urlParams.get("id")}`)
              .then((res) => {
              this.order = res.data
          })
              .catch((error) => {
              console.log(error);
          });
        }
      }
    }
  }
</script>
