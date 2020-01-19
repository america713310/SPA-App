<template>
  <div>
    <h1>List</h1>
    <div class="row">
      <div class="s6 col">
        <select ref="select" v-model="filter">
          <option value="" disabled selected>Choose your option</option>
          <option value="active">Active</option>
          <option value="outdated">Outdated</option>
          <option value="completed">Completed</option>
        </select>
        <button v-if="filter" class="btn red" @click="filter = null">Clear Filter</button>
      </div>
    </div>
    <label>Status filter</label>
    <hr />
    <table v-if="tasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Date</th>
          <th>Description</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, idx) of displayTasks" :key="task.id">
          <td>{{ idx + 1 }}</td>
          <td>{{ task.title }}</td>
          <td>{{ new Date(task.date).toLocaleDateString() }}</td>
          <td class="tdNewFormat">
            <div class="textAreaNewFormat">{{ task.description }}</div>
          </td>
          <td>{{ task.status }}</td>
          <td>
            <router-link
              tag="button"
              class="btn btn-small"
              :to="'/task/' + task.id"
              >Open task</router-link
            >
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No tasks</p>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  data: () => ({
    filter: null
  }),
  computed: {
    tasks() {
      return this.$store.getters.tasks;
    },
    displayTasks() {
      return this.tasks.filter(x => {
        if (!this.filter) {
          return true
        }
        return x.status === this.filter
      })
    }
  },
  mounted() {
    M.FormSelect.init(this.$refs.select);
  }
};
</script>

<style lang="scss" scoped>
.tdNewFormat {
  max-width: 400px;
}
.textAreaNewFormat {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}</style
>>
