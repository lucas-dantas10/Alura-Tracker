<template>
  <main class="columns is-gapless is-multiline" :class="{ 'modo-escuro': modoEscuroAtivo }">
    <div class="column is-one-quarter">
      <BarraLateral @ao-tema-alterado="trocarTema"></BarraLateral>
    </div>

    <div class="column is-three-quarters conteudo">
      <Notificacoes></Notificacoes>
      <router-view></router-view>
    </div>
  </main>
</template>

<script lang="ts">

import { defineComponent } from 'vue';
import BarraLateral from './components/BarraLateral.vue';
import Notificacoes from './components/Notificacoes.vue';

export default defineComponent({
  name: 'App',
  components: {
    BarraLateral,
    Notificacoes
  },

  data () {
    return {
      modoEscuroAtivo: false,
    }
  },

  methods: {

    trocarTema(modoEscuroAtivo: boolean) {
      this.modoEscuroAtivo = modoEscuroAtivo;
    }
  }
});
</script>

<style>
.lista {
  padding: 1rem;
}

main {
  --bg-primario: #ffffff;
  --texto-primario: #000000;
}

main.modo-escuro {
  --bg-primario: #2d2d42;
  --texto-primario: #dddddd;
}

.conteudo {
  background-color: var(--bg-primario);
}
</style>
