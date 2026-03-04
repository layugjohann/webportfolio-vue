<template>
  <div class="project-scroll">
    <div class="project-row" id="project-card">
      <a
        v-for="(project, index) in projects"
        :key="index"
        :href="project.link"
        target="_blank"
        class="projet-row"
      >
        <div class="card project-card bg-secondary bg-opacity-25 border-0 rounded-4 p-3 h-100">
          <img :src="project.img" class="card-img-top" :alt="project.title" />
          <div class="card-body text-center">
            <h5 class="fw-bold text-muted">{{ project.title }}</h5>
            <p>{{ project.description }}</p>
          </div>
        </div>
      </a>
    </div>
  </div>
</template>

<script>
import projectsData from '../data/projects';

export default {
  name: "ProjectList",
  data() {
    return {
      projects: projectsData
    };
  },
};
</script>

<style scoped>
.project-scroll {
  display: flex;
  overflow-x: auto;
  gap: 1rem;
  padding: 1rem 0;
}
.project-row {
  flex: 0 0 auto;
}
</style>