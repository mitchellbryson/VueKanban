<template>
  <div class="track">
    <header class="d-flex justify-content-between">
      <h2>{{ track.title }}</h2>
      <button class="btn btn-secondary" @click="showNewCard">
        {{ newCardText }}
      </button>
    </header>
    <ul class="cards list-group">
      <kanban-card-new
        ref="newCard"
        v-show="newCardShown"
        @create="createCard"
        @cancel="cancelCard">
      </kanban-card-new>

      <draggable v-model="cardsOrdered" :options="{group: 'track'}">
        <kanban-card
          v-for="card in cardsOrdered"
          :key="card.id"
          :card="card"
          @update="updateCard">
        </kanban-card>
      </draggable>
    </ul>
  </div>
</template>

<script>
import KanbanCard from './KanbanCard';
import KanbanCardNew from './KanbanCardNew';
import draggable from 'vuedraggable'

export default {
  name: 'KanbanBoard',

  components: {
    KanbanCard,
    KanbanCardNew,
    draggable
  },

  props: ['track', 'cards'],

  data () {
    return {
      newCardShown: false,
      newCardText: 'Add'
    }
  },

  computed: {
    cardsOrdered: {
      get () {
        return this.cards;
      },
      set (cards) {
        this.$emit('update-all-cards', cards, this.track.id)
      }
    }
  },

  methods: {
    showNewCard: function () {
      this.newCardShown = !this.newCardShown;

      if (this.newCardShown) {
        this.newCardText = 'Cancel';

        this.$nextTick( () => (
          this.$refs.newCard.$el.focus()
        ));
      } else {
        this.newCardText = 'Add';
      }
    },

    createCard: function (title) {
      this.$emit('create-card', title, this.track.id);
    },
    updateCard: function (newTitle, cardId) {
      this.$emit('update-card', newTitle, cardId);
    },
    cancelCard: function () {
      this.showNewCard();
    }
  }
}
</script>

<style lang="scss" scoped>
  .track {
    background-color: $gray-300;
  }
</style>
