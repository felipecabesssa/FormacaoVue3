<template>
    <section class="projetos">
        <h1 class="title">Projetos</h1>
        <router-link to="/projetos/novo" class="button">
            <span class="icon is-small">
                <i class="fas fa-plus"></i>
            </span>
            <span>Novo projeto</span>
        </router-link>
        <table class="table is-fullwidth mt-4">
            <thead>
                <tr>
                    <th class="tituloTabela">ID</th>
                    <th class="tituloTabela">Nome</th>
                    <th class="tituloTabela">Ações</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="projeto in projetos" :key="projeto.id">
                    <td>{{ projeto.id }}</td>
                    <td>{{ projeto.nome }}</td>
                    <td>
                        <router-link
                            :to="`/projetos/${projeto.id}`"
                            class="button"
                        >
                            <span class="icon is-small">
                                <i class="fas fa-pencil-alt"></i>
                            </span>
                        </router-link>
                    </td>
                </tr>
            </tbody>
        </table>
    </section>
</template>

<script lang="ts">
import { computed, defineComponent } from "vue";
import { useStore } from "@/store";

export default defineComponent({
    name: "Projetos",
    setup() {
        const store = useStore();
        return {
            projetos: computed(() => store.state.projetos),
        };
    },
});
</script>

<style scoped>
.projetos {
    padding: 1.25rem;
}
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
