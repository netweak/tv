<template>
  <div class="bg-black h-screen text-white">
    <template v-if="!loading">
      <div class="p-8">
        <div class="grid grid-cols-3">
          <div class="bg-gray-800 rounded-md">
            <div class="p-4 text-center">
              <h3 class="font-bold">Everything is online</h3>
            </div>
          </div>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    token: String
  },
  data() {
    return {
      loading: true,
      servers: null
    }
  },
  mounted() {
    axios.get('https://netweak.com.test/api/servers', {headers: {Authorization: `Bearer ${this.token}`}})
      .then(response => {
        this.loading = false;
        this.servers = response.data.data;
      });
  }
}
</script>
