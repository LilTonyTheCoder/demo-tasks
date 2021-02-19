<template lang="pug">
  div.task-page
    h1 Task: {{ currentTaskData.title }}

    div.task-page__info {{ currentTaskData.description }}

    div.task-page__date {{ currentTaskData.created_date }}

    select(
      name="select"
      :value="currentTaskData.status"
      class="task-page__status"
      @input="selectHandler"
    )
      option(
        v-for="(item, index) in comStatuses"
        :key="index"
        :value="item.id"
      ) {{ item.title }}</option>

    button(
      class="task-page__remove"
      @click="removeTask"
    ) Удалить
</template>

<script>
import statuses from '@/utils/statuses';

export default {
  name: 'ExistingTask',

  props: {
    currentTaskData: {
      type: Object,
      default: () => {},
    },
  },

  data() {
    return {};
  },

  computed: {
    comStatuses() {
      return statuses;
    },
  },

  methods: {
    async removeTask() {
      this.$emit('changeLoadingState', true);

      await fetch(`http://localhost:3000/tasks/${this.currentTaskData.id}`, {
        method: 'DELETE',
      })
        .then((response) => {
          this.$emit('changeLoadingState', false);

          if (!response.ok) {
            this.$router.push({ path: '/error/' });
            throw response;
          }

          this.$router.push({ path: '/' });
        });
    },

    async selectHandler(event) {
      const value = Number.parseInt(event.target.value, 10);

      this.$emit('changeStatus', value);
      this.$emit('changeLoadingState', true);

      await fetch(`http://localhost:3000/tasks/${this.currentTaskData.id}`, {
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(this.currentTaskData),
      })
        .then((response) => {
          if (!response.ok) {
            this.$emit('changeLoadingState', false);
            this.$router.push({ path: '/error/' });
            throw response;
          }

          return response.json();
        })
        .then(() => {
          this.$emit('changeLoadingState', false);
        });
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

  &__status, &__remove
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

  &__remove
    background: #ffa1a1
    color: #9d0000
    font-weight: bold

</style>
