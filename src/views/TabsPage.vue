<template>
  <ion-page ref="page">
    <ion-header>
      <ion-toolbar>
        <ion-title>SISALI</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <ion-row>
        <ion-col size="12" style="text-align: center">
          <img
            src="http://107.20.106.196/dist/img/logomsp.png"
            alt="Descripción de la imagen"
            width="20%"
          />
          <h2 style="text-align: center">Hospital General de Chone</h2>
          <h3 style="text-align: center">Confirmación Alimentación</h3>
        </ion-col>
      </ion-row>

      <ion-datetime-button datetime="datetime"></ion-datetime-button>
      <ion-modal :keep-contents-mounted="true">
        <ion-datetime id="datetime"></ion-datetime>
      </ion-modal>

      <ion-list style="margin-top: 20px; margin-bottom: 30px">
        <ion-item> <ion-label>Almuerzo</ion-label>12:30 -- 14:00 </ion-item>
        <ion-item> <ion-label>Merienda</ion-label>17:30 -- 18:30 </ion-item>
        <ion-item> <ion-label>Cena</ion-label>20:00 -- 21:00 </ion-item>
      </ion-list>

     
        <ion-item style="margin-top: 10px; margin-bottom: 20px">
          <ion-input
            label="Ingresa tu número de cedula"
            label-placement="stacked"
          
            id="input_cedula"
            type="text"
            placeholder=""
          ></ion-input>

          <ion-input
            label="Ingresa tu número de teléfono o pin"
            label-placement="stacked"
            id="input_tlfn"
            type="text"
            placeholder=""
          ></ion-input>
        </ion-item>

        <!-- <ion-button id="open-modal"  expand="block">Consultar</ion-button> -->
        <ion-button @click="consultaAlimento()"  expand="block">Consultar</ion-button>
     

      <ion-loading trigger="open-loading" message="Loading..." duration="3000" spinner="circles"></ion-loading>

      <ion-modal
        ref="modal"
        :is-open="isOpen"
      
      >
        <ion-header>
          <ion-toolbar>
            <ion-title>Detalle</ion-title>
            <ion-buttons slot="end">
              <!-- <ion-button @click="dismiss">Cerrar</ion-button> -->
            </ion-buttons>
          </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
          <ion-item style="margin-top: 30px">
            <ion-label> <b>Cedula:</b> <span> {{ datofuncCed }}  </span></ion-label>
          </ion-item>

          <ion-item>
            <ion-label> <b>Nombres:</b> <span> {{ datofunc }} </span></ion-label>
          </ion-item>

          <ion-item>
            <ion-label> <b>Turno:</b> <span> {{ datofuncTurno }} </span></ion-label>
          </ion-item>

          <ion-item> </ion-item>

          <ion-list
            style="margin-top: :150px !important; margin-bottom: :50px;"
          >
            <ion-item style="margin-bottom: 10px"  v-for="item in productoData" :key="item.idal_menu_comida" id="body">
              <ion-checkbox justify="space-between" v-model="item.chequear" :disabled="esCheckboxDesactivado(item)">{{item.comida}}</ion-checkbox>
            </ion-item>

            <!-- <ion-item>
              <ion-checkbox justify="space-between">Merienda</ion-checkbox>
            </ion-item>

            <ion-item style="margin-bottom: 50px">
              <ion-checkbox justify="space-between">Cena</ion-checkbox>
            </ion-item> -->
          </ion-list>

          <ion-content class="ion-text-center ion-padding">
            <ion-row>
              <ion-col size="12">
                <ion-button color="primary" :disabled="esBotonDesactivado(1)" size="default" id="present-alert"
                  >Confirmar</ion-button>

                  <ion-alert
                      trigger="present-alert"
                      header="¿Estas seguro de confirmar los alimentos seleccionados?"
                      :buttons="alertButtons"
                      @didDismiss="logResult($event)"
                    ></ion-alert>
                <ion-button color="danger" @click="dismiss" size="default"
                  >Cancelar</ion-button
                >

                <!-- <ion-loading class="custom-loading" trigger="open-loading" message="Loading..." duration="3000"></ion-loading> -->
              </ion-col>
            </ion-row>
          </ion-content>
        </ion-content>
      </ion-modal>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonButtons,
  IonButton,
  IonModal,
  IonHeader,
  IonContent,
  IonToolbar,
  IonTitle,
  IonPage,
  IonCheckbox,
  IonItem,
  IonList,
  IonLabel,
  IonAlert,
  toastController,
  IonLoading, 
  modalController,
  loadingController 
} from "@ionic/vue";
import { ref } from "vue";
import $ from 'jquery';
import axios from 'axios';


