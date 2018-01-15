<template>
  <div class="track">
    <h2>{{ track.title }}</h2>
    <ul class="cards list-group">
      <kanban-card v-for="card in track.cards" :key="card.id" :card="card"></kanban-card>
    </ul>
  </div>
</template>

<script>
import KanbanCard from './KanbanCard';

export default {
  name: 'KanbanBoard',

  components: {
    KanbanCard
  },

  props: ['track'],

  data () {
    return {

    }
  }
}
</script>

<style lang="scss" scoped>
  .track {
    background-color: $gray-300;
  }
</style>
