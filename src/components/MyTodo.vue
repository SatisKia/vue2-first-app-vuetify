<template>
  <v-card-text class="ma-0 pa-0 text-body-1">
    <todo-input
      v-on:add="addTodo"
    />
    <v-tabs centered v-model="tab">
      <v-tab style="width:50%">ACTIVE</v-tab>
      <v-tab style="width:50%">LOG</v-tab>
      <v-tab-item class="pb-1">
        <todo-label
          v-for="todo in sortedTodo"
          v-bind:key="todo.id"
          v-bind:todo="todo"
          v-on:done="doneTodo"
          v-on:remove="removeTodo"
        />
      </v-tab-item>
      <v-tab-item class="pb-1">
        <todo-label
          v-for="todo in removedTodo"
          v-bind:key="todo.id"
          v-bind:todo="todo"
        />
      </v-tab-item>
    </v-tabs>
  </v-card-text>
</template>

<script lang="ts">
import Vue from 'vue'
import { Todo } from '@/types/todo'
import TodoInput from '@/components/TodoInput.vue'
import TodoLabel from '@/components/TodoLabel.vue'

export default Vue.extend({
  components: {
    TodoInput,
    TodoLabel
  },
  data () {
    return {
      tab: 0 // ゼロオリジンの値
    }
  },
  watch: {
    tab (newVal) {
      console.log(newVal)
    }
  },
  methods: {
    loadData: async function () {
      console.log('loadData')

      // データの読み込み
      this.$store.dispatch('loadData')
    },
    saveData: function () {
      console.log('saveData')

      // データの書き込み
      this.$store.dispatch('saveData')
    },
    addTodo: function (text: string, color: string) {
      this.$store.dispatch('add', {
        todo: {
          id: (new Date()).getTime().toString(),
          done: false,
          removed: false,
          date: new Date(),
          text: text,
          color: color
        }
      })
      this.saveData()
    },
    removeTodo: function (id: string) {
      this.$store.dispatch('remove', { id: id })
      this.saveData()
    },
    doneTodo: function (id: string) {
      this.$store.dispatch('done', { id: id })
      this.saveData()
    }
  },
  computed: {
    sortedTodo: function (): Todo[] {
      // 算出プロパティではデータを直接変更することができないため、sliceで配列をコピー
      let todoList = this.$store.getters.todoList.slice()
      todoList = todoList.filter((todo: { removed: boolean }) => todo.removed !== true)
      return todoList.sort((a: Todo, b: Todo) => {
        return b.date.getTime() - a.date.getTime()
      })
    },
    removedTodo: function (): Todo[] {
      // 算出プロパティではデータを直接変更することができないため、sliceで配列をコピー
      const todoList = this.$store.getters.todoList.slice()
      return todoList.filter((todo: { removed: boolean }) => todo.removed === true)
    }
  },
  beforeCreate: function () {
    console.log('MyTodo beforeCreate')
  },
  created: function () {
    console.log('MyTodo created')
  },
  beforeMount: function () {
    console.log('MyTodo beforeMount')
  },
  mounted: async function () {
    console.log('MyTodo mounted')

    // データの読み込み
    await this.loadData()
  },
  beforeUpdate: function () {
    console.log('MyTodo beforeUpdate')
  },
  updated: function () {
    console.log('MyTodo updated')
  },
  beforeDestroy: function () {
    console.log('MyTodo beforeDestroy')
  },
  destroyed: function () {
    console.log('MyTodo destroyed')
  }
})
</script>
