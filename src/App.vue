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
      entreprise: {} as Entreprise,
      typeRecherche: 'entreprise'
    };
  },
  computed: {
    estCollectiviteTerritoriale(): boolean {
      return this.typeRecherche === 'collectivite';
    },
    estServicePublic(): boolean {
      return this.typeRecherche === 'service-public';
    },
    labelRecherche(): string {
      switch (this.typeRecherche) {
        case 'collectivite':
          return 'Collectivité territoriale';
        case 'service-public':
          return 'Service public';
        default:
          return 'Entreprise';
      }
    }
  },
  methods: {
    handleEntrepriseSelected(entreprise: Entreprise): void {
      this.entreprise = entreprise;
    },
    handleTypeChange(): void {
      // Réinitialise la sélection lors du changement de type
      this.entreprise = {};
    }
  }
});
</script>

<template>
  <h1 class="fr-h4">Démonstration du paquet dsfr-entreprise</h1>

  <div class="fr-container">
    <div class="fr-grid-row">
      <div class="fr-col-lg-12">

        <fieldset class="fr-fieldset">
          <legend class="fr-fieldset__legend">
            Type de recherche
          </legend>

          <div class="fr-fieldset__content">

            <div class="fr-radio-group">
              <input
                id="entreprise"
                v-model="typeRecherche"
                type="radio"
                name="type-recherche"
                value="entreprise"
                @change="handleTypeChange"
              />
              <label class="fr-label" for="entreprise">
                Entreprise
              </label>
            </div>

            <div class="fr-radio-group">
              <input
                id="collectivite"
                v-model="typeRecherche"
                type="radio"
                name="type-recherche"
                value="collectivite"
                @change="handleTypeChange"
              />
              <label class="fr-label" for="collectivite">
                Collectivité territoriale
              </label>
            </div>

            <div class="fr-radio-group">
              <input
                id="service-public"
                v-model="typeRecherche"
                type="radio"
                name="type-recherche"
                value="service-public"
                @change="handleTypeChange"
              />
              <label class="fr-label" for="service-public">
                Service public
              </label>
            </div>

          </div>
        </fieldset>

        <DsfrEntreprise
          :label="labelRecherche"
          hint="Saisissez au moins 3 caractères pour obtenir des suggestions"
          :est_collectivite_territoriale="estCollectiviteTerritoriale"
          :est_service_public="estServicePublic"
          @enterpriseSelected="handleEntrepriseSelected"
        />

        <ul
          v-if="Object.keys(entreprise).length > 0"
          class="resultat"
        >
          <li
            v-for="(value, key) in entreprise"
            :key="key"
          >
            <strong>{{ key }} :</strong> {{ value }}
          </li>
        </ul>

        <a
          href="https://github.com/edouardroger/dsfr-entreprise"
          class="fr-link"
        >
          Retour au paquet
        </a>

      </div>
    </div>
  </div>
</template>

<style scoped>
@import 'https://cdn.jsdelivr.net/npm/@gouvfr/dsfr@1.12.1/dist/dsfr.min.css';

.fr-fieldset {
  margin-bottom: 2rem;
}

.resultat {
  margin-top: 2rem;
}
</style>