<template>
  <ul class="space-y-2">
    <li
        v-for="folder in props.folders"
        :key="folder.id"
        :class="{'bg-blue-100': isFolderSelected(folder), 'p-2 rounded': true}"
    >
      <div class="flex items-center space-x-2">
        <input
            type="checkbox"
            :checked="isFolderSelected(folder)"
            @change="(event) => selectFolder(folder, event)"
            class="cursor-pointer"
        />

        <span @click="(event) => toggleOpen(folder)" class="font-medium cursor-pointer">
          {{ folder.name }}
        </span>

        <span v-if="folder.children && folder.children.length > 0" class="text-gray-500">
          {{ isOpen(folder) ? '[-]' : '[+]' }}
        </span>
      </div>

      <FolderTree
          v-if="isOpen(folder)"
          :folders="folder.children"
          @folder-selected="selectFolder"
      />

    </li>
  </ul>
</template>

<script setup lang="ts">
import { ref } from "vue";
import type { Folder } from '../types';

const props = defineProps<{
  folders: Folder[];
}>();

const emit = defineEmits<{
  (e: 'folder-selected', folderId: Folder): void;
}>();

const openFolders = ref<Set<number>>(new Set());
const selectedFolders = ref<Set<number>>(new Set());

const isOpen = (folder: Folder) => {
  return openFolders.value.has(folder.id);
};

const toggleOpen = (folder: Folder) => {
  if (isOpen(folder)) {
    openFolders.value.delete(folder.id);
  } else {
    openFolders.value.add(folder.id);
  }
};

const selectFolder = (folder: Folder | null, event?: Event) => {
  if (!folder) {
    console.warn("Folder is invalid", folder);
    return;
  }

  if (event) {
    event.stopPropagation();
  }

  const folderId = folder.id;

  if (selectedFolders.value.has(folderId)) {
    selectedFolders.value.delete(folderId);
  } else {
    selectedFolders.value.add(folderId);
  }

  emit('folder-selected', folder);
};



const isFolderSelected = (folder: Folder) => {
  return selectedFolders.value.has(folder.id);
};
</script>
