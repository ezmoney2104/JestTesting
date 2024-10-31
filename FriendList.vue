<template>
  <div style="margin: 40px 0px">
    <section>
      <b-card header-tag="header" header-bg-variant="dark" header-text-variant="white">
        <template #header>
          <h4 class="mb-0" style="text-align: center">MY TWINS</h4>
        </template>
        <ul style="padding: 0">
          <friend-details></friend-details>
        </ul>
      </b-card>
    </section>
  </div>
</template>

<script>
import FriendDetails from '@/components/pages/SlamBook/FriendDetails.vue'

export default {
  name: 'FriendList',
  components: { FriendDetails },
  /**
   * Gives the array value of form inputs
   * @return The array of form inputs
   */
  data() {
    return {
      twinPerson: []
    }
  }
}
</script>
