<template>
  <div class="board">
    <h1>{{ title }}</h1>
    <div class="alert alert-danger" v-show="errorMessage">{{ errorMessage}}</div>
    <div class="row">
      <kanban-track
        class="col"
        v-for="track in tracksOrderedWithCards"
        :key="track.id"
        :track="track"
        :cards="track.cards"
        @create-card="createCard"
        @update-card="updateCard"
        @update-all-cards="updateAllCards">
      </kanban-track>
    </div>
  </div>
</template>

<script>
import KanbanTrack from './KanbanTrack';
import axios from 'axios';

export default {
  name: 'KanbanBoard',

  components: {
    KanbanTrack
  },

  data () {
    return {
      title: "Let's Kanban!",
      errorMessage: '',
      tracks: [],
      cards: []
    }
  },

  created: function () {
    this.loadTracks();
  },

  computed: {
    cardsOrdered: function () {
      if (this.cards.length > 0) {
        return this.cards.sort( (a, b) => a.order - b.order );
      }
    },
    tracksOrdered: function () {
      if (this.tracks.length > 0) {
        return this.tracks.sort( (a, b) => a.order - b.order );
      }
    },
    tracksOrderedWithCards: function () {
      if (this.cards.length > 0) {
        this.tracksOrdered.forEach( (track) =>
          track.cards = this.cardsOrdered.filter( (card) => card.trackId === track.id )
        );

        return this.tracksOrdered;
      }
    }
  },

  methods: {
    loadTracks: function () {
      var self = this;

      axios.get('/static/mock_api/tracks.json')
        .then(function (response) {
          self.tracks = response.data;

          self.loadCards();
        })
        .catch(function (error) {
          self.errorMessage = 'Sorry!';
        });
    },
    loadCards: function () {
      var self = this;

      axios.get('/static/mock_api/cards.json')
        .then(function (response) {
          self.cards = response.data;
        })
        .catch(function (error) {
          self.errorMessage = 'Sorry!';
        });
    },

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
    },
    updateAllCards: function (cards, trackId) {
      cards.forEach( (card, index) => card.order = index );
      cards.forEach( (card) => card.trackId = trackId );

      var track = this.tracks.filter( (track) => track.id == trackId )[0];

      track.cards = cards;
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
