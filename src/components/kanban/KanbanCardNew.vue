<template>
  <input
    type="text"
    class="lead mb-3 p-2"
    placeholder="Enter a task..."
    v-model="title"
    @keyup.enter="create"
    @keyup.escape="cancel">
</template>

<script>
export default {
  name: 'KanbanCardNew',

  data () {
    return {
      title: ''
    }
  },

  methods: {
    create: function () {
      this.$emit('create', this.title);

      this.title = '';
    },
    cancel: function () {
      this.$emit('cancel');

      this.title = '';
    }
  }
}
</script>

<style lang="scss" scoped>
  input {
    font-weight: 500 !important;
    background-color: rgba(white, .8);
    border: 0;
    @include border-radius($border-radius);

    @include hover-focus {
      outline: rgba($gray-900, .1) auto 5px;
    }

    &, &::placeholder {
      color: $gray-700;
    }
  }
</style>
