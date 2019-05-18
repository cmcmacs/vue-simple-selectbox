<template>
    <div class="select-container" :class="{ 'select-active': selectStatus }" v-clickOutside="closeMenu">
        <select v-model="currentValue">
            <option v-for="option in options" :value="option.value" :key="option.value">{{option.text}}</option>
        </select>
        <div class="select-box">
            <span class="select-box--single" v-on:click="selectStatus = !selectStatus"
                  :style="{ minWidth: minWidth + 'px' }">
                <span class="select-box--value">{{ currentValue != null ? currentText : placeholder }}</span>
                <span class="select-box--chevron"></span>
            </span>
            <ul class="select-box--dropdown">
                <li v-for="option in options" class="select-box--element" v-on:click="updateOption(option.value)"
                    :class="[option.value == currentValue ? 'select-box--active' : '' ]" :value="option.value"
                    :key="option.value">{{option.text}}</li>
            </ul>
        </div>
    </div>
</template>
<script>
  export default {
    data: function () {
      return {
        currentValue: this.selected,
        selectStatus: false
      }
    },
    computed: {
      currentText: function () {
        let text = this.options.find(obj => {
          return obj.value == this.currentValue;
        })
        if (text != null) {
          return text.text;
        } else {
          return false;
        }
      }
    },
    props: {
      placeholder: {
        type: String,
        required: false,
        default: 'Select item...'
      },
      selected: {
        type: [String, Number],
        required: false
      },
      minWidth: {
        type: Number,
        required: false,
        default: 160
      },
      options: {
        type: Array,
        required: true,
        validator: (value) => {
          let error = false;
          if (value.length > 0) {
            value.forEach((v, i, a) => {
              if (v.text == null || v.value == null) {
                error = true;
              }
            });
            if (error) {
              return false;
            }
          } else {
            return false;
          }
          return true;
        }
      }
    },
    //Vue click outside directive by xiaoyu2er
    //https://stackoverflow.com/users/3373909/xiaoyu2er
    directives: {
      clickOutside: {
        bind: function (el, binding, vNode) {
          // Provided expression must evaluate to a function.
          if (typeof binding.value !== 'function') {
            const compName = vNode.context.name;
            let warn = `[Vue-click-outside:] provided expression '${binding.expression}' is not a function, but has to be`;
            if (compName) {
              warn += `Found in component '${compName}'`;
            }
            console.warn(warn);
          }
          // Define Handler and cache it on the element
          const bubble = binding.modifiers.bubble;
          const handler = (e) => {
            if (bubble || (!el.contains(e.target) && el !== e.target)) {
              binding.value(e);
            }
          };
          el.__vueClickOutside__ = handler;

          // add Event Listeners
          document.addEventListener('click', handler);
        },

        unbind: function (el, binding) {
          // Remove Event Listeners
          document.removeEventListener('click', el.__vueClickOutside__);
          el.__vueClickOutside__ = null;

        }
      }
    },
    methods: {
      updateOption(option) {
        this.selectStatus = false;
        this.currentValue = option;
        this.$emit('selectOption', this.currentValue);
      },
      closeMenu() {
        this.selectStatus = false;
      }
    }
  }
</script>

<style>
    .select-box {
        display: inline;
    }

    .select-container {
        /* Prevent text from highlighting on click */
        -webkit-touch-callout: none; /* iOS Safari */
        -webkit-user-select: none; /* Safari */
        -khtml-user-select: none; /* Konqueror HTML */
        -moz-user-select: none; /* Firefox */
        -ms-user-select: none; /* Internet Explorer/Edge */
        user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome and Opera */
        position: relative;
        display: inline;
    }

    .select-container select {
        /* Hide default select box */
        display: none;
    }

    .select-box--single {
        cursor: pointer;
        display: inline-block;
        padding: 10px;
        border: 1px solid #d8d8d8;
        border-radius: 3px;
        font-size: 16px;
        color: #222222;
        position: relative;
    }

    .select-active .select-box--single,
    .select-box--single:hover {
        background-color: #F9F9F9;
        border-color: #BBBBBB;
    }

    .select-box--dropdown {
        visibility: hidden;
        opacity: 0;
        list-style-type: none;
        position: absolute;
        padding: 0;
        margin: 0;
        border: 1px solid #d8d8d8;
        border-radius: 3px;
        font-size: 16px;
        z-index: 10;
        background: #fff;
        width: 100%;
        left: 0;
        text-align: left;
        -webkit-box-shadow: 0 4px 24px 0 rgba(0, 0, 0, 0.08);
        -moz-box-shadow: 0 4px 24px 0 rgba(0, 0, 0, 0.08);
        box-shadow: 0 4px 24px 0 rgba(0, 0, 0, 0.08);
    }

    .select-active .select-box--dropdown {
        visibility: visible;
        opacity: 1;
    }

    .select-box--dropdown li {
        display: block;
        cursor: pointer;
        padding: 10px;
        color: #222222;
    }

    .select-box--active,
    .select-box--dropdown li:hover {
        background: rgba(115, 115, 115, 0.1);
    }

    .select-box--active::after {
        border-style: solid;
        border-width: 1px 1px 0 0;
        content: '';
        display: inline-block;
        height: 5px;
        left: 0;
        position: relative;
        top: 7px;
        -webkit-transform: rotate(135deg);
        transform: rotate(135deg);
        vertical-align: top;
        width: 10px;
        margin-left: 10px;
    }

    .select-box--chevron {
        border-style: solid;
        border-width: 1px 1px 0 0;
        content: '';
        display: inline-block;
        height: 10px;
        right: 0;
        float: right;
        position: relative;
        top: 4px;
        -webkit-transform: rotate(135deg);
        transform: rotate(135deg);
        vertical-align: top;
        width: 10px;
        margin-left: 10px;
        border-color: #999999;
    }

    .select-active .select-box--chevron {
        -webkit-transform: rotate(315deg);
        transform: rotate(315deg);
        top: 10px;
    }
</style>