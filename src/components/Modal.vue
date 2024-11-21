<template>
  <div v-if="isVisible" class="modal fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center">
    <div class="modal-content bg-white p-6 rounded-lg w-80">
      <h2 class="text-xl font-semibold mb-4">{{ title }}</h2>
      <div class="tree mb-4">
        <FolderTree :folders="folders" @folder-selected="onFolderSelected" />
      </div>
      <div class="flex justify-between">
        <button
            @click="handleOk"
            class="bg-green-500 text-white py-2 px-4 rounded hover:bg-green-700 transition"
        >
          Ок
        </button>
        <button
            @click="handleClose"
            class="bg-red-500 text-white py-2 px-4 rounded hover:bg-red-700 transition"
        >
          Закрыть
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import type { PropType } from 'vue';
import type { Folder } from '../types';
import FolderTree from './FolderTree.vue';

export default defineComponent({
  name: 'Modal',
  components: { FolderTree },
  props: {
    title: {
      type: String,
      required: true
    },
    folders: {
      type: Array as PropType<Folder[]>,
      required: true
    }
  },
  emits: ['close', 'select'],
  setup(props, { emit }) {
    const isVisible = ref(true);
    const selectedFolder = ref<Folder | null>(null);

    const handleClose = () => {
      emit('close');
      isVisible.value = false;
    };

    const handleOk = () => {
      emit('select', selectedFolder.value);
      handleClose();
    };

    const onFolderSelected = (folder: Folder) => {
      selectedFolder.value = folder;
    };


    return { isVisible, handleClose, handleOk, onFolderSelected };
  }
});
</script>
