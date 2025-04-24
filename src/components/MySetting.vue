<template>
  <v-card-text class="ma-0 pa-0 text-body-1">
    <v-btn style="width:100%" v-on:click="onSet">設定</v-btn>
    <v-card-text class="mt-3 pa-0 text-body-1">
      <span>年月日表記:</span>
    </v-card-text>
    <v-radio-group class="mt-0" row v-model="dateType" hide-details>
      <v-radio v-bind:label="dateType1String" v-bind:value="1" v-on:change="selectDateType1" />
      <v-radio v-bind:label="dateType2String" v-bind:value="2" v-on:change="selectDateType2" />
      <v-radio label="非表示" v-bind:value="3" v-on:change="selectDateType3" />
    </v-radio-group>
    <v-checkbox label="年を表示する" v-model="dispYear" v-on:change="changeCheckYear" hide-details />
    <v-card-text class="mt-3 pa-0 text-body-1">
      <v-btn v-on:click="onClear">ログの消去</v-btn>
    </v-card-text>
    <v-dialog v-model="dialog">
      <v-sheet>
        <v-card-text class="text-center">ログを消去してもよろしいですか？</v-card-text>
        <v-card-text class="text-center">
          <v-btn class="mr-5" v-on:click="onDialogOK()">はい</v-btn>
          <v-btn v-on:click="onDialogCancel()">キャンセル</v-btn>
        </v-card-text>
      </v-sheet>
    </v-dialog>
  </v-card-text>
</template>

<script lang="ts">
import Vue from 'vue'
import MyCookie from '@/plugins/Cookie'

export default Vue.extend({
  data () {
    return {
      dateType: 1,
      dispYear: true,
      dateType1String: 'YYYY年MM月DD日',
      dateType2String: 'YY/MM/DD',
      dialog: false
    }
  },
  methods: {
    onSet: function () {
      this.$store.dispatch('setDateType', { dateType: this.dateType })
      this.$store.dispatch('setDispYear', { dispYear: this.dispYear })

      const cookie: MyCookie = new MyCookie()
      cookie.setNumber('dateType', this.dateType)
      cookie.setBool('dispYear', this.dispYear)

      this.$router.back()
    },
    onClear: function () {
      // ダイアログを開く
      this.dialog = true
    },
    onDialogOK: function () {
      console.log('onDialogOK')

      this.$store.dispatch('clear')

      // データの書き込み
      this.$store.dispatch('saveData')

      // ダイアログを閉じる
      this.dialog = false
    },
    onDialogCancel: function () {
      // ダイアログを閉じる
      this.dialog = false
    },
    selectDateType1: function () {
      console.log('selectDateType1')
    },
    selectDateType2: function () {
      console.log('selectDateType2')
    },
    selectDateType3: function () {
      console.log('selectDateType3')
    },
    changeCheckYear: function () {
      console.log('changeCheckYear')
      this.dateType1String = this.dispYear ? 'YYYY年MM月DD日' : 'MM月DD日'
      this.dateType2String = this.dispYear ? 'YY/MM/DD' : 'MM/DD'
    }
  },
  beforeCreate: function () {
    console.log('MySetting beforeCreate')
  },
  created: function () {
    console.log('MySetting created')

    this.dateType = this.$store.getters.dateType
    this.dispYear = this.$store.getters.dispYear
    this.dateType1String = this.dispYear ? 'YYYY年MM月DD日' : 'MM月DD日'
    this.dateType2String = this.dispYear ? 'YY/MM/DD' : 'MM/DD'
  },
  beforeMount: function () {
    console.log('MySetting beforeMount')
  },
  mounted: async function () {
    console.log('MySetting mounted')
  },
  beforeUpdate: function () {
    console.log('MySetting beforeUpdate')
  },
  updated: function () {
    console.log('MySetting updated')
  },
  beforeDestroy: function () {
    console.log('MySetting beforeDestroy')
  },
  destroyed: function () {
    console.log('MySetting destroyed')
  }
})
</script>

<style>
.v-overlay {
  width: 320px;
}
.v-dialog__content {
  width: 320px; /* margin分を引く前の値 */
}
.v-dialog {
  margin: 12px;
}
</style>
