<template>
  <div class="task-input">
    <b-form>
      <b-input-group class="mt-3">
        <template #append>
          <b-button
              variant="info"
              class="text-white"
              @click="save"
          >
            <b-icon icon="plus" aria-hidden="true"></b-icon>Add
          </b-button>
        </template>
        <b-form-input
            id="task-name"
            v-model="name"
            type="text"
        />
      </b-input-group>
    </b-form>
  </div>
</template>

<script>
export default {
  name: "Input",
  data() {
    return {
      name: ''
    }
  },
  methods: {
    async save() {
      if (this.name === '') {
        this.$bvToast.toast(
            'Please, you need to add something.',
            {
              title: 'Form warning',
              variant: 'warning',
              'auto-hide-delay': 5000
            }
        )
      }

      await fetch('http://localhost:3000/tasks', {
        method: 'POST',
        headers: { 'Content-type': 'application/json' },
        body: JSON.stringify({ name: this.name })
      }).then(() => {
        this.$bvToast.toast(
            'Task added.',
            {
              title: 'Form submit',
              variant: 'success',
              'auto-hide-delay': 5000
            }
        )
      }).catch(() => {
        this.$bvToast.toast(
            'Error during task added.',
            {
              title: 'Form error',
              variant: 'danger',
              'auto-hide-delay': 5000
            }
        )
      }).finally(() => {
        this.name = ''
      })
    }
  }
}
</script>

<style type="text/css" scoped>
.task-input {
  width: 100%;
  padding-left: 10rem;
  padding-right: 10rem;
}
</style>