<template>
    <ion-modal>
      <ion-card>
        <ion-card-header>
          <ion-list>
            <ion-item>
              <ion-label position="floating">Nombre</ion-label>
              <ion-input v-model="nombre"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Categoría</ion-label>
              <ion-input v-model="categoria"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Resumen</ion-label>
              <ion-input v-model="resumen"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Descripción</ion-label>
              <ion-input v-model="descripcion"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Imagen (Archivo)</ion-label>
              <ion-input v-model="imagenArchivo"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Precio</ion-label>
              <ion-input v-model="precio" type="number"></ion-input>
            </ion-item>
          </ion-list>
        </ion-card-header>
        <ion-card-content>
          <ion-button expand="full" @click="submitForm">Guardar</ion-button>
        </ion-card-content>
      </ion-card>
    </ion-modal>
</template>

<script>
import {
  IonCard,
  IonCardContent,
  IonList,
  IonItem,
  IonLabel,
  IonInput,
  IonButton,
  IonCardHeader,
  IonModal,
} from "@ionic/vue";
export default {
  components: {
    IonCard,
    IonCardContent,
    IonInput,
    IonItem,
    IonLabel,
    IonList,
    IonButton,
    IonCardHeader,
    IonModal,
  },
  data() {
    return {
      nombre: "",
      categoria: "",
      resumen: "",
      descripcion: "",
      imagenArchivo: "",
      precio: null,
    };
  },
  methods: {
    async submitForm() {
      const data = {
        nombre: this.nombre,
        categoria: this.categoria,
        resumen: this.resumen,
        descripcion: this.descripcion,
        imagenArchivo: this.imagenArchivo,
        precio: parseFloat(this.precio),
      };
      try {
        const response = await fetch("https://localhost:5001/api/Catalog/", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(data),
        });
        console.log(data);
        if (response.ok) {
          alert("Se insertó");
          
        } else {
          throw new Error("Se tuvo un error en la solicitud.");
        }
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
