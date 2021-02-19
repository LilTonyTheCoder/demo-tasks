<template lang="pug">
  div#app
    router-view
</template>

<style lang="sass">
@import '@/assets/main.sass'

#app
  background: rgb(240 244 247)
  width: 500px
  border-radius: 30px
  min-height: 500px
  padding: 60px 40px
  box-sizing: border-box
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%)
  position: relative
  margin-top: 50px
</style>
