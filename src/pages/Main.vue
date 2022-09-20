<template>
  <div class="hg-main-page">
    <organism-dropdown v-if="dropdownData" :dropdown-data="dropdownData" @select="selectItem"/>
  </div>
</template>

<script>
import OrganismDropdown from '@/components/Dropdown';

export default {
  name: "Main-page",
  components: {
    OrganismDropdown
  },
  data() {
    return {
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
      ]
    }
  },
  methods: {
    selectItem(itemID) {
      console.log('itemID: ', itemID)
    }
  }
}
</script>

<style lang="sass" scoped>
@import '@/assets/styles/pages/Main.scss'
</style>
