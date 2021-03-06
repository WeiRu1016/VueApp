<template>
<span>
  <transition :name="transition" @after-enter="show()" @after-leave="hide()">
    <div class="mu-popup" ref="popup" v-show="open" :class="popupCss" :style="{'z-index': zIndex}">
      <slot></slot>
    </div>
  </transition>
</span>
</template>

<script>
import Popup from '../src'
import { convertClass } from '../src/utils'
export default {
  name: 'mu-popup',
  mixins: [Popup],
  props: {
    popupClass: {
      type: [String, Object, Array]
    },
    popupTransition: {
      type: String,
      default: ''
    },
    position: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      transition: this.popupTransition
    }
  },
  created () {
    if (!this.popupTransition) {
      this.transition = `popup-slide-${this.position}`
    }
  },
  computed: {
    popupCss () {
      const {position, popupClass} = this
      let classNames = []
      if (position) classNames.push('mu-popup-' + position)
      return classNames.concat(convertClass(popupClass))
    }
  },
  methods: {
    overlayClick () {
      this.$emit('close', 'overlay')
    },
    escPress () {
      this.$emit('close', 'esc')
    },
    show () {
      this.$emit('show')
    },
    hide () {
      this.$emit('hide')
    }
  },
  watch: {
    popupTransition (val, oldVal) {
      if (val === oldVal) return
      this.transition = val
    }
  }
}
</script>

<style lang="scss">
.mu-popup {
  position: fixed;
  background-color: '#ccc';
  top: 50%;
  left: 50%;
  transform: translate3d(-50%, -50%, 0);
  backface-visibility: hidden;
}
.mu-popup-top {
  top: 0;
  right: auto;
  bottom: auto;
  left: 50%;
  transform: translate3d(-50%, 0, 0);
}
.mu-popup-right {
  top: 50%;
  right: 0;
  bottom: auto;
  left: auto;
  transform: translate3d(0, -50%, 0);
}
.mu-popup-bottom {
  top: auto;
  right: auto;
  bottom: 0;
  left: 50%;
  transform: translate3d(-50%, 0, 0);
}
.mu-popup-left {
  top: 50%;
  right: auto;
  bottom: auto;
  left: 0;
  transform: translate3d(0, -50%, 0);
}
.popup-slide-top-enter-active,
.popup-slide-top-leave-active,
.popup-slide-right-enter-active,
.popup-slide-right-leave-active,
.popup-slide-bottom-enter-active,
.popup-slide-bottom-leave-active,
.popup-slide-left-enter-active,
.popup-slide-left-leave-active {
  transition: transform .3s ease;
}
.popup-slide-top-enter,
.popup-slide-top-leave-active {
  transform: translate3d(-50%, -100%, 0);
}
.popup-slide-right-enter,
.popup-slide-right-leave-active {
  transform: translate3d(100%, -50%, 0);
}
.popup-slide-bottom-enter,
.popup-slide-bottom-leave-active {
  transform: translate3d(-50%, 100%, 0);
}
.popup-slide-left-enter,
.popup-slide-left-leave-active {
  transform: translate3d(-100%, -50%, 0);
}
.popup-fade-enter-active,
.popup-fade-leave-active {
  transition: opacity .3s;
}
.popup-fade-enter,
.popup-fade-leave-active {
  opacity: 0;
}
</style>