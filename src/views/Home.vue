<template lang="pug">
  div
    transition(name="fade")
      Loader(v-if="isLoading")

    div.home
      h1 Daily tasks

      div(
        v-for="(task, index) in tasks"
        :key="index"
        class="home__task task"
        :style="`background: ${task.color}`"
        @dblclick="goToTaskPage(task)"
      )
        div.task__title {{ task.title || 'no title' }}

        div.task__description {{ task.description }}

      router-link(
        class="home__add-new-task add-new-task"
        to="/tasks/new"
      )
        div.add-new-task__text Добавить

        div.add-new-task__icon +
</template>

<script>
import statuses from '@/utils/statuses';

export default {
  name: 'Home',

  data() {
    return {
      tasks: [],
      isLoading: true,
    };
  },

  components: {
    Loader: () => import('@/components/Loader.vue'),
  },

  async mounted() {
    await fetch('http://localhost:3000/tasks')
      .then((response) => response.json())
      .then((data) => {
        this.tasks = data.map((task) => {
          const { color } = statuses.find((el) => el.id === task.status);

          return {
            ...task,
            color,
          };
        });

        this.isLoading = false;
      });
  },

  methods: {
    goToTaskPage(el) {
      this.$router.push({ path: `/tasks/${el.id}` });
    },
  },
};
</script>

<style lang="sass">
.home
  position: relative

  .task
    background: #fff
    box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%)
    margin-bottom: 20px
    padding: 20px
    cursor: pointer
    border-radius: 10px

    &__title
      // color: rgb(144 165 184)
      color: #fff
      font-weight: bold
      font-size: 24px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      margin-bottom: 5px

    &__description
      // color: rgb(186 205 222)
      color: #fff
      opacity: 0.7
      overflow: hidden
      text-overflow: ellipsis
      display: -webkit-box
      -webkit-line-clamp: 2
      -webkit-box-orient: vertical

  .add-new-task
    position: absolute
    right: 0
    top: 0
    display: flex
    text-decoration: none
    align-items: center

    &__icon
      color: rgb(36 151 253)
      font-size: 46px
      font-weight: bold
      line-height: 36px
      margin-left: 10px

    &__text
      color: #bacdde
      font-size: 12px
</style>
