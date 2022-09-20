<template>
  <div class="relative">
    <div class="flex">
      <button class="z-10 relative flex items-center focus:outline-none select-none">
        <div class="flex">
          <div @click="open = !open">
            <slot name="button" v-if="customButton"></slot>

            <span v-else class="px-2 py-2 border rounded inline-flex items-center text-sm">
              <span class="mr-2">{{ dropdownTitle }}</span>

              <svg class="w-4 h-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                <path d="M4.516 7.548c.436-.446 1.043-.481 1.576 0L10 11.295l3.908-3.747c.533-.481 1.141-.446 1.574 0 .436.445.408 1.197 0 1.615-.406.418-4.695 4.502-4.695 4.502a1.095 1.095 0 0 1-1.576 0S4.924 9.581 4.516 9.163c-.409-.418-.436-1.17 0-1.615z" />
              </svg>
            </span>
          </div>

          <slot name="filter" v-if="customFilter"></slot>
          <div class="flex hg-filter-content" v-else-if="!customContent">
            <img id="reorderIcon"
                 v-if="!order"
                 :class="{'hg-disabled-icon': !open}"
                 title="Order"
                 src="../assets/images/icon-sort.svg"
                 alt="reorderOrder"
                 v-on:click="reorder"
            />
            <img id="reorderIcon"
                 v-else
                 :class="{'hg-disabled-icon': !open}"
                 src="../assets/images/icon-sort-active.svg"
                 :title="order === 1 ? 'Reverse order' : 'Cancel order'"
                 alt="reorderOrder"
                 v-on:click="reorder"
            />

            <img id="showAll"
                 v-if="!isShowAll"
                 :class="{'hg-disabled-icon': !open}"
                 src="../assets/images/icon-show-all.svg"
                 title="Show all items"
                 width="25"
                 alt="reorderOrder"
                 v-on:click="showAll"
            />
            <img id="showAll"
                 v-else
                 :class="{'hg-disabled-icon': !open}"
                 src="../assets/images/icon-show-all-active.svg"
                 title="Hide hidden items"
                 width="25"
                 alt="reorderOrder"
                 v-on:click="showAll"
            />
          </div>
        </div>

      </button>
    </div>

    <!-- to close when clicked on space around it in desktop-->
    <button class="fixed inset-0 h-full w-full cursor-default focus:outline-none" v-if="open" @click="open = false" tabindex="-1"></button>
    <!--dropdown content: desktop-->
    <transition enter-active-class="transition-all duration-200 ease-out" leave-active-class="transition-all duration-750 ease-in" enter-class="opacity-0 scale-75" enter-to-class="opacity-100 scale-100" leave-class="opacity-100 scale-100" leave-to-class="opacity-0 scale-75">
      <div class="hidden md:block absolute shadow-lg border w-48 rounded py-1 px-2 text-sm mt-4 bg-white hg-drop-down-content" :class="placement === 'right' ? 'right-0' : 'left-0'" v-if="open">
        <slot name="content" v-if="customContent"></slot>
        <a v-else
           v-for="(item, index) in dropdownItems"
           v-bind:key="`dropdown_${index}`"
           class="flex w-full justify-between items-center rounded px-2 py-1 my-1 hover:bg-indigo-600 hover:text-white"
           href="#"
           @click="selectItem(item.id)"
        >{{ item.id }}</a>
      </div>
    </transition>

    <!--dropdown content: mobile-->
    <transition enter-active-class="transition-all duration-200 ease-out" leave-active-class="transition-all duration-750 ease-in" enter-class="opacity-0 scale-75" enter-to-class="opacity-100 scale-100" leave-class="opacity-100 scale-100" leave-to-class="opacity-0 scale-75">
      <div class="md:hidden fixed inset-x-0 bottom-0 bg-white w-full z-20 px-2 py-2 shadow-2xl leading-loose hg-drop-down-content" v-if="open">
        <slot name="content" v-if="customContent"></slot>
        <a v-else
           v-for="(item, index) in dropdownData"
           v-bind:key="`dropdown_${index}`"
           class="flex w-full justify-between items-center rounded px-2 py-1 my-1 hover:bg-indigo-600 hover:text-white"
           href="#"
           @click="selectItem(item.id)"
        >{{ item.id }}</a>
      </div>
    </transition>
    <!-- to close when clicked on space around it in mobile-->
    <div class="md:hidden fixed w-full h-full inset-0 bg-gray-900 opacity-50 z-10" @click="open = false" v-if="open"></div>
  </div>
</template>

<script>
import mitt from 'mitt'
const emitter = mitt()

export default {
  name: "Dropdown-component",
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    placement: {
      type: String,
      default: "left",
      validator: (value) => ["right", "left"].indexOf(value) !== -1,
    },
    dropdownTitle: {
      type: String,
      default: 'Dropdown'
    },
    dropdownData: {
      type: Array,
      required: false
    },
    customButton: {
      type: Boolean,
      default: false
    },
    customContent: {
      type: Boolean,
      default: false
    },
    customFilter: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    dropdownItems() {
      return this.dropdownData
          .filter(item => this.isShowAll ? 1 === 1: item.hide === false)
          .sort((a, b) => this.order === 1
              ? a.order - b.order : this.order === 2
                  ? b.order - a.order : 1 === 1)
    }
  },
  data() {
    return {
      open: this.visible,
      order: 0,
      isShowAll: false,
    };
  },
  watch: {
    'visible': {
      handler() {
        this.open = this.visible
      }
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    reorder() {
      if (!this.open) return

      if (this.order === 2) {
        this.order = 0
      } else {
        this.order++
      }
    },
    showAll() {
      if (!this.open) return

      this.isShowAll = !this.isShowAll
    },
    init() {
      const onEscape = (e) => {
        if (e.key === "Esc" || e.key === "Escape") {
          this.open = false;
        }
      };

      document.addEventListener("keydown", onEscape);

      emitter.on("hook:beforeDestroy", () => {
        document.removeEventListener("keydown", onEscape);
      });
    },
    selectItem(itemID) {
      this.open = false
      this.$emit('select', itemID)
    }
  }
};
</script>


<style lang="sass" scoped>
@import '@/assets/styles/components/Dropdown.scss'
</style>
