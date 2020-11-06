<template>
  <div class="m-auto w-1/2">
    <form class="w-full max-w-sm" @submit.prevent="submitForm">
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/3">
          <label class="block text-gray-600 font-bold md:text-right mb-1 md:mb-0 pr-4" for="inline-full-name">
            First Name
          </label>
        </div>
        <div class="md:w-2/3">
          <input class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-gray-600" id="inline-full-name" type="text" placeholder="First Name" v-model="firstName">
        </div>
      </div>
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/3">
          <label class="block text-gray-600 font-bold md:text-right mb-1 md:mb-0 pr-4" for="inline-full-name">
            Last Name
          </label>
        </div>
        <div class="md:w-2/3">
          <input class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-gray-600" id="inline-full-name" type="text" placeholder="Last Name" v-model="lastName">
        </div>
      </div>
      <div class="md:flex md:items-center mb-6">
        <div class="md:w-1/3">
          <label class="block text-gray-600 font-bold md:text-right mb-1 md:mb-0 pr-4" for="email">
            Email
          </label>
        </div>
        <div class="md:w-2/3">
          <input class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-gray-600" id="email" type="email" placeholder="Email" v-model="email">
        </div>
      </div>
      <div class="md:flex md:items-center">
        <div class="md:w-1/3"></div>
        <div class="md:w-2/3">
          <button class="shadow bg-gray-600 text-white font-bold py-2 px-4 rounded flex" disabled v-if="isLoading">
            <svg class="animate-spin h-5 w-5 mr-3" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
            {{
              userEdit ? 'Updating' : 'Creating'
            }}
          </button>
          <div v-else>
            <button class="shadow bg-gray-900 hover:bg-gray-600 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" type="submit">
              {{
                userEdit ? 'Update' : 'Create'
              }} User
            </button>
            <button class="shadow bg-gray-200 hover:bg-gray-400 focus:shadow-outline focus:outline-none text-red-500 font-bold py-2 px-4 rounded" type="button" @click="onCancel">
              Cancel
            </button>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: ['onSubmit', 'userEdit', 'isLoading', 'onCancel'],
  data() {
    return {
      id: null,
      firstName: '',
      lastName: '',
      email: '',
    }
  },
  mounted() {
    if (this.userEdit) {
      for (let key of Object.keys(this.userEdit)) {
        this[key] = this.userEdit[key]
      }
    }
  },
  methods: {
    async submitForm() {
      await this.onSubmit(this.firstName, this.lastName, this.email, this.id)
      this.firstName = ''
      this.lastName = ''
      this.email = ''
    }
  }
}
</script>
