<template>
    <div class="box formulario">
        <div class="columns">
            <div class="column is-5" role="form" aria-label="Formulario para criação de uma nova tarefa">
                <input type="text" class="input" placeholder="Qual tarefa você deseja iniciar" v-model="descricao">
            </div>

            <div class="column is-3">
                <div class="select">
                    <select v-model="idProjeto">
                        <option value="">Selecione o projeto</option>
                        <option :value="projeto.id" v-for="projeto in projetos" :key="projeto.id">
                            {{ projeto.nome }}
                        </option>
                    </select>
                </div>
            </div>

            <div class="column">
                <Temporizador @ao-temporizador-finalizado="finalizarTarefa"></Temporizador>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { TipoNotificacao } from '@/interfaces/INotificacao';
import { key } from '@/store';
import { NOTIFICAR } from '@/store/tipo-mutacoes';
import { computed, defineComponent } from 'vue';
import { useStore } from 'vuex';
import Temporizador from './Temporizador.vue';

export default defineComponent({
    name: 'Formulario',

    emits: ['aoSalvarTarefa'],

    data() {
        return {
            descricao: '',
            idProjeto: '',
        }
    },

    components: {
        Temporizador
    },

    methods: {
        finalizarTarefa(tempoDecorrido: number): void {
            const projeto = this.projetos.find((proj) => proj.id == this.idProjeto)

            if (!projeto) {
                this.store.commit(NOTIFICAR, {
                    titulo: 'Ops!',
                    texto: 'Selecione um projeto antes de finalizar uma tarefa.',
                    tipo: TipoNotificacao.FALHA
                });

                return;
            }

            if (this.descricao == '') {
                this.store.commit(NOTIFICAR, {
                    titulo: 'Nome da tarefa vazio ;-;',
                    texto: 'Adicionar nome da tarefa.',
                    tipo: TipoNotificacao.ATENCAO
                });

                return;
            }

            this.store.commit(NOTIFICAR, {
                titulo: 'Show',
                texto: 'Tarefa adicionada com sucesso ;)',
                tipo: TipoNotificacao.SUCESSO
            });
        
            this.$emit('aoSalvarTarefa', {
                duracaoEmSegundos: tempoDecorrido,
                descricao: this.descricao,
                projeto: this.projetos.find(proj => proj.id == this.idProjeto)
            });
            this.descricao = '';
        },
    },

    setup() {
        const store = useStore(key);

        return {
            projetos: computed(() => store.state.projetos),
            store
        }
    }
});
</script>

<style>
.formulario {
    color: var(--texto-primario);
    background-color: var(--bg-primario);
}
</style>