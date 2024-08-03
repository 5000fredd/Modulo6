<template>
    <div>
      <h1>Editar Venta</h1>
      <form @submit.prevent="submitForm" v-if="form">
        <div class="form-group">
          <label for="cliente">Cliente:</label>
          <select id="cliente" v-model="form.clienteId" :class="{ 'is-invalid': errors.clienteId }">
            <option :value="cliente.id" v-for="(cliente, index) in clienteList" :key="`cliente-${index}`">{{ cliente.nombre }}
            </option>
          </select>
          <div v-if="errors.clienteId" class="invalid-feedback">{{ errors.clienteId }}</div>
        </div>

        <div class="form-group">
          <label for="producto">Producto:</label>
          <select id="producto" v-model="form.productoId" :class="{ 'is-invalid': errors.productoId }">
            <option :value="producto.id" v-for="(producto, index) in productoList" :key="`producto-${index}`">{{ producto.nombre }}
            </option>
          </select>
          <div v-if="errors.productoId" class="invalid-feedback">{{ errors.productoId }}</div>
        </div>
  
        <div class="form-group">
            <label for="codigo">Codigo:</label>
            <input type="text" id="codigo" v-model="form.codigo" :class="{ 'is-invalid': errors.codigo }"
              placeholder="Ingrese la codigo" />
            <div v-if="errors.codigo" class="invalid-feedback">{{ errors.codigo }}</div>
          </div>
  
        <button type="submit" class="btn btn-primary">Registrar</button>
      </form>
    </div>
  </template>
    
  <script>
  import { mapState, mapGetters, mapActions } from 'vuex'
  export default {
    name: 'VentaEdit',
    data() {
      return {
        clienteList: [],
        productoList: [],
        errors: {}
      };
    },
    methods: {
      ...mapActions(['increment']),
      validateForm() {
        this.errors = {};
  
        if (!this.form.codigo) {
          this.errors.codigo = 'El codigo es obligatorio.';
        }
    
          if (!this.form.clienteId) {
            this.errors.clienteId = 'El cliente es obligatorio.';
          }

          if (!this.form.productoId) {
            this.errors.productoId = 'EL producto es obligatorio.';
          }
  
        return Object.keys(this.errors).length === 0;
      },
  
      submitForm() {
        if (this.validateForm()) {
          // Enviar los datos al servidor
          this.save();
          // Reiniciar el formulario
          this.form = {
            codigo: '',
            clienteId: null,
            productoId: null
          };
        }
      },
      save() {
        const vm = this;
        this.axios.patch(this.baseUrl + "/ventas/" + this.item.id, this.form)
          .then(function (response) {
            if (response.status == '200') {
              vm.$emit('on-update', response.data);
            }
            vm.itemList = response.data;
          })
          .catch(function (error) {
            console.error(error);
          });
      },
      getClienteList() {
        const vm = this;
        this.axios.get(this.baseUrl + "/clientes")
          .then(function (response) {
            vm.clienteList = response.data;
          })
          .catch(function (error) {
            console.error(error);
          });
      },
      getProductoList() {
        const vm = this;
        this.axios.get(this.baseUrl + "/productos")
          .then(function (response) {
            vm.productoList = response.data;
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
      },
      form() {
        return Object.assign({},this.item);
      }
    },
    mounted() {
      this.getClienteList();
      this.getProductoList();
    },
    props: ['item']
  }
  </script>
    
  <style scoped></style>
    