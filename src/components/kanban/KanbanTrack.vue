<template>
  <div class="track">
    {{ track }}
    <ul>
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
</style>
