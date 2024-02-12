<template>
  <ion-header>
    <ion-toolbar>
      <ion-title>SISALI</ion-title>
    </ion-toolbar>
  </ion-header>
  <ion-content class="ion-padding">
    <h1 style="text-align: center">Hospital General de Chone</h1>

    <h3 style="text-align: center">Confirmación Alimentación</h3>

    <ion-datetime-button datetime="datetime"></ion-datetime-button>
    <ion-modal :keep-contents-mounted="true">
      <ion-datetime id="datetime"></ion-datetime>
    </ion-modal>

    <ion-list style="margin-top: 20px; margin-bottom: 50px">
      <ion-item> <ion-label>Almuerzo</ion-label>12:30 -- 14:00 </ion-item>
      <ion-item> <ion-label>Merienda</ion-label>17:30 -- 18:30 </ion-item>
      <ion-item> <ion-label>Cena</ion-label>20:00 -- 21:00 </ion-item>
    </ion-list>

    <ion-item style="margin-top: 50px; margin-bottom: 50px">
      <ion-input
        label="Ingresa tu número de cedula"
        label-placement="stacked"
        ref="input"
        type="text"
        placeholder=""
      ></ion-input>

      <ion-input
        label="Ingresa tu número de teléfono o pin"
        label-placement="stacked"
        ref="input"
        type="text"
        placeholder=""
      ></ion-input>
    </ion-item>

    <ion-button id="open-modal" expand="block">Open</ion-button>
  </ion-content>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonTitle,
  IonToolbar,
  IonItem,
  IonLabel,
  IonList,
  IonDatetime,
  IonDatetimeButton,
  IonModal,
  IonButtons,
  IonButton,
  IonPage,
} from "@ionic/vue";
import { ref } from "vue";
import { defineComponent } from "vue";
const page = ref(null);
const modal = ref(null);

export default defineComponent({
  components: {
    IonContent,
    IonHeader,
    IonTitle,
    IonToolbar,
    IonItem,
    IonLabel,
    IonList,
    IonDatetime,
    IonDatetimeButton,
    IonModal,
  },

  methods: {
    dismiss() {
      alert("sdsd");
    },
    canDismiss() {
      alert("sdsd");
    },
  },
});
</script>
