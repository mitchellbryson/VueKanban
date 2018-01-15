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

      <kanban-card v-for="card in cards" :key="card.id" :card="card"></kanban-card>
    </ul>
  </div>
</template>

<script>
import KanbanCard from './KanbanCard';
import KanbanCardNew from './KanbanCardNew';

export default {
  name: 'KanbanBoard',

  components: {
    KanbanCard,
    KanbanCardNew
  },

  props: ['track', 'cards'],

  data () {
    return {
      newCardShown: false,
      newCardText: 'Add'
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
