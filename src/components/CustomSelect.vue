<template>
  <div class="custom-select"  v-click-outside="closeOptions">
    <div
        class="custom-select__selection selection"
        @click="focusInputAndOpenOptions"
        :class="isFocus ? 'selection--focus' : ''"
    >
      <div  class="selection__list">
        <div>
          <ul>
            <li v-for="item in selected" :key="item.value">
              <chip :label="item.label" @cancel="toggleSelect(item)"></chip>
            </li>
            <li>
              <input
                  ref="select_input"
                  type="text"
                  v-model="searchString"
                  @keydown.delete="deleteLastItem"
                  @input="filter($event)"
                  @focus="isFocus = true"
                  @blur="isFocus=false"
                  onkeydown="this.style.width = ((this.value.length + 1) * 9) + 'px';">
            </li>
          </ul>
        </div>
      </div>
    </div>

    <transition name="fade">
      <div class="custom-select__options options" v-if="visibleOptions">
        <ul v-if="currentOptions.length !== 0">
          <li v-for="item in currentOptions" :key="item.value" @click="toggleSelect(item)">
            <input type="checkbox" :checked="checkChecked(item)">
            {{item.label}}
          </li>
        </ul>

        <div v-else>No data</div>
      </div>
    </transition>
  </div>
</template>

<script>
import { find, findIndex } from 'lodash'
import Chip from './Chip'
export default {
  components: {
    Chip
  },
  props: {
    value: {
      type: Array,
      default: () => []
    },
    options: {
      type: Array,
      required: true,
      validate: (options) => {
        const props = ['value', 'label']
        return Array.isArray(options)
            && options.every(option =>
                typeof option === 'object' &&
                !Array.isArray(option) &&
                props.every(prop => prop in option)
            )
      }
    }
  },
  data() {
    return {
      searchString: '',
      currentOptions: [],
      visibleOptions: false,
      isFocus: false
    }
  },
  computed: {
    selected: {
      get() {
        return this.value.map(val =>
            find(this.options, { value: val }) ||
            {value: val, label: val})
      },
      set(val) {
        this.$emit('input', val.map(item => item.value))
      }
    }
  },
  watch: {
    options: {
      handler() {
        this.currentOptions = this.options
      },
      immediate: true
    }
  },
  methods: {
    toggleSelect(item) {
      const index = findIndex(this.selected, {value: item.value})
      let selected
      if(index !== -1) {
        selected = this.selected.filter(data => data.value !== item.value).map(data => data.value)
      } else {
        selected = [...this.selected]
        selected.push(item)
        selected = selected.map(data => data.value)
      }
      this.$emit('input', selected)
    },
    checkChecked(data) {
      return findIndex(this.selected, {value: data.value}) !== -1
    },
    filter(event) {
      const value = event.target.value.toLowerCase()
      this.currentOptions = this.options.filter(item => item.label.toLowerCase().match(value))
    },
    focusInputAndOpenOptions() {
      this.$refs.select_input.focus()
      this.visibleOptions = true
    },
    deleteLastItem() {
      if(this.searchString.length === 0) {
        const selected = [...this.selected]
        selected.splice(selected.length - 1, 1)
        this.$emit('input', selected.map(item => item.value))
      }
    },
    closeOptions() {
      this.visibleOptions = false
    }
  }
}
</script>

<style scoped lang="scss">
.custom-select {
  width: 300px;
  position: relative;

  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  &__selection {
    background: #F6F6F8;
    border: 2px solid transparent;
    border-radius: 4px;
    min-height: 32px;
    padding: 10px 14px;
    transition: all .3s cubic-bezier(.645,.045,.355,1);

    &:hover {
      background: #ECECEE;
      border: 2px solid #E3E3E8;
    }

    ul {
      display: flex;
      flex: 1 1;
      flex-wrap: wrap;

      li {
        margin-top: 3px;
        line-height: 22px;
        height: 24px;
        position: relative;
        overflow: hidden;
        transition: padding .3s cubic-bezier(.645,.045,.355,1);

        &:last-child {
          height: 24px;
          margin-top: 3px;
          line-height: 22px;
          width: auto;
          max-width: 100%;
          padding: 0;


          input {
            width: auto;
            padding: 1px;
            height: 100%;
            font-size: 100%;
            line-height: 1;
            background: transparent;
            border-width: 0;
            border-radius: 4px;
            outline: 0;
            color: #07152E;
          }
        }
      }
    }
  }

  .options {
    background: #FFFFFF;
    box-shadow: 0 15px 43px rgba(140, 158, 170, 0.15);
    border-radius: 10px;
    padding: 20px 0;
    position: absolute;
    width: 100%;

    ul {
      li {
        text-align: left;
        padding: 4px 14px;
        cursor: pointer;
        font-weight: 500;
        font-size: 16px;
        line-height: 19px;
        color: #07152E;

        &:hover {
          text-decoration-line: underline;
          color: #555D69;
        }
      }
    }
  }

  .selection {
    &--focus {
      background: #FFFFFF;
      border: 2px solid #0060F0;
    }

    &__list {
      &:before {
        display: table;
        content: "";
      }
    }
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
