<template>

  <div v-if="!loading">
    <Gallery :page="currentPage" :limit="perPage" />
    <Pagination :page-count="pageCount" :current-page="currentPage" :on-change-page="handleChangePage" />
  </div>

  <Loader v-else />

</template>

<script>
import {defineComponent, onMounted, ref, computed} from 'vue';
import Gallery from './components/Gallery';
import Pagination from './components/Pagination';
import {fetchAllPhotos} from "@/api/fetchData";
import Loader from "@/components/Loader.vue";

export default defineComponent({
  components: {
    Loader,
    Gallery,
    Pagination,
  },
  setup() {
    const currentPage = ref(1);
    const perPage = ref(15);
    const photosCount = ref(0);

    const loading = ref(true);

    const handleChangePage = (page) => {
      currentPage.value = page;
    };

    const pageCount = computed(() => {
      return Math.ceil(photosCount.value / perPage.value);
    });

    onMounted(()=>{

      fetchAllPhotos()
          .then((data) => {
            photosCount.value = data.length;
            loading.value = false;
          })
          .catch((error) => {
            console.error(error);
            loading.value = false;
          });

      // hardcode data.length
      // photosCount.value = 5000;

    })

    return {
      currentPage,
      perPage,
      photosCount,
      handleChangePage,
      pageCount,
      loading
    };
  }
});
</script>

<style>

* {
  font-family: 'Lexend', sans-serif;
}

</style>