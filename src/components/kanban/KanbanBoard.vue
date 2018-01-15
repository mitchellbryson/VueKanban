<template>
  <div class="board-wrapper">
    <header>
      <h1 class="title h3 m-0 text-center">{{ title }}</h1>
    </header>

    <div class="board">
      <div
        class="alert alert-danger text-center"
        v-show="errorMessage">
        {{ errorMessage }}
      </div>
      <draggable class="drag-area row" v-model="tracksOrderedWithCards" :options="{group: 'board'}">
        <kanban-track
          class="col"
          v-for="track in tracksOrderedWithCards"
          :key="track.id"
          :track="track"
          :cards="track.cards"
          @update-track="updateTrack"
          @create-card="createCard"
          @update-card="updateCard"
          @update-all-cards="updateAllCards"
          @destroy-card="destroyCard">
        </kanban-track>
      </draggable>
    </div>
  </div>
</template>

<script>
import KanbanTrack from './KanbanTrack';
import draggable from 'vuedraggable'
import axios from 'axios';

export default {
  name: 'KanbanBoard',

  components: {
    KanbanTrack,
    draggable
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
    this.loadTracks().then( () => this.loadCards() );
  },

  computed: {
    tracksOrdered: function () {
      if (this.tracks.length > 0) {
        return this.tracks.sort( (a, b) => a.order - b.order );
      }
    },
    tracksOrderedWithCards: {
      get () {
        if (this.cards.length > 0) {
          this.tracksOrdered.forEach( (track) =>
            track.cards = this.cards.filter( (card) => card.trackId === track.id )
          );

          return this.tracksOrdered;
        }
      },
      set(tracks) {
        var self = this;

        tracks.forEach(function(track, index) {
          var track = self.tracks.filter((t) => t.id === track.id)[0];

          self.$set(track, 'order', index + 1);
        });

        this.postTracks();
      }
    }
  },

  methods: {
    loadTracks: function () {
      var self = this;

      return axios.get('/static/mock_api/tracks.json')
        .then(function (response) {
          self.tracks = response.data;
        })
        .catch(function (error) {
          self.errorMessage = 'There was a problem with the API!';
        });
    },
    loadCards: function () {
      var self = this;

      return axios.get('/static/mock_api/cards.json')
        .then(function (response) {
          self.cards = response.data;
        })
        .catch(function (error) {
          self.errorMessage = 'There was a problem with the API!';
        });
    },
    postTracks: function () {
      var self = this;

      return axios.post('/static/mock_api/tracks.json', self.tracks)
        .then(function (response) {
          self.tracks = response.data;
        })
        .catch(function (error) {
          // self.errorMessage = "Sorry, there was a problem saving your changes."
        });
    },
    postCards: function () {
      var self = this;

      return axios.post('/static/mock_api/cards.json', self.tracks)
        .then(function (response) {
          self.cards = response.data;
        })
        .catch(function (error) {
          // self.errorMessage = "Sorry, there was a problem saving your changes."
        });
    },

    updateTrack: function (title, trackId) {
      var track = this.tracks.filter((track) => track.id === trackId)[0];

      this.$set(track, 'title', title);

      this.postTracks();
    },

    nextCardId: function () {
      if (this.cards.length > 0) {
        var cardsSortedById = this.cards.sort((a, b) => a.id - b.id);
        var lastCard = cardsSortedById[this.cards.length - 1];
      }
      return (lastCard || {id: 0}).id + 1;
    },
    nextCardOrder: function (trackId) {
      var track = this.tracks.filter((track) => trackId === track.id)[0];

      if (track.cards.length > 0) {
        var cardsSortedByOrder = track.cards.sort((a, b) => a.order - b.order);
        var lastCard = cardsSortedByOrder[track.cards.length - 1];
      }
      return (lastCard || {order: 0}).order+1;
    },

    createCard: function (title, trackId) {
      var newCard = {
        id: this.nextCardId(),
        order: this.nextCardOrder(trackId),
        title: title,
        trackId: trackId
      };

      this.cards.push(newCard);

      this.postCards();
    },
    updateCard: function (newTitle, cardId) {
      var card = this.cards.filter( (card) => card.id == cardId )[0];

      this.$set(card, 'title', newTitle);

      this.postCards();
    },
    updateAllCards: function (cards, trackId) {
      cards.forEach( (card, index) => card.order = index );
      cards.forEach( (card) => card.trackId = trackId );

      var track = this.tracks.filter( (track) => track.id == trackId )[0];

      track.cards = cards;

      this.postCards();
    },
    destroyCard: function (cardId) {
      this.cards = this.cards.filter((card) => card.id !== cardId);

      this.postCards();
    }
  }
}
</script>

<style lang="scss" scoped>
  header {
    position: relative;
    z-index: 6;
    padding: 5vh;
    background-color: white;
    box-shadow: 0 5px 20px rgba($gray-900, .1);
  }
  .board {
    position: relative;
    z-index: 5;
    min-height: 100vh;
    padding: 5vh;
    background-color: rgba($yellow, .8);
  }
  .title {
    color: rgba($gray-900, .8);
    font-weight: 800;
  }

  .alert {
    border: 0;
  }
</style>
