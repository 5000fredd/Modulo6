<template>
  <Menu>
    <router-link to="/">Home</router-link> 
    <router-link to="/about">About</router-link>
    <router-link to="/cliente">Clientes</router-link>
    <router-link to="/categoria">Categorias</router-link>
    <router-link to="/producto">Productos</router-link>
    <router-link to="/venta">Ventas</router-link>
  </Menu>
  <router-view/>
</template>

<script>

import Menu from './components/Menu.vue'

export default {
  name:'App',
  data(){
    return {

    }
  },
  components:{
    Menu
  }
}
</script>

<style lang="scss">

</style>
