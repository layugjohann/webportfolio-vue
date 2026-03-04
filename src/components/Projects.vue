<template>
  <section class="container py-5" id="projects">
    <div class="text-center mb-4">
      <h2 class="fw-bold">My Projects</h2>
      <h4 class="text-muted">A preview of what I’ve built</h4>
    </div>

    <div class="project-scroll">
      <a
        v-for="project in projects"
        :key="project.id"
        :href="project.link"
        target="_blank"
        class="project-row text-decoration-none"
      >
        <div class="card project-card bg-secondary bg-opacity-25 border-0 rounded-4 p-3 h-100">
          <img v-if="project.img" :src="project.img" class="card-img-top" alt="" />
          <div class="card-body text-center">
            <h5 class="fw-bold text-muted">{{ project.title }}</h5>
            <p>{{ project.description }}</p>
          </div>
        </div>
      </a>
    </div>
  </section>
</template>

<script>
import projectsList from '../data/projects';

export default {
  name: 'Projects',
  data() {
    return {
      projects: projectsList
    };
  }
};
</script>

<style scoped>
.project-scroll {
  display: flex;
  overflow-x: auto; /* horizontal scroll */
  gap: 1rem;
  padding: 1rem 0;
  scroll-behavior: smooth;
}

.project-row {
  flex: 0 0 300px; /* fixed card width */
  min-width: 300px;
}

.project-scroll::-webkit-scrollbar {
  height: 8px;
}

.project-scroll::-webkit-scrollbar-thumb {
  background-color: rgba(0,0,0,0.3);
  border-radius: 4px;
}
</style>