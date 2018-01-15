<template>
  <li class="card list-group-item">
    <p class="lead">
      {{ card.title }}
    </p>
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
  .card {
    margin-bottom: 30px;
  }
</style>
