<template>
  <div>
    <h1>Registrar Producto</h1>
    <form @submit.prevent="submitForm">
      <div class="form-group">
        <label for="categoria">Categoria:</label>
        <select id="categoria" v-model="form.categoriaId" :class="{ 'is-invalid': errors.categoriaId }">
          <option :value="categoria.id" v-for="(categoria, index) in categoriaList" :key="`categoria-${index}`">{{ categoria.nombre }}
          </option>
        </select>
        <div v-if="errors.categoriaId" class="invalid-feedback">{{ errors.categoriaId }}</div>
      </div>

      <div class="form-group">
        <label for="name">Nombre producto:</label>
        <input type="text" id="name" v-model="form.nombre" :class="{ 'is-invalid': errors.nombre }"
          placeholder="Ingrese el nombre" />
        <div v-if="errors.nombre" class="invalid-feedback">{{ errors.nombre }}</div>
      </div>

      <div class="form-group">
          <label for="descripcion">Descripcion:</label>
          <input type="text" id="descripcion" v-model="form.descripcion" :class="{ 'is-invalid': errors.descripcion }"
            placeholder="Ingrese la descripcion" />
          <div v-if="errors.descripcion" class="invalid-feedback">{{ errors.descripcion }}</div>
        </div>

        <div class="form-group">
          <label for="precio">Precio:</label>
          <input type="number" id="precio" v-model="form.precio" :class="{ 'is-invalid': errors.precio }"
            placeholder="Ingrese el precio" />
          <div v-if="errors.precio" class="invalid-feedback">{{ errors.precio }}</div>
        </div>

      <button type="submit" class="btn btn-primary">Registrar</button>
    </form>
  </div>
</template>
  
<script>
import { mapState, mapGetters, mapActions } from 'vuex'
export default {
  name: 'ProductoNew',
  data() {
    return {
      categoriaList: [],
      form: {
        nombre: '',
        descripcion: '',
        precio: '',
        categoriaId: null
      },
      errors: {}
    };
  },
  methods: {
    ...mapActions(['increment']),
    validateForm() {
      this.errors = {};

      if (!this.form.nombre) {
        this.errors.nombre = 'El nombre es obligatorio.';
      }

      if (!this.form.descripcion) {
          this.errors.descripcion = 'La descripcion es obligatoria.';
        }
  
        if (!this.form.precio) {
          this.errors.precio = 'El precio es obligatorio.';
        }
  
        if (!this.form.categoriaId) {
          this.errors.categoriaId = 'La categoria es obligatoria.';
        }

      return Object.keys(this.errors).length === 0;
    },

    submitForm() {
      if (this.validateForm()) {
        // Enviar los datos al servidor
        this.save();
        // Reiniciar el formulario
        this.form = {
          nombre: '',
          descripcion: '',
          precio: '',
          categoriaId: null
        };
      }
    },
    save() {
      const vm = this;
      this.axios.post(this.baseUrl + "/productos", this.form)
        .then(function (response) {
          if (response.status == '201') {
            vm.$emit('on-register', response.data);
          }
          vm.itemList = response.data;
        })
        .catch(function (error) {
          console.error(error);
        });
    },
    getCategoriaList() {
      const vm = this;
      this.axios.get(this.baseUrl + "/categorias")
        .then(function (response) {
          vm.categoriaList = response.data;
        })
        .catch(function (error) {
          console.error(error);
        });
    },
  },
  computed: {
    // propiedades computadas que dependen de otras propiedades reactivas
    ...mapState(['count']),
    ...mapGetters(['doubleCount', 'getBaseUrl']),
    baseUrl() {
      return this.getBaseUrl
    }
  },
  mounted() {
    this.getCategoriaList();
  },
}
</script>
  
<style scoped></style>
  