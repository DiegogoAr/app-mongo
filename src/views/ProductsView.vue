<template>
  <ion-content>
    <ion-card>
      <ion-button @click="insert">Agregar</ion-button>
      <insert class="agregarProducto"></insert>
    </ion-card>
    <v-dialog>
      <ion-card>
        <ion-card-title>Insertar un nuevo Producto</ion-card-title>
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
              <ion-label position="floating">Imagen (Link)</ion-label>
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
    </v-dialog>
    <div
      v-for="(products, index) in grupoProductos"
      :key="index"
      class="productos"
    >
      <ion-card v-for="product in products" :key="product.id">
        <ion-img
          :src="product.imagenArchivo"
          style="width: 150px; height: 150px"
        ></ion-img>
        <ion-card-header>
          <ion-card-title>{{ product.nombre }}</ion-card-title>
          <ion-card-subtitle>{{ product.categoria }}</ion-card-subtitle>
          <ion-card-subtitle>{{ product.id }}</ion-card-subtitle>
        </ion-card-header>
        <ion-card-content>
          <p>{{ product.resumen }}</p>
          <p>{{ product.descripcion }}</p>
          <h3>{{ product.precio }}</h3>
        </ion-card-content>
        <ion-card-content>
          <ion-button color="danger" @click="eliminar(product.id)"
            >Eliminar</ion-button
          >
          <ion-button color="success" @click="dialogoActualizar(product)"
            >Actualizar</ion-button
          >
        </ion-card-content>
      </ion-card>
    </div>
    <v-dialog v-model="dialogo">
      <ion-card>
        <ion-card-title>Actualizar el Producto</ion-card-title>
        <ion-card-header>
          <ion-list>
            <ion-item>
              <ion-label position="floating">Nombre</ion-label>
              <ion-input v-model="form.nombre"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Categoría</ion-label>
              <ion-input v-model="form.categoria"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Resumen</ion-label>
              <ion-input v-model="form.resumen"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Descripción</ion-label>
              <ion-input v-model="form.descripcion"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Imagen (Link)</ion-label>
              <ion-input v-model="form.imagenArchivo"></ion-input>
            </ion-item>
            <ion-item>
              <ion-label position="floating">Precio</ion-label>
              <ion-input v-model="form.precio" type="number"></ion-input>
            </ion-item>
          </ion-list>
        </ion-card-header>
        <ion-card-content>
          <ion-button expand="full" @click="actualizarProduct(form)"
            >Actualizar</ion-button
          >
        </ion-card-content>
      </ion-card>
    </v-dialog>
  </ion-content>
</template>
<script>
import {
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent,
  IonImg,
  IonButton,
  IonModal,
  IonList,
  IonItem,
  IonLabel,
  IonInput,
  IonContent,
} from "@ionic/vue";
import Insert from "../components/InsertProducts.vue";
export default {
  components: {
    IonCard,
    IonCardContent,
    IonCardHeader,
    IonCardTitle,
    IonImg,
    IonButton,
    Insert,
    IonModal,
    IonList,
    IonItem,
    IonLabel,
    IonInput,
    IonContent,
  },
  data() {
    return {
      products: [],
      agregarProducto: false,
      id: "",
      nombre: "",
      categoria: "",
      resumen: "",
      descripcion: "",
      imagenArchivo: "",
      precio: null,
      form: {
        id: "",
        nombre: "",
        categoria: "",
        resumen: "",
        descripcion: "",
        imagenArchivo: "",
        precio: null,
      },
    };
  },
  computed: {
    grupoProductos() {
      const groupSize = 4;
      const groups = [];
      let group = [];

      for (let i = 0; i < this.products.length; i++) {
        group.push(this.products[i]);

        if ((i + 1) % groupSize === 0 || i === this.products.length - 1) {
          groups.push(group);
          group = [];
        }
      }

      return groups;
    },
  },
  mounted() {
    fetch("https://localhost:5001/api/Catalog")
      .then((response) => {
        if (response.ok) {
          return response.json();
        } else {
          throw new Error("Se tuvo un error en la solicitud.");
        }
      })
      .then((data) => {
        this.products = data;
      })
      .catch((error) => {
        console.error(error);
      });
  },
  methods: {
    async eliminar(id) {
      try {
        const response = await fetch(
          `https://localhost:5001/api/Catalog/${id}`,
          {
            method: "DELETE",
          }
        );
        if (response.ok) {
          this.products = this.products.filter((product) => product.id !== id);
        } else {
          throw new Error("No se pudo eliminar");
        }
      } catch (error) {
        console.error(error);
      }
    },
    insert() {
      this.agregarProducto = true
    },
    dialogoActualizar(products) {
      (this.form.id = products.id),
        (this.form.categoria = products.categoria),
        (this.form.descripcion = products.descripcion),
        (this.form.imagenArchivo = products.imagenArchivo),
        (this.form.nombre = products.nombre),
        (this.form.resumen = products.resumen),
        (this.form.precio = parseFloat(products.precio));
      this.dialog = true;
    },
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
          (this.nombre = ""),
            (this.categoria = ""),
            (this.resumen = ""),
            (this.descripcion = ""),
            (this.imagenArchivo = ""),
            (this.precio = null),
            window.location.reload();
        } else {
          throw new Error("Se tuvo un error en la solicitud.");
        }
      } catch (error) {
        console.error(error);
      }
    },
    async actualizarProduct(products) {
      const data = {
        nombre: products.nombre,
        categoria: products.categoria,
        resumen: products.resumen,
        descripcion: products.descripcion,
        imagenArchivo: products.imagenArchivo,
        precio: parseFloat(products.precio),
      };
      try {
        const response = await fetch(
          `https://localhost:5001/api/Catalog/${products.id}`,
          {
            method: "PUT",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify(data),
          }
        );
        console.log(data);
        if (response.ok) {
          alert("Se Actualizó");
          this.dialog = false;
          window.location.reload();
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
<style>
.productos {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  border-radius: 10px;
  flex-wrap: wrap;
  justify-content: space-between;
}
</style>
