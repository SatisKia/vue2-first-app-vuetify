<template>
  <v-card class="mt-1 pa-0 text-body-1" v-bind:style="{ 'background-color': (todo.done ? '#808080' : todo.color) }">
    <v-card-text class="ma-0 pa-0 text-body-1" style="width:100%">
      <table class="table">
        <tbody>
          <tr>
            <td v-if="!todo.removed" class="td" style="width:5%">
              <v-checkbox class="my-4 pa-0" v-model="todoDone" v-on:change="done" hide-details />
            </td>
            <td v-if="$store.getters.dateType !== 3" class="td" style="width:20%">
              <span>{{ date }}</span>
            </td>
            <td class="td" v-bind:style="{ 'width': (($store.getters.dateType !== 3) ? (!todo.removed ? '65%' : '80%') : (!todo.removed ? '85%' : '100%')) }">
              <span>{{ todo.text }}</span>
            </td>
            <td v-if="!todo.removed" class="td" style="width:10%">
              <v-btn v-if="todo.done" plain style="width:100%; min-width: 100%" v-bind:style="{ 'background-color': (todo.done ? '#808080' : todo.color) }" v-on:click="remove">消</v-btn>
            </td>
          </tr>
        </tbody>
      </table>
    </v-card-text>
  </v-card>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue'
import { Todo } from '@/types/todo'

function keta (value: number): string {
  if (value >= 100) {
    return '' + (value % 100)
  }
  return ((value < 10) ? '0' : '') + value
}

export default Vue.extend({
  data () {
    return {
      todoDone: false
    }
  },
  props: {
    todo: {
      type: Object as PropType<Todo>,
      required: true
    }
  },
  methods: {
    done: function () {
      if (this.$props.todo) {
        this.$emit('done', this.$props.todo.id)
      }
    },
    remove: function () {
      if (this.$props.todo) {
        this.$emit('remove', this.$props.todo.id)
      }
    }
  },
  computed: {
    date: function (): string {
      if (!this.$props.todo) return ''
      const { date } = this.$props.todo
      if (this.$store.getters.dispYear) {
        if (this.$store.getters.dateType === 1) {
          return '' + date.getFullYear() + '年' + (date.getMonth() + 1) + '月' + date.getDate() + '日'
        } else {
          return '' + keta(date.getFullYear()) + '/' + keta(date.getMonth() + 1) + '/' + keta(date.getDate())
        }
      } else {
        if (this.$store.getters.dateType === 1) {
          return '' + (date.getMonth() + 1) + '月' + date.getDate() + '日'
        } else {
          return '' + keta(date.getMonth() + 1) + '/' + keta(date.getDate())
        }
      }
    }
  },
  beforeCreate: function () {
    console.log('TodoLabel beforeCreate')
  },
  created: function () {
    console.log('TodoLabel created')
    this.todoDone = this.$props.todo.done
  },
  beforeMount: function () {
    console.log('TodoLabel beforeMount')
  },
  mounted: async function () {
    console.log('TodoLabel mounted')
  },
  beforeUpdate: function () {
    console.log('TodoLabel beforeUpdate')
  },
  updated: function () {
    console.log('TodoLabel updated')
  },
  beforeDestroy: function () {
    console.log('TodoLabel beforeDestroy')
  },
  destroyed: function () {
    console.log('TodoLabel destroyed')
  }
})
</script>

<style scoped>
.table {
  width: 100%;
  min-height: 48px;
  border: 0;
}
.td {
  text-align: left;
  vertical-align: middle;
  word-break: break-all;
}
</style>
