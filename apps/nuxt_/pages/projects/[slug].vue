<template>
    <div v-if="project">
        <nuxt-link class="back-link" :to="{path: '/'}" id="RetourAccueil">↩ Retour à l'accueil</nuxt-link>
        <h1 class="PageTitle">{{  project.Name }}</h1>
        <div v-if="project.Images.length" class="mainDiv">
            <div id="ImagesDataContainer">
                <div v-for="image in project.Images" :key="image.id" class="ImageContainer">
                    <img :src="image.url" alt="Image du projet" style="height: auto;" class="Image">
                </div>
            </div>
            <hr>
        </div>
        <div id="DataContainer">
            <div class="DataObject">
                <h2>Description du projet :</h2>
                <p>{{ project.Description }}</p>
            </div>
            <hr>
            <div class="DataObject2">
                <div v-if="project.technologies.length"  class="DataObjectChild">
                    <h2>Technologies :</h2>
                    <ul>
                        <li v-for="technologie in project.technologies" :key="technologie.id">{{ technologie.Name }}</li>
                    </ul>
                </div>
            </div>
            <hr>
            <div class="DataObject2">
                <div v-if="project.developers.length"  class="DataObjectChild">
                    <h2>Développeurs :</h2>
                    <ul>
                        <li v-for="developer in project.developers" :key="developer.id">{{ developer.Name }} | {{ developer.Experience }}</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
    const {findOne} = useStrapi();
    const route = useRoute();

    const project = ref();

    onMounted(async () => {
        project.value = await findOne(`projects?filters[slug]=${route.params.slug}&populate=*`);
        project.value = project.value.data[0];
        console.log(project.value);
    });
</script>