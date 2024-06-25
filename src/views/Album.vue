<template>
  <div class="q-pa-md">
    <q-page padding>
      <q-card class="q-ma-md">
        <q-card-section>
          <h2 class="text-h4">Albums</h2>
        </q-card-section>
        <q-card-section v-if="loading" class="text-center">
          <q-spinner color="teal" />
          <p>Loading...</p>
        </q-card-section>
        <q-card-section v-else>
          <q-list bordered class="album-list">
            <q-item
              v-for="album in albums"
              :key="album.id"
              clickable
              v-ripple
              @click="viewAlbum(album.id)"
              class="album-item"
            >
              <q-item-section>
                <q-item-label class="text-h6">{{ album.title }}</q-item-label>
              </q-item-section>
            </q-item>
          </q-list>
        </q-card-section>
      </q-card>
    </q-page>
  </div>
</template>
<script>
import { useAlbumStore } from '../store/albumStore';
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

export default {
  name: 'Album',
  setup() {
    const albumStore = useAlbumStore();
    const router = useRouter();

    const albums = ref([]);
    const loading = ref(false);

    onMounted(() => {
      loading.value = true;
      albumStore.fetchAlbums().then(() => {
        loading.value = false;
        albums.value = albumStore.albums;
      });
    });

    const viewAlbum = (albumId) => {
      router.push(`/albums/${albumId}`);
    };

    return {
      albums,
      loading,
      viewAlbum,
    };
  },
};
</script>
<style scoped>
h2 {
  margin-bottom: 20px;
}

.album-list {
  max-width: 600px;
  margin: auto;
}

.album-item {
  margin-bottom: 10px;
  border-radius: 5px;
  background-color: #f9f9f9;
  transition: background-color 0.3s;
}

.album-item:hover {
  background-color: #f1f1f1;
}

.text-h4 {
  font-size: 1.5em;
  margin-bottom: 20px;
}

.text-h6 {
  font-size: 1.2em;
  color: #333;
}

.text-center {
  text-align: center;
}

.q-spinner {
  margin-bottom: 10px;
}
</style>
