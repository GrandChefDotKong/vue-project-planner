<template>
  <div class="home">
    <filter-nav :current="current" @filterChange="current = $event" />
    <div v-if="projects.length">
        <div v-for="project in filteredProjects" :key="project.id">
          <single-project :project="project" @complete="handleComplete" @delete="handleDelete" />
        </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue';
import FilterNav from '../components/FilterNav.vue';

export default {
  name: 'HomeView',
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter(project => { 
        return project.id != id 
      })
    },
    handleComplete(id) {
      console.log("completed")
      let p = this.projects.find(project => { 
        return project.id === id
      });
      p.complete = !p.complete;
    }
  },
  computed: {
    filteredProjects() {
      if(this.current === 'completed') {
        return this.projects.filter((p) => p.complete);
      } else if ( this.current === 'ongoing') {
        return this.projects.filter((p) => !p.complete);
      }

      return this.projects;
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err));
  }
}
</script>
