<template>
  <div id="app" class="flex flex-col justify-center items-center min-h-screen space-y-6 bg-gray-100">
    <button
        @click="openModal"
        class="bg-blue-500 text-white py-2 px-6 rounded-lg hover:bg-blue-700 transition duration-300"
    >
      Открыть
    </button>

    <Modal
        v-if="isModalVisible"
        :title="'Выберите папку'"
        :folders="mockFolders"
        @close="isModalVisible = false"
        @select="handleFolderSelect"
    />

    <div v-if="selectedFolderId !== null" class="mt-6 p-4 bg-green-100 text-green-700 rounded-lg shadow-md">
      <p class="text-lg font-semibold">Выбрана папка с ID:</p>
      <p class="text-2xl">{{ selectedFolderId }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import Modal from './components/Modal.vue';
import type { Folder } from './types';

export default defineComponent({
  name: 'App',
  components: { Modal },
  setup() {
    const isModalVisible = ref(false);
    const selectedFolderId = ref<number | null>(null);
    const mockFolders = ref<Folder[]>([
      { id: 1, name: 'Папка 1', children: [
          { id: 2, name: 'Папка 1.1', children: [] },
          { id: 3, name: 'Папка 1.2', children: [
              { id: 4, name: 'Папка 1.2.1', children: [] }
            ]}
        ]},
      { id: 5, name: 'Папка 2', children: [] },
    ]);

    const openModal = () => {
      isModalVisible.value = true;
    };

    const closeModal = () => {
      isModalVisible.value = false;
    };

    const handleFolderSelect = (folder: Folder | null) => {
      if (folder) {
        selectedFolderId.value = folder.id;
        console.log('Выбрана папка с ID:', folder.id);
      } else {
        selectedFolderId.value = null;
        console.log('Папка не выбрана');
      }
    };


    return { isModalVisible, selectedFolderId, openModal, closeModal, handleFolderSelect, mockFolders };
  }
});
</script>

