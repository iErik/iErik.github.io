<script lang="coffee">
import HamButton from 'components/HamButton'

export default RootLayout =
  name: 'root-layout'
  data: ->
    isNavActive: false
    layoutColors: 'colors-accent'
    layoutBackground: 'bg-standard'
  created: ->
    # For some reason the RootLayout can't listen to custom events
    # that are triggered from components instantiated in the router view.

    # So we'll have to use the $router.app instance (basically the
    # root Vue instance) to emit custom events up to the RootLayout
    # and register the event listeners here.

    @$router.app.$on 'toggleNav', () =>
      @isNavActive = !@isNavActive

    @$router.app.$on 'changeLayoutColors', (pallete) =>
      @layoutColors = "colors-#{pallete}"

    @$router.app.$on 'changeLayoutBackground', (palette) =>
      @layoutBackground = "bg-#{pallete}"
  components:
    HamButton: HamButton

</script>

<template lang="html">
  <div :class="['root-container', layoutBackground]">
    <div :class="['wrapper-nav-trigger', layoutColors]">
      <ham-button v-model="isNavActive" :isActive="isNavActive" />
    </div>
    <div class="wrapper-page">
      <router-view :isNavActive="isNavActive" :layoutColors="layoutColors"/>
    </div>
  </div>
</template>

<style lang="sass">
@import ~styles/helpers/module

.wrapper-nav-trigger
  position: fixed
  z-index: 200

  padding: 30px

  +media-breakpoint-down(medium)
    font-size: 14.7px
    padding: 19.385px

</style>
                                                                                                                                                                                                                                                                                                                         
