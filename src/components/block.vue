<template>
  <div class="block">
    <transition v-on:enter="enter" v-bind:css="false">
      <span v-show="figure > -1">{{ fig }}</span>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'block',
  props: {
    figure: {
      type: Number,
      default: -1
    }
  },

  computed: {
    fig() {
      return this.figure === 0 ? 'O' : 'X';
    }
  },

  data() {
    return {
      selected: false
    };
  },

  methods: {
    enter(el, done) {
      window.TweenMax.from(el, 1, {
        autoAlpha: 0,
        scale: 0,
        ease: window.Elastic.easeOut.config(1.25, 0.5),
        onComplete: done
      });
    }
  }
};
</script>
