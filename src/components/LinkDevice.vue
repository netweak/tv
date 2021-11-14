<template>
  <div class="h-screen py-10 flex flex-col justify-center items-center bg-gray-100">
    <div>
      <a href="https://netweak.com">
        <img class="w-64" src="https://netweak.com/img/logo/512h/logo-dark.png" alt="Netweak Logo">
      </a>
    </div>

    <div class="container max-w-lg px-5 my-6">
      <div class="bg-white shadow-md rounded-lg">
        <div class="p-8">
          <div class="mb-4 text-center">
            <h1 class="text-2xl">To login, visit <span class="text-blue-500">netweak.com/link</span></h1>
            <p class="text-gray-700">on a different device and enter the code below.</p>
          </div>

          <h1 v-if="code" class="py-1 text-center text-7xl text-gray-700 font-medium tracking-widest">{{code}}</h1>
          <div v-else class="animate-pulse">
            <div class="h-20 w-52 mx-auto bg-gray-400 rounded"></div>
          </div>

          <div class="mt-4">
            <svg class="block mx-auto" width="38" height="38" viewBox="0 0 38 38" xmlns="http://www.w3.org/2000/svg" stroke="#333">
              <g fill="none" fill-rule="evenodd">
                <g transform="translate(1 1)" stroke-width="2">
                  <circle stroke-opacity=".5" cx="18" cy="18" r="18"/>
                  <path d="M36 18c0-9.94-8.06-18-18-18">
                    <animateTransform
                      attributeName="transform"
                      type="rotate"
                      from="0 18 18"
                      to="360 18 18"
                      dur="1s"
                      repeatCount="indefinite"/>
                  </path>
                </g>
              </g>
            </svg>
          </div>
        </div>
      </div>
    </div>

    <p class="text-center text-gray-400 text-xs">
      &copy; 2019 — 2021 Netweak. All rights reserved.
    </p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  emits: ['tokenFetched'],
  data() {
    return {
      code: null,
      key: null,
      token: null,
      interval: null
    }
  },
  mounted() {
    setTimeout(() => this.getCode(), 3000);

    setInterval(() => {
      clearInterval(this.interval);
      this.getCode();
    }, 5 * 60 * 1000)
  },
  methods: {
    getCode() {
      axios.get('https://netweak.com.test/api/link/register', {params: {device: 'Netweak for TV'}})
        .then(response => {
          this.code = response.data.code;
          this.key = response.data.key;

          this.interval = setInterval(() => {
            axios.get('https://netweak.com.test/api/link/token', {params: {code: this.code, key: this.key}})
              .then(response => {
                this.token = response.data.token;
                this.$emit('tokenFetched', this.token);
              });
          }, 8000);
        });
    }
  },
  beforeUnmount() {
    clearInterval(this.interval);
  }
}
</script>