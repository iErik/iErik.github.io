<script lang="coffee">
import Vue from 'vue'
import { bus } from './TransitionBus'

Vue.component 'split-layout-transition',
  functional: true
  render: (createElement, context) ->
    createElement 'transition',
      props:
        name: 'l-split'
      on:
        enter: (el, done) ->
          pendingEnter = () =>
            el.addEventListener 'animationend', () => done()

          if bus.$data.hasPendingLeave
            bus.$once 'next', pendingEnter
          else
            pendingEnter();
        enterCancelled: (el, done) ->
          bus.$off 'next'
        leave: (el, done) ->
          bus.$data.hasPendingLeave = true;
          el.addEventListener 'transitionend', () =>
            done()
            bus.$emit 'next'
            bus.$data.hasPendingLeave = false;
    , context.children

</script>

<style lang="sass">

//  Transition Enter Stage
//  ----------------------

@keyframes splitEnter
  from
    transform: scale(0.6)
    opacity: 0

  to
    transform: scale(1)
    opacity: 1

.l-split-enter-active
  animation: splitEnter 700ms

  // NOTE: A little hack to fix bad code, this should be removed soon
  // as this probably isn't the most appropriate way to deal with this
  .left-column

    nav.menu-overlay
      visibility: hidden

    .contents *
      transform: scale(1.0) !important
      opacity: 1 !important

//  Transition Leave Stage
//  ----------------------

.l-split-leave
  transform: scale(1)
  opacity: 1

.l-split-leave-to
  transform: scale(1)
  opacity: 0

.l-split-leave-active
  z-index: 30
  transition: opacity 200ms ease-out

</style>
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
