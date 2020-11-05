<template>
  <div class="mx-auto max-w-screen-md px-6 pt-12">
    <div>
      <form @submit.prevent="createUser">
        <input type="text" placeholder="name" v-model="name">
        <input type="text" placeholder="email" v-model="email">
        <button type="submit">Create new user</button>
      </form>
      <p>{{ name }} {{ email }}</p>
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
          <div v-if="o.id === userEdit.id">
            <form action="" @submit.prevent="updateUser">
              <input type="text" placeholder="name" v-model="userEdit.name">
              <input type="text" placeholder="email" v-model="userEdit.email">
              <button type="submit">Update</button>
            </form>
          </div>
          <div v-else>
            <nuxt-link to="/" class="font-semibold text-white">
              {{ o.email }}
            </nuxt-link>
            <div class="text-gray-500">
              {{ o.first_name }} {{ o.last_name }}
            </div>
          </div>
          <div class="ml-auto">
            <a
              href="#"
              class="uppercase text-sm text-white text-opacity-75 hover:text-opacity-100 font-semibold"
              @click.prevent="showEdit(o)"
            >
              {{ o.id === userEdit.id ? 'Cancel' : 'Edit' }}
            </a>
            <a
              href="#"
              class="uppercase text-sm text-red-500 text-opacity-75 hover:text-opacity-100 font-semibold"
              @click.prevent="deleteUser(o.id)"
              v-if="o.id !== userEdit.id"
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
export default {
  data() {
    return {
      response: {},
      newUser: {},
      name: '',
      email: '',
      userEdit: {
        id: null,
        name: '',
        email: ''
      }
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
    async createUser() {
      const { name, email } = this
      const newUser = await this.$axios.$post('users', { name, email })
      this.newUser = newUser
      console.log('success \n', newUser)

      this.name = ''
      this.email = ''
    },
    async updateUser() {
      const { name, email, id } = this.userEdit
      const updated = await this.$axios.$put(`users/${id}`, { name, email })
      this.userEdit.id = null
      console.log(updated)
    },
    async deleteUser(id) {
      const deleteResponse = await this.$axios.delete(`users/${id}`)
      console.log('delete user \n', deleteResponse)
    },
    showEdit({ id, first_name, last_name, email }) {
      if (id === this.userEdit.id) return this.userEdit.id = null
      const name = `${first_name} ${last_name}`
      this.userEdit = { id, name, email }
    }
  }
}
</script>

<style>
.list:after {
  content: '';
  @apply border-b absolute bottom-0 left-0 right-0 ml-24 border-gray-700;
}
</style>
