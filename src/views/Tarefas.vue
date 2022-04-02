<template>
    <Formulario @aoSalvarTarefa="salvarTarefa" />
    <div class="lista">
        <div class="field">
            <p class="control has-icons-left">
                <input
                    class="input"
                    type="text"
                    placeholder="Digite para filtrar"
                    v-model="filtro"
                />
                <span class="icon is-small is-left">
                    <i class="fas fa-search"></i>
                </span>
            </p>
        </div>
        <Box v-if="listaEstaVazia"
            ><p class="corTexto">Você não está muito produtivo hoje :(</p></Box
        >
        <Tarefa
            v-for="(tarefa, index) in tarefas"
            :key="index"
            :tarefa="tarefa"
            @aoTarefaClicada="selecionarTarefa"
        />

        <Modal :mostrar="tarefaSelecionada != null">
            <header class="modal-card-head">
                <p class="modal-card-title">Editando uma tarefa</p>
                <button
                    @click="fecharModal"
                    class="delete"
                    aria-label="close"
                ></button>
            </header>
            <section class="modal-card-body">
                <div class="field">
                    <label for="descricaoDaTarefa" class="label">
                        Descrição
                    </label>
                    <input
                        type="text"
                        class="input"
                        v-model="tarefaSelecionada.descricao"
                        id="descricaoDaTarefa"
                    />
                </div>
            </section>
            <footer class="modal-card-foot">
                <button @click="alterarTarefa" class="button is-success">
                    Salvar alterações
                </button>
                <button @click="fecharModal" class="button">Cancelar</button>
            </footer>
        </Modal>
    </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref, watchEffect } from "vue";
import Formulario from "../components/Formulario.vue";
import Tarefa from "../components/Tarefa.vue";
import Box from "../components/Box.vue";
import Modal from "../components/Modal.vue";
import { useStore } from "@/store";
import {
    ALTERAR_TAREFA,
    OBTER_PROJETOS,
    OBTER_TAREFAS,
} from "@/store/tipos-acoes";
import { CADASTRAR_TAREFA } from "@/store/tipos-acoes";
import ITarefa from "@/interfaces/ITarefa";

export default defineComponent({
    name: "App",
    components: {
        Formulario,
        Tarefa,
        Box,
        Modal
    },
    data() {
        return {
            tarefaSelecionada: null as ITarefa | null,
        };
    },
    methods: {
        salvarTarefa(tarefa: ITarefa): void {
            this.store.dispatch(CADASTRAR_TAREFA, tarefa);
        },
        selecionarTarefa(tarefa: ITarefa) {
            this.tarefaSelecionada = tarefa;
        },
        fecharModal() {
            this.tarefaSelecionada = null;
        },
        alterarTarefa() {
            this.store
                .dispatch(ALTERAR_TAREFA, this.tarefaSelecionada)
                .then(() => this.fecharModal());
        },
    },
    computed: {
        listaEstaVazia(): boolean {
            return this.tarefas.length === 0;
        },
    },
    setup() {
        const store = useStore();
        store.dispatch(OBTER_TAREFAS);
        store.dispatch(OBTER_PROJETOS);

        const filtro = ref("");

        watchEffect(() => {
            store.dispatch(OBTER_TAREFAS, filtro.value);
        });

        return {
            tarefas: computed(() => store.state.tarefa.tarefas),
            store,
            filtro,
        };
    },
});
</script>
