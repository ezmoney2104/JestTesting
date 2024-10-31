<template>
  <div>
    <ul>
      <li class="card-list" v-for="(twin, index) in twinShowData" :key="twin.id">
        <h2>
          {{ twin.isFavorite ? '★' : '' }} {{ twin.btnTwin.nickname }}
          {{ twin.isFavorite ? '★' : '' }}
        </h2>
        <hr />
        <b-button pill variant="danger" @click="toggleDelete(index)"
          ><i class="bi bi-x-circle"></i> Delete</b-button
        >
        <b-button pill variant="warning" @click="toggleFavorite(index)">
          <i class="bi bi-star-fill"></i> Favorite</b-button
        >
        <b-button pill variant="success" @click="toggleDetails(index)">
          <i class="bi bi-info-circle"></i>
          {{ this.twinDetails.includes(this.twinShowData[index]) ? 'Hide' : 'Show' }}
          Details</b-button
        >
        <hr />
        <slot-card>
          <div v-if="this.twinDetails.includes(this.twinShowData[index])">
            <p v-for="[key, value] in Object.entries(twin.btnTwin)" :key="key">
              {{ label[key] }}:
              <span v-if="key == 'lovelanguage'">{{ value.join(', ') }}</span>
              <span v-else>{{ value }}</span>
            </p>
          </div>
        </slot-card>
      </li>
    </ul>

    <b-card v-if="twinShowData.length === 0" class="text-center">
      <b-card-text>No Listed Twin</b-card-text>
    </b-card>
  </div>
</template>

<script>
import SlotCard from '@/components/common/SlotCard.vue'
import { TWIN_OUTPUT } from '@/common/recipe/rTwinOutput'
import btnCrudMixin from '@/common/mixins/btnCrudMixin.js'

/**
 * Object of data from the registration form is injected.
 */
export default {
  name: 'FriendDetails',
  inject: ['twinShowData'],
  components: { SlotCard },
  mixins: [btnCrudMixin],
  data() {
    return {
      label: TWIN_OUTPUT,
      favorites: [],
      twinDetails: []
    }
  }
}
</script>

<style scoped>
ul {
  padding: 0 10px;
}

li {
  margin: 5px 0;
  list-style: none;
  overflow: auto;
}

p {
  padding: 0 10px;
}

.card-list {
  border: 2px solid rgba(0, 0, 0, 0.26);
  margin: 1.5rem auto;
  border-radius: 5px;
  padding: 1rem;
  text-align: center;
  width: 100%;
  max-width: 40rem;
}

#app h2 {
  font-size: 2.5rem;
  margin: 0;
}

button {
  font: inherit;
  cursor: pointer;
  border: 0;
  padding: 0.2rem 1.2rem;
  margin: 2px 10px;
}
@media only screen and (max-width: 400px) {
  button {
    width: auto;

    font-size: 0px;
    margin: 5px 2px;
  }
  i {
    font-size: 15px;
  }
}
</style>
