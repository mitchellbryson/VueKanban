<template>
  <li class="card">
    {{ card }}
  </li>
</template>

<script>
export default {
  name: 'KanbanCard',

  props: ['card'],

  data () {
    return {

    }
  }
}
</script>

<style lang="scss" scoped>
</style>
