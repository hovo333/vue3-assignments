<template>
  <div class="hg-main-page">
    <h1>Props example</h1>
    <div class="flex hg-dropdown-content">
      <organism-dropdown
          v-if="dropdownData"
          :visible="showDropdown1"
          :dropdown-data="dropdownData"
          @select="selectItem"
      />
    </div>

    <h1>Slots example</h1>
    <div class="flex hg-dropdown-content">
      <organism-dropdown v-if="dropdownData"
                         :visible="showDropdown2"
                         :custom-button="true"
                         :custom-filter="true"
                         :custom-content="true"
      >
        <!-- Button content -->
        <template v-slot:button>
          <span class="px-2 py-2 border rounded inline-flex items-center text-sm" @click="showDropdown2 = true">
            <span class="mr-2">Dropdown2</span>

            <svg class="w-4 h-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
              <path d="M4.516 7.548c.436-.446 1.043-.481 1.576 0L10 11.295l3.908-3.747c.533-.481 1.141-.446 1.574 0 .436.445.408 1.197 0 1.615-.406.418-4.695 4.502-4.695 4.502a1.095 1.095 0 0 1-1.576 0S4.924 9.581 4.516 9.163c-.409-.418-.436-1.17 0-1.615z" />
            </svg>
          </span>
        </template>

        <!-- Button content -->
        <template v-slot:filter>
          <div class="flex hg-filter-content">
            <img id="reorderIcon"
                 v-if="!slotDropDownSettings.order"
                 title="Order"
                 src="../assets/images/icon-sort.svg"
                 alt="reorderOrder"
                 v-on:click="reorder"
            />
            <img id="reorderIcon"
                 v-else
                 src="../assets/images/icon-sort-active.svg"
                 :title="slotDropDownSettings.order === 1 ? 'Reverse order' : 'Cancel order'"
                 alt="reorderOrder"
                 v-on:click="reorder"
            />
          </div>
        </template>

        <!-- Opened dropdown content -->
        <template v-slot:content>
          <a v-for="(item, index) in dropdownItems"
             v-bind:key="`dropdown_${index}`"
             class="flex w-full justify-between items-center rounded px-2 py-1 my-1 hover:bg-indigo-600 hover:text-white"
             href="#"
             @click="selectItem(item.id)"
          >{{ item.id }}</a>
        </template>
      </organism-dropdown>
    </div>
  </div>
</template>

<script>
import OrganismDropdown from '@/components/Dropdown';

export default {
  name: "Main-page",
  components: {
    OrganismDropdown
  },
  computed: {
    dropdownItems() {
      return this.dropdownData
          .filter(item => this.isShowAll ? 1 === 1: item.hide === false)
          .sort((a, b) => this.slotDropDownSettings.order === 1
              ? a.order - b.order : this.slotDropDownSettings.order === 2
                  ? b.order - a.order : 1 === 1)
    }
  },
  data() {
    return {
      showDropdown1: false,
      showDropdown2: false,
      dropdownTitle: 'Main',
      dropdownData: [
        {
          id: 1014,
          order: 2,
          hide: false
        },
        {
          id: 4352,
          order: 3,
          hide: true
        },
        {
          id: 23,
          order: 1,
          hide: false
        },
        {
          id: 144,
          order: 4,
          hide: false
        },
        {
          id: 543,
          order: 6,
          hide: false
        },
        {
          id: 5719,
          order: 5,
          hide: true
        }
      ],
      slotDropDownSettings: {
        order: 0
      }
    }
  },
  methods: {
    selectItem(itemID) {
      console.log('itemID: ', itemID)
      this.showDropdown = false
    },
    reorder() {
      if (this.slotDropDownSettings.order === 2) {
        this.slotDropDownSettings.order = 0
      } else {
        this.slotDropDownSettings.order++
      }
    }
  }
}
</script>

<style lang="sass" scoped>
@import '@/assets/styles/pages/Main.scss'
</style>
