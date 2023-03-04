<template>
  <div v-if="projects">
    <h1 class="PageTitle">Liste des projets</h1>
    <div v-if="types" id="TypesContainer">
      <div class="TypesButtons">
        <button class="test" @click="filterProjects('all')">Tout</button>
        <button
          :class="{
            'filter-button': true,
            active: activeFilter.value === type,
          }"
          v-for="type in types"
          @click="filterProjects(type)"
          :key="type"
        >
          {{ type }}
        </button>
      </div>
    </div>
    <div id="ProjectContainer">
      <div  class="ProjectPreview"
        v-for="project in filteredProjects"
        :key="project.id"
      >
        <img :src="project.Cover.url" alt="Image du projet">
        <button @click="() => $router.push(`/projects/${project.slug}`)">
          {{ project.Name }}
        </button>
       </div>
    </div>
  </div>
</template>

<script setup>

const { find } = useStrapi();
const projects = ref();

const types = ref([]);
const activeFilter = ref('all');

const filterProjects = (type) => {
  activeFilter.value = type;
};

const filteredProjects = computed(() => {
  if (activeFilter.value === "all") return projects.value.data;
  return projects.value.data.filter(
    (project) => project.Type === activeFilter.value
  );
});

onMounted(async () => {
  projects.value = await find("projects", { populate: "*" });
  types.value = new Set(
    projects.value.data.map((project) => {
      console.log(project)
      return project.Type;
    })
  );
});
</script>
