<template>
  <Formulario @aoSalvarTarefa="salvarTarefa" />
  <div class="lista">
    <Tarefa v-for="(tarefa, index) in tarefas" :key="index" :tarefa="tarefa" />
    <Box v-if="listaEstaVazia"
      ><p class="corTexto">Você não está muito produtivo hoje :(</p></Box
    >
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from "vue";
import Formulario from "../components/Formulario.vue";
import Tarefa from "../components/Tarefa.vue";
import Box from "../components/Box.vue";
import { useStore } from "@/store";
import {  OBTER_TAREFAS } from "@/store/tipos-acoes";

export default defineComponent({
  name: "App",
  components: {
    Formulario,
    Tarefa,
    Box,
  },
  computed: {
    listaEstaVazia(): boolean {
      return this.tarefas.length === 0;
    },
  },
    setup() {
        const store = useStore()
        store.dispatch(OBTER_TAREFAS)

        return {
            tarefas: computed(() => store.state.tarefas),
            store
        };
    },
  methods: {
    //salvarTarefa(tarefa: ITarefa) {
     // this.tarefas.push(tarefa);
    //},
  }
});
</script>

