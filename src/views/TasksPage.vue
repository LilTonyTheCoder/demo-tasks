<template lang="pug">
  div.task-page
    transition.fade
      Loader(v-if="isLoading")

    NewTask(
      v-if="creatingNewTask"
      @createTask="handleCreateTask"
      @changeLoadingState="changeLoadingState"
    )
    ExistingTask(
      v-else
      :currentTaskData="currentTaskData"
      @changeStatus="changeTaskStatus"
      @changeLoadingState="changeLoadingState"
    )
</template>

<script>
// const sleep = (m) => new Promise((r) => setTimeout(r, m));

export default {
  name: 'TasksPage',

  data() {
    return {
      isLoading: true,

      currentTaskData: {},

      creatingNewTask: false,
    };
  },

  components: {
    NewTask: () => import('@/components/pages/NewTask.vue'),
    ExistingTask: () => import('@/components/pages/ExistingTask.vue'),
    Loader: () => import('@/components/Loader.vue'),
  },

  async mounted() {
    const { id } = this.$route.params;

    if (id === 'new') {
      this.creatingNewTask = true;
      this.isLoading = false;
      return;
    }

    // await sleep(10000);

    await fetch(`http://localhost:3000/tasks/${id}`)
      .then((response) => {
        if (!response.ok) {
          this.$router.replace({ path: '/error/' });
          throw response;
        }
        return response.json();
      })
      .then((data) => {
        this.currentTaskData = data;

        this.isLoading = false;
      });
  },

  methods: {
    changeTaskStatus(newStatus) {
      this.currentTaskData.status = newStatus;
    },

    handleCreateTask(newTaskObj) {
      this.creatingNewTask = false;
      this.currentTaskData = newTaskObj;
    },

    changeLoadingState(flag) {
      this.isLoading = flag;
    },
  },
};
</script>

<style lang="sass">
.task-page
  display: flex
  flex-direction: column

  &__info
    font-size: 16px
    margin-bottom: 16px
    color: rgb(144 165 184)

  &__date
    position: absolute
    right: 20px
    bottom: 20px
    color: rgb(144 165 184)

  &__status, &__remove, &__create, input
    height: 40px
    border: none
    background: #fff
    border-radius: 10px
    box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%)
    margin-bottom: 16px
    padding: 0 10px
    // color: #eee
    color: rgb(144 165 184)
    font-size: 16px
    width: 200px
    cursor: pointer
    outline: none

  &__remove, &__create
    background: #ffa1a1
    color: #9d0000
    font-weight: bold

  &__create
    background: #acffb7
    color: #249423

</style>
