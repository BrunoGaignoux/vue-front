<template>
  <div class="list">
    <div v-if="list.length > 0">
      <b-card v-for="(item, index) of list" :key="index">
        <div class="flex-row justify-content-between">
          <div
              @click="changeStatus(item.id, !item.completed)"
              :class="`${ item.completed ? 'bg-success' : 'bg-light'} text-gray w-`"
          >
            {{ item.name }}
          </div>
          <div>
            <b-button variant="danger" class="text-white">
              <b-icon icon="x" aria-hidden="true" @click="remove(item.id)">
              </b-icon>
            </b-button>
          </div>
        </div>
      </b-card>
    </div>
    <div v-else class="justify-content-center">
      Empty data
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      list: []
    }
  },
  async created() {
    await this.get()
  },
  methods: {
    async get() {
      await fetch('http://localhost:3000/tasks')
        .then(response => {
          this.list = response.data
        }).catch(() => {
          this.$bvToast.toast(
              'Error during fetch tasks.',
              {
                title: 'List error',
                variant: 'danger',
                'auto-hide-delay': 5000
              }
          )
        })
    },
    async changeStatus(id, status) {
      await fetch(`http://localhost:3000/tasks/${id}`, {
        method: 'PUT',
        headers: { 'Content-type': 'application/json' },
        body: JSON.stringify({ status })
      })
      .then(async () => {
        await this.get()
      }).catch(() => {
        this.$bvToast.toast(
            'Error during delete task.',
            {
              title: 'List error',
              variant: 'danger',
              'auto-hide-delay': 5000
            }
        )
      })
    },
    async remove(id) {
      await fetch(`http://localhost:3000/tasks/${id}`, {
        method: 'DELETE'
      })
      .then(async () => {
        await this.get()
      }).catch(() => {
        this.$bvToast.toast(
            'Error during delete task.',
            {
              title: 'List error',
              variant: 'danger',
              'auto-hide-delay': 5000
            }
        )
      })
    }
  }
}
</script>

<style type="text/css" scoped>
.list {
  width: 100%;
  padding-top: 5rem;
  padding-left: 5rem;
  padding-right: 5rem;
}
</style>
