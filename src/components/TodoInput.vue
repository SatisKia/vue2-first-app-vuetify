<template>
  <v-card-text class="ma-0 pa-0 text-body-1">
    <v-card-actions class="ma-0 pa-0 text-body-1">
      <v-card-text class="ma-0 pa-0 text-body-1" style="width:50%">
        <span>TODO:</span>
      </v-card-text>
      <v-card-text class="ma-0 pa-0 text-body-1" style="width:50%; text-align:right">
        <router-link to="/setting">設定</router-link>
      </v-card-text>
    </v-card-actions>
    <v-text-field class="mt-0 py-0" style="width:100%" v-model="text" placeholder="100文字まで" hide-details />
    <v-radio-group class="mt-0 py-1" row v-model="colorType" hide-details>
      <v-radio label="シアン" id="color1" value="#80ffff" />
      <v-radio label="マゼンタ" id="color2" value="#ff80ff" />
      <v-radio label="イエロー" id="color3" value="#ffff80" />
    </v-radio-group>
    <v-card-text class="ma-0 pa-0 text-body-1" v-if="!isError">
      <v-btn style="width:100%" v-on:click="onAdd" v-bind:disabled="(text.length == 0) || isError">登録</v-btn>
    </v-card-text>
    <v-card flat v-if="isError">
      <span class="error">入力できる文字は100文字までです</span>
    </v-card>
  </v-card-text>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data () {
    return {
      text: '',
      isError: false,
      colorType: '#80ffff'
    }
  },
  methods: {
    onAdd: function () {
      const input1 = document.getElementById('color1') as HTMLInputElement
      const input2 = document.getElementById('color2') as HTMLInputElement
      const input3 = document.getElementById('color3') as HTMLInputElement
      let color = input1.value
      if (input2.checked) {
        color = input2.value
      } else if (input3.checked) {
        color = input3.value
      }
      this.$emit('add', this.text, color)
      this.text = ''
    }
  },
  watch: {
    text (newVal) {
      if (newVal.length > 100) {
        this.isError = true
      } else {
        this.isError = false
      }
    }
  },
  beforeCreate: function () {
    console.log('TodoInput beforeCreate')
  },
  created: function () {
    console.log('TodoInput created')
  },
  beforeMount: function () {
    console.log('TodoInput beforeMount')
  },
  mounted: async function () {
    console.log('TodoInput mounted')
  },
  beforeUpdate: function () {
    console.log('TodoInput beforeUpdate')
  },
  updated: function () {
    console.log('TodoInput updated')
  },
  beforeDestroy: function () {
    console.log('TodoInput beforeDestroy')
  },
  destroyed: function () {
    console.log('TodoInput destroyed')
  }
})
</script>

<style scoped>
.error {
  color: #ff0000;
}
</style>
