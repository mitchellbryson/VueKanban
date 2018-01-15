<template>
  <div class="track p-3">
    <header class="d-flex justify-content-between align-items-start">
      <input
        ref="editing"
        class="h4 mb-3"
        placeholder="Enter a title..."
        v-model="newTitle"
        @keyup.enter="update"
        @keyup.escape="cancel"
        v-show="editing">
      <h2
        class="title h4 mb-3"
        @dblclick="edit"
        v-show="!editing">
        {{ track.title }}
      </h2>
      <button
        class="btn btn-add btn-sm btn-dark"
        @click="newCard"
        v-html="newCardText">
      </button>
    </header>
    <ul class="cards list-group">
      <kanban-card-new
        ref="newCard"
        v-show="creatingCard"
        @create="createCard"
        @cancel="cancelCard">
      </kanban-card-new>
      <draggable class="drag-area" v-model="cardsOrdered" :options="{group: 'track'}">
        <kanban-card
          v-for="card in cardsOrdered"
          :key="card.id"
          :card="card"
          @update="updateCard"
          @destroy="destroyCard">
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
      creatingCard: false,
      newCardText: '&plus;',
      editing: false,
      newTitle: this.track.title
    }
  },

  computed: {
    cardsOrdered: {
      get () {
        return this.cards.sort( (a, b) => a.order - b.order );;
      },
      set (cards) {
        this.$emit('update-all-cards', cards, this.track.id)
      }
    }
  },

  methods: {
    edit() {
      this.editing = !this.editing;

      this.$nextTick(function () {
        this.$refs.editing.focus();
        this.$refs.editing.select();
      });
    },
    update() {
      this.$emit('update-track', this.newTitle, this.track.id);

      this.editing = !this.editing;
    },
    cancel() {
      this.newTitle = this.track.title;

      this.editing = !this.editing;
    },

    newCard: function () {
      this.creatingCard = !this.creatingCard;

      if (this.creatingCard) {
        this.newCardText = '&times;';

        this.$nextTick( () => this.$refs.newCard.$el.focus() );
      } else {
        this.newCardText = '&plus;';

        this.$nextTick( () => this.$refs.newCard.$el.blur() );
      }
    },
    createCard: function (title) {
      this.$emit('create-card', title, this.track.id);
    },
    updateCard: function (newTitle, cardId) {
      this.$emit('update-card', newTitle, cardId);
    },
    cancelCard: function () {
      this.newCard();
    },
    destroyCard: function(cardId) {
      this.$emit('destroy-card', cardId);
    }
  }
}
</script>

<style lang="scss" scoped>
  .track {
    @include border-radius($border-radius);

    .title {
      color: rgba($gray-900, .8);
      font-weight: 800;
      border: 1px solid transparent;
    }

    input {
      color: rgba($gray-900, .8);
      font-weight: 800;
      border: 0;

      @include hover-focus {
        outline: rgba($gray-900, .1) auto 5px;
      }
    }

    .btn-add {
      padding: 0 $spacer / 2;
      font-size: $font-size-lg;
      font-weight: 700;
      background-color: rgba($gray-900, .2);
      border-color: transparent;

      @include hover-focus {
        background-color: rgba($gray-900, .6);
        border-color: transparent;
      }
    }

    .drag-area {
      min-height: 52px;
      @include border-radius($border-radius);

      &:empty {
        background-color: rgba($gray-900, .05);
      }
    }
  }
</style>
