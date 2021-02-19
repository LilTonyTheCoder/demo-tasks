<template lang="pug">
  div
    h1 Creating new task

    div.task-page__info Title:
      br
      input(
        v-model="task.title"
        type="text"
        @keyup.enter="createTask"
      )

    div.task-page__info Description:
      br
      input(
        v-model="task.description"
        type="text"
        @keyup.enter="createTask"
      )

    button.task-page__create(
      @click="createTask"
    ) Create Task
</template>

<script>
export default {
  name: 'NewTask',

  data() {
    return {
      task: {
        title: '',
        description: '',
        status: 1,
        created_date: null,
      },
    };
  },

  methods: {
    async createTask() {
      const date = new Date();
      this.task.created_date = date.toJSON();

      this.$emit('changeLoadingState', true);

      await fetch('http://localhost:3000/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(this.task),
      })
        .then((response) => {
          if (!response.ok) {
            this.$emit('changeLoadingState', false);

            this.$router.push({ path: '/error/' });
            throw response;
          }

          return response.json();
        })
        .then((data) => {
          this.$emit('changeLoadingState', false);

          this.$emit('createTask', data);
          this.$router.replace({ path: `/tasks/${data.id}` });
        });
    },
  },
};
</script>

<style lang="sass">

</style>
