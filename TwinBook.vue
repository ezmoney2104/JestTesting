<template>
  <div class="twinpage">
    <common-header :item="headerProp"></common-header>
    <b-container class="page_container">
      <common-button
        v-for="header in HEADER_SWITCH"
        :key="header.id"
        :item="header"
        @action="getButtonAction"
      ></common-button>
    </b-container>
    <registration-form v-if="showRegistrationForm"></registration-form>
    <keep-alive>
      <friend-list v-if="showFriendList"></friend-list>
    </keep-alive>
  </div>
</template>

<script>
import RegistrationForm from '@/components/pages/SlamBook/RegistrationForm.vue'
import FriendList from '@/components/pages/SlamBook/FriendList.vue'
import CommonHeader from '@/components/common/CommonHeader.vue'
import CommonButton from '@/components/common/CommonButton.vue'
import { HEADER_BTN_PROPERIES } from '@/common/recipe/rCommonButton.js'
import { HEADER_PROPERTIES } from '@/common/recipe/rCommonHeader'

export default {
  name: 'TwinBook',
  Components: { RegistrationForm, CommonHeader, CommonButton, FriendList },
  data() {
    return {
      HEADER_SWITCH: HEADER_BTN_PROPERIES,
      showRegistrationForm: false,
      showFriendList: true,
      twinListArray: [],
      headerProp: HEADER_PROPERTIES
    }
  },
  /**
   * Return array from the form reistration.
   */
  provide() {
    return {
      twinShowData: this.twinListArray
    }
  },
  methods: {
    /**
     * Display registration form or friendlist.
     * @param {string} action The button clicked.
     */
    getButtonAction(action) {
      if (action === 'add') {
        this.showRegistrationForm = true
        this.showFriendList = false
      } else if (action === 'display') {
        this.showFriendList = true
        this.showRegistrationForm = false
      }
    }
  }
}
</script>

<style scoped>
.twinpage {
  margin: 10px 10px;
  padding: 10px 300px;
  background-color: rgb(236, 247, 247);
}
.page_container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 0 60px;
}
@media only screen and (max-width: 1440px) {
  .twinpage {
    padding: 10px 10px;
  }
}
@media only screen and (max-width: 500px) {
  .twinpage {
    padding: 10px 0;
  }
}
</style>
