<template>
  <div class="board">
    <h1>{{ title }}</h1>
    <div class="row">
      <kanban-track
        class="col"
        v-for="track in tracksOrderedWithCards"
        :key="track.id"
        :track="track"
        :cards="track.cards"
        @create-card="createCard"
        @update-card="updateCard">
      </kanban-track>
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
          id: 1,
          title: 'This needs to be done please.',
          order: 1,
          trackId: 1
        },
        {
          id: 2,
          title: 'This one is late.',
          order: 2,
          trackId: 1
        },
        {
          id: 3,
          title: "Let's get this done now!",
          order: 1,
          trackId: 2
        },
        {
          id: 4,
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
    }
  },

  methods: {
    createCard: function (title, trackId) {
      var newCard = {
        title: title,
        trackId: trackId
      };

      this.cards.push(newCard);
    },
    updateCard: function (newTitle, cardId) {
      var card = this.cards.filter( (card) => card.id == cardId )[0];
      
      this.$set(card, 'title', newTitle);
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
