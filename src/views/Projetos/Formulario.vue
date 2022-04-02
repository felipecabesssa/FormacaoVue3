<template>
    <section>
        <form @submit.prevent="salvar">
            <div class="field">
                <label for="nomeDoProjeto" class="label">
                    Nome do projeto
                </label>
                <input
                    type="text"
                    class="input"
                    v-model="nomeDoProjeto"
                    id="nomeDoProjeto"
                />
            </div>
            <div class="field">
                <button class="button" type="submit">Salvar</button>
            </div>
        </form>
    </section>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { useStore } from "@/store";

import { ALTERAR_PROJETO } from "@/store/tipos-acoes";
import { TipoNotificacao } from "@/interfaces/INotificacao";

import useNotificador from "@/hooks/notificador";
import { CADASTRAR_PROJETO } from "@/store/tipos-acoes";
import { useRouter } from "vue-router";

export default defineComponent({
    name: "Formulario",
    props: {
        id: {
            type: String,
        },
    },
    setup(props) {

        const router = useRouter()

        const store = useStore();
        const { notificar } = useNotificador();

        const nomeDoProjeto = ref("")

        if (props.id) {
            const projeto = store.state.projeto.projetos.find(
                (proj) => proj.id == props.id
            );
            nomeDoProjeto.value = projeto?.nome || "";
        }

        const lidarComSucesso = () => {
            nomeDoProjeto.value = "";
            notificar(TipoNotificacao.SUCESSO, "Show", "Foi");
            router.push("/projetos");
        }

        const salvar = () => {
            if (props.id) {
                store
                    .dispatch(ALTERAR_PROJETO, {
                        id: props.id,
                        nome: nomeDoProjeto.value,
                    })
                    .then(() => lidarComSucesso());
            } else {
                store
                    .dispatch(CADASTRAR_PROJETO, nomeDoProjeto.value)
                    .then(() => lidarComSucesso());
            }
        }

        return {
            nomeDoProjeto,
            salvar
        };
    },
});
</script>

<style scoped>
.title,
.label {
    color: var(--texto-primario);
}
.table {
    background-color: var(--fundo-primario);
    color: var(--texto-primario);
}
.tituloTabela {
    color: var(--texto-primario);
}
</style>
