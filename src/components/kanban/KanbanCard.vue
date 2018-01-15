<template>
  <li class="card list-group-item">
    <p
      class="lead"
      @dblclick="showEditCard"
      v-show="!editCardShown">
      {{ card.title }}
    </p>
    <input
      type="text"
      ref="editCard"
      placeholder="Enter a task..."
      v-show="editCardShown"
      v-model="editedTitle"
      @keyup.enter="update"
      @keyup.escape="cancel">
  </li>
</template>

<script>
export default {
  name: 'KanbanCard',

  props: ['card'],

  data () {
    return {
      editedTitle: this.card.title,
      editCardShown: false
    }
  },

  methods: {
    showEditCard: function () {
      this.editCardShown = !this.editCardShown;

      if (this.editCardShown) {
        this.$nextTick( () => (
          this.$refs.editCard.focus(),
          this.$refs.editCard.select()
        ));
      }
    },
    update: function () {
      this.$emit('update', this.editedTitle, this.card.id);

      this.showEditCard();

      this.editedTitle = this.card.title;
    },
    cancel: function () {
      this.showEditCard();

      this.editedTitle = this.card.title;
    }
  }
}
</script>

<style lang="scss" scoped>
  .card {
    margin-bottom: 30px;
  }
</style>
