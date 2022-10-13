<template>
    <button class="button" @click="clicado" :disabled="cronometroRodando">
        <span class="icon">
            <i :class="icone"></i>
        </span>
        <span>{{texto}}</span>
    </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({

    name: 'Botao',

    emits: ['clicado'],

    props: {
        cronometroRodando: Boolean,
        texto: {
            type: String,
            required: true
        },
        
        icone: {
            type: String,
            required: true
        }
    },

    methods: {
        clicado() {
            this.$emit('clicado');
        }
    }

}); 
</script>