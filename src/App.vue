<template>
  <LinkDevice v-if="!token" @tokenFetched="value => setToken(value)"/>
  <Dashboard v-else :token="token"/>
</template>

<script>
import axios from 'axios';
import LinkDevice from './components/LinkDevice.vue';
import Dashboard from './components/Dashboard.vue';

export default {
  components: {
    LinkDevice,
    Dashboard
  },
  data() {
    return {
      token: localStorage.getItem('token') ?? null
    }
  },
  methods: {
    setToken(value) {
      this.token = value;
      localStorage.setItem('token', value);
    }
  },
  mounted() {
    if (this.token !== null) {
      axios.get('https://netweak.com.test/api/hello', {headers: {Authorization: `Bearer ${this.token}`}})
        .then(response => console.log(response.data))
        .catch(() => {
          //localStorage.clear();
          //this.token = null;
        });
    }
  }
}
</script>

