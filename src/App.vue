<script lang="ts">
import { defineComponent } from 'vue';
import DsfrEntreprise from 'dsfr-entreprise';

interface Entreprise {
  [key: string]: string | number | undefined;
}

export default defineComponent({
  name: 'App',
  components: {
    DsfrEntreprise
  },
  data() {
    return {
      entreprise: {} as Entreprise
    };
  },
  methods: {
    handleEntrepriseSelected(entreprise: Entreprise): void {
      this.entreprise = entreprise;
    }
  }
});
</script>

<template>
  <h1 class="fr-h4">Démonstration du paquet dsfr-entreprise</h1>
  <div class="fr-container">
    <div class="fr-grid-row">
      <div class="fr-col-lg-12">
        <DsfrEntreprise 
          label="Collectivité territoriale" 
          hint="Saisissez au moins 3 caractères pour obtenir des suggestions" 
          est_collectivite_territoriale 
          @enterpriseSelected="handleEntrepriseSelected" 
        />

        <!-- Affichage des informations de l'item sélectionné sous forme de liste -->
        <ul class="resultat" v-if="Object.keys(entreprise).length > 0">
          <li v-for="(value, key) in entreprise" :key="key">
            <strong>{{ key }}:</strong> {{ value }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
  @import 'https://cdn.jsdelivr.net/npm/@gouvfr/dsfr@1.12.1/dist/dsfr.min.css';
</style>
