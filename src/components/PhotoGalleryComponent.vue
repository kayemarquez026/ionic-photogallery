<template>
  <ion-card class="gallery-card">
    <ion-card-header>
      <div class="header">
        <ion-card-title>Photo Gallery ({{ photos.length }})</ion-card-title>

        <ion-button
          v-if="photos.length"
          fill="clear"
          size="small"
          @click="toggleSelect"
        >
          {{ selecting ? "Done" : "Select" }}
        </ion-button>
      </div>

      <div v-if="selecting && photos.length" class="actions">
        <ion-button fill="clear" size="small" @click="selectAll">
          {{ selected.length === photos.length ? "Deselect All" : "Select All" }}
        </ion-button>

        <ion-button
          v-if="selected.length"
          fill="clear"
          color="danger"
          size="small"
          @click="deleteSelected"
        >
          Delete ({{ selected.length }})
        </ion-button>
      </div>
    </ion-card-header>

    <ion-card-content>
      <ion-text
        v-if="photos.length === 0"
        color="medium"
        class="ion-text-center"
      >
        <p>No photos captured yet.</p>
      </ion-text>

      <ion-grid v-else>
        <ion-row>
          <ion-col
            size="6"
            size-md="4"
            v-for="(photo, index) in photos"
            :key="index"
          >
            <div
              class="photo"
              :class="{ selected: selected.includes(index) }"
              @click="selecting ? togglePhoto(index) : null"
            >
              <ion-img :src="photo" alt="Captured photo" />

              <span class="number">{{ index + 1 }}</span>

              <span v-if="selecting" class="check">
                {{ selected.includes(index) ? "✓" : "" }}
              </span>
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
import {
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent,
  IonGrid,
  IonRow,
  IonCol,
  IonImg,
  IonText,
  IonButton,
} from "@ionic/vue";
import { ref } from "vue";

const props = defineProps<{
  photos: string[];
}>();

const emit = defineEmits<{
  delete: [indexes: number[]];
}>();

const selecting = ref(false);
const selected = ref<number[]>([]);

const toggleSelect = () => {
  selecting.value = !selecting.value;
  selected.value = [];
};

const togglePhoto = (index: number) => {
  selected.value.includes(index)
    ? selected.value = selected.value.filter(i => i !== index)
    : selected.value.push(index);
};

const selectAll = () => {
  selected.value =
    selected.value.length === props.photos.length
      ? []
      : props.photos.map((_, index) => index);
};

const deleteSelected = () => {
  emit("delete", selected.value);
  selected.value = [];
  selecting.value = false;
};
</script>

<style scoped>
.gallery-card {
  margin: 15px;
  border-radius: 22px;
  overflow: hidden;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

ion-card-title {
  font-size: 20px;
  font-weight: 600;
}

.actions {
  display: flex;
  justify-content: space-between;
}

ion-col {
  padding: 5px;
}

.photo {
  position: relative;
  border-radius: 14px;
  overflow: hidden;
}

ion-img {
  width: 100%;
  height: 120px;
  object-fit: cover;
}

.photo.selected {
  border: 3px solid #3880ff;
}

.number,
.check {
  position: absolute;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  background: rgba(0, 0, 0, 0.7);
  font-size: 12px;
}

.number {
  right: 6px;
  bottom: 6px;
}

.check {
  left: 6px;
  top: 6px;
  background: #3880ff;
  font-weight: bold;
}
</style>