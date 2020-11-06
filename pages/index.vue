<template>
  <div class="mx-auto max-w-screen-md px-6 pt-12">
    <FlashMessage :message="message" :onClose="() => message = null" />
    <div class="fixed top-0 right-0 mt-12 mr-32 bg-gray-300 shadow-md p-6 pr-10 rounded-lg w-1/5 z-10" role="alert" v-if="userDelete.id">
      <div class="flex items-center">Delete user with name {{ userDelete.first_name + ' ' + userDelete.last_name }}?</div>
      <div class="shadow bg-gray-600 text-white font-bold py-2 px-4 rounded flex" v-if="deleteLoading">
        <svg class="animate-spin h-5 w-5 mr-3" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
          <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
        Deleting
      </div>
      <div v-else>
        <button
          class="shadow bg-red-600 hover:bg-red-200 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
          @click="deleteUser(userDelete.id)"
        >
          Delete
        </button>
        <button
          class="shadow bg-gray-200 hover:bg-gray-400 focus:shadow-outline focus:outline-none text-black font-bold py-2 px-4 rounded"
          @click="userDelete = {}"
        >
          Cancel
        </button>
      </div>
    </div>
    <div>
      <button
        class="shadow bg-gray-900 hover:bg-gray-600 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
        @click="createForm = true"
        v-if="createForm === false"
      >
        Add User
      </button>
      <UserForm
        v-else
        :onSubmit="createUser"
        :isLoading="createLoading"
        :onCancel="cancelCreate"
      />
    </div>
    <div class="bg-gray-800 rounded-lg shadow">
      <div class="">
        <div
          v-for="o in data"
          :key="o.id"
          class="flex items-center px-6 py-4 list relative"
        >
          <div class="mr-5">
            <img
              :src="o.avatar"
              :alt="o.first_name"
              class="rounded-full"
              style="width: 50px"
            >
          </div>
          <UserForm
            v-if="o.id === userEdit.id"
            :userEdit="userEdit"
            :onSubmit="updateUser"
            :isLoading="updateLoading"
            :onCancel="cancelEdit"
          />
          <div v-else>
            <nuxt-link to="/" class="font-semibold text-white">
              {{ o.email }}
            </nuxt-link>
            <div class="text-gray-500">
              {{ o.first_name }} {{ o.last_name }}
            </div>
          </div>
          <div
            class="ml-auto"
            v-if="o.id !== userEdit.id"
          >
            <a
              href="#"
              class="uppercase text-sm text-white text-opacity-75 hover:text-opacity-100 font-semibold"
              @click.prevent="showEdit(o)"
            >
              Edit
            </a>
            <a
              href="#"
              class="uppercase text-sm text-red-500 text-opacity-75 hover:text-opacity-100 font-semibold"
              @click.prevent="deleteConfirmation(o)"
            >
              Delete
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import UserForm from '../components/UserForm'
import FlashMessage from '../components/FlashMessage'

export default {
  data() {
    return {
      response: {},
      newUser: {},
      firstName: '',
      lastName: '',
      email: '',
      userEdit: {
        id: null,
        firstName: '',
        lastName: '',
        email: ''
      },
      updateLoading: false,
      createLoading: false,
      deleteLoading: false,
      createForm: false,
      userDelete: {},
      message: null
    }
  },
  computed: {
    data() {
      return this.response.data
    },
    meta() {
      const {
        page,
        per_page,
        total_pages,
        total
      } = this.response

      return {
        page,
        per_page,
        total_pages,
        total,
      }
    }
  },
  mounted() {
    this.getData()
  },
  methods: {
    async getData() {
      const response = await this.$axios.$get('users')
      this.response = response
    },
    async createUser(firstName, lastName, email) {
      this.createLoading = true
      const newUser = await this.$axios.$post('users', { firstName, lastName, email })
      this.newUser = newUser
      console.log('success \n', newUser)

      this.createLoading = false
      this.createForm = false

      this.message = {
        type: 'success',
        text: 'User Created!'
      }
    },
    async updateUser(firstName, lastName, email, id) {
      this.updateLoading = true
      const updated = await this.$axios.$put(`users/${id}`, { firstName, lastName, email })
      this.userEdit.id = null
      console.log(updated)
      this.updateLoading = false

      this.message = {
        type: 'success',
        text: 'User Updated!'
      }
    },
    deleteConfirmation(user) {
      this.userDelete = user
    },
    async deleteUser(id) {
      this.deleteLoading = true
      const deleteResponse = await this.$axios.delete(`users/${id}`)
      console.log('delete user \n', deleteResponse)
      this.deleteLoading = false
      this.userDelete = {}

      this.message = {
        type: 'success',
        text: 'User Deleted!'
      }
    },
    showEdit({ id, first_name: firstName, last_name: lastName, email }) {
      this.userEdit = { id, firstName, lastName, email }
    },
    cancelCreate() {
      this.createForm = false
    },
    cancelEdit() {
      this.userEdit.id = null
    }
  },
  components: {
    UserForm,
    FlashMessage
  }
}
</script>

<style>
.list:after {
  content: '';
  @apply border-b absolute bottom-0 left-0 right-0 ml-24 border-gray-700;
}
</style>
