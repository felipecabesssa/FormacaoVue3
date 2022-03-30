<template>
  <header>
    <h1>
      <img src="../assets/logo.png" alt="Logo alura-tracker">
    </h1>
    <button class="button" @click="alterarTema">
      <i class="fa-solid fa-moon"><p>{{ textoBotao }}</p></i>
    </button>
  </header>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'BarraLateral',
  emits: ['aoTemaAlterado'],
  data(){
    return{
      modoEscuroAtivo: true
    }
  },
  computed: {
    textoBotao(){
      if (this.modoEscuroAtivo){
        return 'Modo claro'
      }
      return 'Modo escuro'
    }
  },
  methods: {
    alterarTema(){
      this.modoEscuroAtivo = !this.modoEscuroAtivo
      this.$emit('aoTemaAlterado', this.modoEscuroAtivo)
    }
  }
})
</script>

<style scoped>
header {
  padding: 1rem;
  background: #22272e;
  width: 100%;
  height: 100vh;
  text-align: center;
}
@media only screen and (max-width: 768px) {
  header {
    padding: 2.5rem;
    height: auto;
  }
}
button{
  padding: 2rem 15px;
  background-color: var(--bg-primario);
  color: var(--texto-primario);
}
button:hover{
  color: var(--texto-primario);
}
p{
  padding-top: 10px;
  font-size: 12px;
}
</style>
