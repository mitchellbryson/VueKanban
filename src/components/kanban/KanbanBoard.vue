<template>
  <div class="board">
    <h1>{{ title }}</h1>
    <div class="row">
      <kanban-track class="col" v-for="track in tracksOrderedWithCards" :key="track.id" :track="track"></kanban-track>
    </div>
  </div>
</template>

<script>
import KanbanTrack from './KanbanTrack';

export default {
  name: 'KanbanBoard',

  components: {
    KanbanTrack
  },
  
  data () {
    return {
      title: "Let's Kanban!",
      tracks: [
        {
          id: 1,
          title: 'To Do',
          order: 1
        },
        {
          id: 2,
          title: 'Doing',
          order: 2
        },
        {
          id: 3,
          title: 'Done',
          order: 3
        }
      ],
      cards: [
        {
          title: 'This needs to be done please.',
          order: 1,
          trackId: 1
        },
        {
          title: 'This one is late.',
          order: 2,
          trackId: 1
        },
        {
          title: "Let's get this done now!",
          order: 1,
          trackId: 2
        },
        {
          title: "Already done this one.",
          order: 1,
          trackId: 3
        }
      ]
    }
  },

  computed: {
    cardsOrdered: function () {
      return this.cards.sort( (a, b) => a.order - b.order );
    },
    tracksOrdered: function () {
      return this.tracks.sort( (a, b) => a.order - b.order );
    },
    tracksOrderedWithCards: function () {
      this.tracksOrdered.forEach( (track) =>
        track.cards = this.cardsOrdered.filter( (card) => card.trackId === track.id )
      );

      return this.tracksOrdered;
    },
  }
}
</script>

<style lang="scss" scoped>
</style>
