<template>
  <div class="row">
    <div v-if="task" class="col s6 offset-s3">
      <h1>{{ task.title }}</h1>

      <form @submit.prevent = "submitHandler">
        <div class="chips" ref="chips"></div>

        <div class="input-field">
            <textarea v-model="description" id="description" class="materialize-textarea" data-length="120"></textarea>
            <label for="description">Description</label>
            <span class="character-counter" style="float: right; font-size: 12px;">{{description.length}}/2048</span>
        </div>

         <input type="text" ref="datepicker">

        <div v-if="task.status !== 'completed'">
          <button class="btn" type="submit" style="margin-right:300px">Update Task</button>
          <button class="btn" type="button" @click="completeTask">Complete Task</button>
        </div>
      </form>
    </div>
    <p v-else>Task is not found</p>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'

export default {
  computed: {
    task() {
      return this.$store.getters.taskById(+this.$route.params.id)
    }
  },
  data: () => ({
    description: '',
    chips: null,
    date: null
  }),
  mounted() {
    this.description = this.task.description;
    this.chips = M.Chips.init(this.$refs.chips, { placeholder: 'Task tags', data: this.task.tags });
    this.date = M.Datepicker.init(this.$refs.datepicker, { format: 'dd.mm.yyyy', defaultDate: new Date(this.task.date), setDefaultDate: true });
    setTimeout(() => {
      M.updateTextFields()
    }, 0)
  },
  methods: {
    submitHandler() {
      this.$store.dispatch('updateTask', {
        id: this.task.id,
        description: this.description,
        date: this.date.date
      })
      this.$router.push('/list')
    },
    completeTask() {
      this.$store.dispatch('completeTask', this.task.id)
      this.$router.push('/list')
    }
  },
  destroyed() {
    if (this.date && this.date.destroy) {
      this.date.destroy()
    }

    if (this.chips && this.chips.destroy) {
      this.chips.destroy()
    }
  }
}
</script>

<style lang="scss" scoped>
</style>>