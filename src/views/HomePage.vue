<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Photo App</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <CameraComponent @photoCaptured="handlePhotoCaptured" />

      <PhotoGalleryComponent
        :photos="photos"
        @delete="deletePhotos"
      />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from "vue";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from "@ionic/vue";

import CameraComponent from "@/components/CameraComponent.vue";
import PhotoGalleryComponent from "@/components/PhotoGalleryComponent.vue";

const photos = ref<string[]>([]);

const handlePhotoCaptured = (photoPath: string) => {
  photos.value.unshift(photoPath);
};

const deletePhotos = (indexes: number[]) => {
  photos.value = photos.value.filter(
    (_, index) => !indexes.includes(index)
  );
};
</script>