<template>
  <input
    type="text"
    class="lead mb-3 mt-1 p-2"
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
</style>
