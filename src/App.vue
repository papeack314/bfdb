<template>
  <div id="app">
    <Navibar />
    <b-container class="mt-3">
      <router-view />
    </b-container>
  </div>
</template>

<script>
import Navibar from "@/components/TheNavibar";

export default {
  components: {
    Navibar
  }
};
</script>
