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
                <button class="button" type="submit">
                    Salvar
                </button>
            </div>
        </form>

    </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { useStore } from "@/store";
import { ALTERA_PROJETO, ADICIONA_PROJETO, NOTIFICAR } from "@/store/tipo-mutacoes";
import { TipoNotificacao } from "@/interfaces/INotificacao";

export default defineComponent({
    name: "Formulario",
    props: {
        id: {
            type: String
        }
    },
    mounted() {
        if(this.id) {
            const projeto = this.store.state.projetos.find(proj => proj.id == this.id)
            this.nomeDoProjeto = projeto?.nome || ''
        }
    },
    data() {
        return {
            nomeDoProjeto: '',
        }
    },
    methods: {
        salvar() {
            if (this.id) {
                this.store.commit(ALTERA_PROJETO, {
                    id: this.id,
                    nome: this.nomeDoProjeto
                })
            } else {
                this.store.commit(ADICIONA_PROJETO, this.nomeDoProjeto)
            }
            this.nomeDoProjeto = '';
            this.notificar(TipoNotificacao.SUCESSO, 'Show', 'Foi')
            this.$router.push('/projetos')
        },
        notificar(tipo: TipoNotificacao, titulo: string, texto: string) {
            this.store.commit(NOTIFICAR, {
                titulo,
                texto,
                tipo
            })
        }
    },
    setup() {
        const store = useStore()
        return {
            store
        }
    }
});
</script>

<style scoped>

.title, .label{
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
