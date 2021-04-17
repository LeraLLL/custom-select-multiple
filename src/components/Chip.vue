<template>
  <transition name="fade">
    <div class="chip" v-if="show">
      <p>{{label}}</p><span @click="cancel">✕</span>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      show: false
    }
  },
  mounted() {
    this.show = true
  },
  methods: {
    cancel() {
      this.show = false
      setTimeout(() => {
        this.$emit('cancel')
      }, 300)
    }
  }
}
</script>

<style scoped lang="scss">
.chip {
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 99%;
  margin-right: 4px;
  padding: 0 20px 0 10px;
  height: 22px;
  color: #1890ff;
  background: #e6f7ff;
  border: 1px solid #91d5ff;
  border-radius: 2px;
  cursor: default;
  overflow: hidden;

  p {
    margin: 0;
    margin-right: 10px;
  }

  span {
    cursor: pointer;
  }
}

// animation
.fade-enter-active, .fade-leave-active {
  transition: opacity .3s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
