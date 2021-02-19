<template lang="pug">
  div
    h1 Cannot find this page
    br
    div.symbol ¯\_(ツ)_/¯
    br
    br

    button.go-back(@click="goBack") Go Back
</template>

<script>
export default {
  name: 'Error',

  methods: {
    goBack() {
      this.$router.go(-1);
    },
  },
};
</script>

<style lang="sass">
.symbol
  font-size: 66px

.go-back
    height: 40px
    border: none
    background: #fff
    border-radius: 10px
    box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%)
    margin-bottom: 16px
    padding: 0 10px
    color: #90a5b8
    font-size: 16px
    width: 200px
    cursor: pointer
</style>
