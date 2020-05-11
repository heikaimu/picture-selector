<template>
  <div
    class="card-wrapper"
    :class="{active: relChecked}"
    @click="handleClick"
  >
    <img
      :src="src"
      alt
    >
    <i
      v-if="closeable"
      class="close-btn"
      @click="handleClose"
    />
  </div>
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      default: ''
    },
    clickable: {
      type: Boolean,
      default: false
    },
    checked: {
      type: Boolean,
      default: false
    },
    closeable: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      relChecked: false
    };
  },
  watch: {
    checked: {
      handler: function(val) {
        if (this.relChecked !== val) this.relChecked = val;
      },
      immediate: true
    },
    relChecked(val) {
      this.$emit('update:checked', val);
    }
  },
  methods: {
    handleClick() {
      if (this.clickable) {
        this.relChecked = !this.relChecked;
        this.$nextTick(() => {
          this.$emit('handleClick', this.relChecked);
        });
      }
    },
    handleClose() {
      this.$emit('handleClose');
    }
  }
};
</script>

<style lang="scss" scoped>
.card-wrapper {
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  border-radius: 4px;
  overflow: hidden;
  position: relative;
  border: 2px solid transparent;
  transition: 0.3s;
  &.active {
    border: 2px solid red;
  }
  img {
    width: 100%;
    height: 100%;
  }
  .close-btn {
    position: absolute;
    right: 0;
    top: 0;
    padding: 15px;
    background-color: red;
    transition: .3s;
    transform: translate3d(25px, -25px, 0) rotate(45deg);
    opacity: 0.6;
    cursor: pointer;
    &::after {
      content: "";
      display: block;
      width: 20px;
      height: 20px;
      background-size: cover;
      background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAABX0lEQVRYR+2VrU7FQBCFzwSFIyFB4VD8KBwKhUMhroMn4B24vANPAA6BwqFQOBQ/CociIcGhyCFLzibN5rYzu1dcRGsq2tnv69nZqWHBly2Yj1FgTOD/J0DyFMCqmZ3XnBiSZwA+zexiqG4wAZLHAC61wDQqIfhUdSdmdtUn4QkcAbgGsBSVKOA/ACZmdtMkkIpIHkpi2ZMo4N+C3zZvQS4keSCJlT6JAv4l+J3XN+FTQHJfEmulRAH/EPzeg6fnYQFtx54k1rNEcX8X/CECrxaQxK4kNgrIm+CPUXiTgCR2JLEp2KvgTzXweQTSkMnnPDPDc6IrWdUD+vou/FmLbXtHtHkOdAuLbk/wiZ6nYdUkEU5gFtzMXpTKlnqiWiIkMATvDKsmCVcgAp9HwvsZlQ2Xfix/sfddJKuS8ATyYonnwmck4dZFtiBJwPvyMhEl4da5ArWTrfb9UWBMYEzgF6R/oiEZY/8cAAAAAElFTkSuQmCC");
      transform: translate3d(0px, 16px, 0) rotate(45deg);
    }
    &:hover {
      opacity: 1;
    }
  }
}
</style>
