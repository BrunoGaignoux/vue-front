<template>
  <div id="app">
    <div class="title">
      <h3>Tasks ({{ list.length }})</h3>
    </div>
    <Form @submit="load += 1" />
    <TaskList :key="load" @load="list = $event" />
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {
    Form: () => import('./components/Form'),
    TaskList: () => import('./components/TaskList')
  },
  data() {
    return {
      list: [],
      load: 0
    }
  }
}
</script>

<style type="text/css">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.title {
  width: 100%;
  padding-left: 10rem;
  padding-right: 10rem;
  text-align: left;
}
</style>