const page = ref(null);
const modal = ref(null);
const mostrarModal= false
const isOpen = ref(false);
const datofunc= ref(null);
const datofuncCed= ref(null);
const datofuncTurno= ref(null);
const productoData = ref(null);
const isChecked=false
const alertButtons = [
    {
      text: 'Cancel',
      role: 'cancel',
      handler: () => {
        console.log('Alert canceled');
      },
    },
    {
      text: 'OK',
      role: 'confirm',
      handler: () => {
        console.log('Alert confirmed');
        // presentToast()
        showLoading()
      },
    },
  ];

  const logResult = (ev: CustomEvent) => {
    console.log(`Dismissed with role: ${ev.detail.role}`);
  };

function dismiss() {
  modal.value.$el.dismiss();
  
}

async function canDismiss(data?: any, role?: string) {
  return role !== "gesture";
  
}

async function presentToast() {
  const toast = await toastController.create({
    message: 'Alimentos confirmados exitosamente!',
    duration: 5500,
    color:'success',
    position: 'top',
    // translucent: true,
  });

  await toast.present();
}

async function consultaAlimento(){
  const cedula=$('#input_cedula').val()
  const pin_tlf=$('#input_tlfn').val()

  

  if(cedula==""){
    alert("Ingrese la cedula")
    return
  }

  if(cedula.length != 10){
    alert("El numero de cedula debe tener 10 digitos")
    return
  }

  if(pin_tlf==""){
    alert("Ingrese el pin o telefono")
    return
  }

  const datos = {
    cedula_func:cedula,
    telef_pin:pin_tlf
        // ... otros datos
      };

      const config = {
        headers: {
          'Content-Type': 'application/json', 
        }
      };
      
      const loading = await loadingController.create({
        message: 'Espere por favor...',
        // duration: 3000,
      });

    loading.present();

    this.isOpen = false 
  // Realizar una solicitud POST
  axios.post('http://107.20.106.196/api/consulta-comida-empleado-api', datos, config)  
    .then(response => {
      console.log(response)
      loading.dismiss();
      if (response.data.error==true){
        alert(response.data.mensaje)
      }else{
        // Abrir el modal después de que la solicitud se completa
        // this.mostrarModal = true;
        this.isOpen = true  

        $('#cedula_func').val(response.data.data.cedula)
        this.datofuncCed=response.data.data.cedula
        this.datofunc=response.data.data.nombres
        this.datofuncTurno=response.data.data.hora_ini+" -- "+response.data.data.hora_fin
        this.productoData=response.data.detalleMenu.Menu 

       

// También puedes usar la referencia para abrir el modal
// this.$refs.miModal.open();
      }
    })
    .catch(error => {
      loading.dismiss();
      // Manejar errores
      console.error('Error en la solicitud:', error);
    });
  }

  function esCheckboxDesactivado(item) {
      let compro=0;
      if(item.chequear === true){
        // esBotonDesactivado(1)
        compro=compro+1;
      } 
      if(compro>0){
        esBotonDesactivado(1)
      }else{
        esBotonDesactivado(0)
      }    
      return item.chequear === true;
      
  }

  function esBotonDesactivado(valor){
    console.log(valor)
    return valor === 1
  }
async function mostrarLoading() {
  const loading = await modalController.create({
    component: IonLoading,
    message: 'Cargando...', // Mensaje que se mostrará
    duration: 6000, // Duración del indicador en milisegundos (puedes ajustarlo)
  });

  await loading.present();

  // Aquí puedes realizar operaciones asíncronas, por ejemplo, cargar datos de una API

  // Cuando las operaciones asíncronas han terminado, ocultamos el indicador de carga
  await loading.dismiss();
}

const showLoading = async () => {
  const loading = await loadingController.create({
    message: 'Espere por favor...',
    duration: 3000,
  });

  loading.present();

  presentToast()
}

  

</script>

<style>
  ion-loading.custom-loading {
    --background: #e3edff;
    --spinner-color: #1c6dff;

    color: #1c6dff;
  }
</style>
