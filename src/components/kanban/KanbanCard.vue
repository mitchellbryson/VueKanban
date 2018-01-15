<template>
  <li class="card mb-3 p-2">
    <input
      type="text"
      class="lead p-2"
      ref="editTitle"
      placeholder="Enter a task..."
      v-show="editing"
      v-model="newTitle"
      @keyup.enter="update"
      @keyup.escape="cancel">
    <p
      class="title lead mb-2 p-2"
      @dblclick="toggleEdit"
      v-show="!editing">
      <button
        class="close text-muted"
        @click="destroy">
        &times;
      </button>
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
      newTitle: this.card.title,
      editing: false
    }
  },

  methods: {
    toggleEdit: function () {
      this.editing = !this.editing;

      if (this.editing) {
        this.$nextTick( () => {
          var input = this.$refs.editTitle;

          input.focus();
          input.select();
        });
      }
    },
    update: function () {
      this.$emit('update', this.editedTitle, this.card.id);

      this.toggleEdit();
    },
    cancel: function () {
      this.editedTitle = this.card.title;

      this.toggleEdit();
    },
    destroy() {
      if ( confirm("Are you sure?") ) {
        this.$emit('destroy', this.card.id);
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .card {
    border-color: transparent;
    background-color: white;
    box-shadow: 0 5px 20px rgba($gray-900, .1);

    &:last-child {
      margin-bottom: 0 !important;
    }

    .title {
      color: $gray-700;
      font-weight: 500;

      &:last-child {
        margin-bottom: 0 !important;
      }
    }

    input {
      color: $gray-700;
      background-color: $gray-200;
      font-weight: 500;
      border: 0;

      @include hover-focus {
        outline: rgba($gray-900, .1) auto 5px;
      }
    }
  }
</style>
