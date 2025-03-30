<template>
  <div id="setting">
    <my-setting />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import MySetting from '@/components/MySetting.vue' // @ is an alias to /src

export default Vue.extend({
  name: 'SettingView',
  components: {
    MySetting
  }
})
</script>
