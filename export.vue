<template>
  <mc-dialogbox
    :show-dialogbox="documentsVisualiserExportVisible"
    dialogbox-title="EXPORT DE DOCUMENTS"
    dialogbox-class="document_visualiser_export"
    dialogbox-width="700"
    @close-dialogbox="$emit('documentVisualiserExportFermerEmise', false)"
  >
    <mc-card>
      <!-- Infos generales -->
      <mc-accordion accordion-label="Information du document" 
      :accordion-start-open="true">
        <documents-information-document
          :documents-information-document-donnee="documentsVisualiserExportLigneDonne"
        ></documents-information-document>
      </mc-accordion>

      <mc-accordion
        accordion-label="Documents / Partenaires / Destinataires concernés"
        :accordion-start-open="false"
        >
        <!-- Documents -->
        <documents-partenaire-destinataire-select
            test-id="test_dvedsd"
            :documents-partenaire-destinataire-select-valeur="null"
            :documents-partenaire-destinataire-select-options="comp__documentsVisualiserExport_OptionsDocuments"
            :documents-partenaire-destinataire-select-label="'Documents concernés :'"
            :documents-partenaire-destinataire-select-initiale="{ label: 'Afficher les documents', value: '' }"
            :documents-partenaire-destinataire-select-formulaire-active="true"
            :documents-partenaire-destinataire-select-label-inline="false"
        ></documents-partenaire-destinataire-select>

        <!-- Partenaires -->
        <documents-partenaire-destinataire-select
            test-id="test_dvedsp"
            :documents-partenaire-destinataire-select-valeur="null"
            :documents-partenaire-destinataire-select-options="comp__documentsVisualiserExport_OptionsPartenaires"
            :documents-partenaire-destinataire-select-label="'Partenaire(s) concerné(s) :'"
            :documents-partenaire-destinataire-select-initiale="{ label: 'Afficher le(s) partenaire(s)', value: '' }"
            :documents-partenaire-destinataire-select-formulaire-active="true"
            :documents-partenaire-destinataire-select-label-inline="false"
        ></documents-partenaire-destinataire-select>

        <!-- Destinataires -->
        <documents-partenaire-destinataire-select
            test-id="test_dvedsdest"
            :documents-partenaire-destinataire-select-valeur="null"
            :documents-partenaire-destinataire-select-options="comp__documentsVisualiserExport_OptionsDestinataires"
            :documents-partenaire-destinataire-select-label="'Destinataires concernés :'"
            :documents-partenaire-destinataire-select-initiale="{ label: 'Afficher les destinataires', value: '' }"
            :documents-partenaire-destinataire-select-formulaire-active="true"
            :documents-partenaire-destinataire-select-label-inline="false"
        ></documents-partenaire-destinataire-select>
        </mc-accordion>

      <mc-accordion accordion-label="Action du document" :accordion-start-open="false">
        <documents-actions-document
          test-id="test_dvedad"
          :documents-action-document-donnee="documentsVisualiserExportLigneDonne"
        ></documents-actions-document>
      </mc-accordion>
      
      <mc-accordion accordion-label="téléchargement de l'archives .zip" 
      :accordion-start-open="false"
      v-if="documentsVisualiserExportLigneDonne?.archive"
      >
        <!-- contenu éventuel -->
        <documents-telechargement-zip
            test-id="test_dvedtz"
            :documents-telechargement-zip-donnee="documentsVisualiserExportLigneDonne"
            @documents-telechargement-zip-annuler-emise="$emit('documentVisualiserExportFermerEmise', false)">
           </documents-telechargement-zip>
      </mc-accordion>
    </mc-card>
  </mc-dialogbox>
</template>

<script setup>
    import { computed } from 'vue'
    import DocumentsInformationDocument from './DocumentsInformationDocument.vue'
    import DocumentsActionsDocument from './DocumentsActionsDocument.vue'
    import DocumentsPartenaireDestinataireSelect from './DocumentsPartenaireDestinataireSelect.vue'
    import DocumentsTelechargementZip from './DocumentsTelechargementZip.vue'

    /***** Props *****/
    const props = defineProps({
        documentsVisualiserExportVisible: {
            type: Boolean,
            default: false,
        },

        documentsVisualiserExportLigneDonne: {
            type: Object,
            required: true
        }
    })
    /***** Emits *****/
    const emit = defineEmits(['documentVisualiserExportFermerEmise'])

    /***** Helpers pour normaliser une entrée en option {label, value} *****/
    function normalizeToOption(item) {
    
        if (item && typeof item === 'object') {
            if ('label' in item && 'value' in item) return item
            const label = item.name ?? item.nom ?? item.title ?? item.label ?? JSON.stringify(item)
            const value = item.id ?? item.value ?? label
            return { label, value }
        }

        if (item === 0 || item) {
            return { label: String(item), value: item }
        }

        return null
    }

    /***** Computed  *****/

    const comp__documentsVisualiserExport_OptionsPartenaires = computed(() => {
        const partenaires = props.documentsVisualiserExportLigneDonne?.partenaires
        if (!Array.isArray(partenaires)) return []
        return partenaires
            .map(p => normalizeToOption(p))
            .filter(Boolean)
    })

    const comp__documentsVisualiserExport_OptionsDestinataires = computed(() => {
        const destinataires = props.documentsVisualiserExportLigneDonne?.destinataires
        if (!Array.isArray(destinataires)) return []
        return destinataires
            .map(d => normalizeToOption(d))
            .filter(Boolean)
    })

    const comp__documentsVisualiserExport_OptionsDocuments = computed(() => {
        const documents = props.documentsVisualiserExportLigneDonne?.documents
        if (!Array.isArray(documents)) return []
        return documents
            .map(doc => normalizeToOption(doc))
            .filter(Boolean)
    })

    
</script>

