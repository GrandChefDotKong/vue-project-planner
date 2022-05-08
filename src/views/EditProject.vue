<template>
  <h1>Edit</h1>
  <div v-if="project">
    <form @submit.prevent="handleSubmit">
        <label for="">Title :</label>
        <input v-model="project.title" type="text" required>
        <label for="">Details :</label>
        <textarea v-model="project.details" required></textarea>
        <button>Update Project</button>
    </form>
  </div>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      project: null,
      uri: `http://localhost:3000/projects/${this.id}`

    }
  },
  mounted() {
    fetch(this.uri)
      .then(res => res.json())
      .then(data => this.project = data)
      .catch(err => console.log(err));
  },
  methods: {
    handleSubmit() {
        fetch(this.uri, { 
            method: 'PATCH', 
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(this.project)
            })
            .then(() => {
                this.$router.push('/');
            })
            .catch(err => console.log(err));
    }
  }
}
</script>

<style>

</style>