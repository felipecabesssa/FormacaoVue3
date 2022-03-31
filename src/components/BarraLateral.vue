<template>
  <header>
    <h1>
      <img src="../assets/logo.png" alt="Logo alura-tracker">
    </h1>
    <button class="button" @click="alterarTema">
      <i class="fa-solid fa-moon"><p>{{ textoBotao }}</p></i>
    </button>
    <nav class="panel mt-5">
      <ul class="display">
        <li>
          <router-link to="/" class="link">
            <i class="fas fa-tasks"></i>
            Tarefas
          </router-link>
        </li>
        <li>
          <router-link to="/projetos" class="link">
            <i class="fas fa-project-diagram"></i>
            Projetos
          </router-link>
        </li>
      </ul>
    </nav>
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
.button{
  padding: 2rem 15px;
  background-color: var(--bg-primario);
  color: var(--texto-primario);
}
.button:hover{
  color: var(--texto-primario);
}
p{
  padding-top: 10px;
  font-size: 12px;
}
.panel li {
  margin: 8px 0;
}
.link {
  color: #fff;
}
.link:hover {
  color: #dbe91f
}
.link.router-link-active {
  color: #dbe91f
}
.display{
    display: flex;
    justify-content: space-around;
}
</style>
