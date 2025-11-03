<template>
    <div v-if="facturationExports_pageCharge">
        <document-dasboard-export-list>
          
        </document-dasboard-export-list>
        <documents-tableau-export
        @documents-tableau-export-download-emise ="facturationExport_telecharger"
        ></documents-tableau-export>
    </div>
</template>

<script setup>
    /***** Les imports vue *****/
    import {  inject, onMounted,  ref } from 'vue'
    /***** Les imports composant *****/
    import DocumentDasboardExportList from './commun/DocumentDasboardExportList.vue'
    import DocumentsTableauExport from './commun/DocumentsTableauExport.vue'
    /***** Les imports store *****/
    import { useFacturationDocumentsStore } from '../../stores/facturation/store-facturation-documents'
    import { useLoaderStore } from '../../stores/commun/store-loader'

    /**************************/
    /**** Les injections ******/
    /**************************/
    const $mcNotify = inject('McNotify')

    /**************************/
    /**** Les stores **********/
    /**************************/
    const storeLoader = useLoaderStore()
    const storeFD = useFacturationDocumentsStore()

    const facturationExport_telecharger = async (index) => {
        try {
            await storeFD.action__facturationDocuments_telechargerArchive(index?.archive)
        } catch (error) {
            $mcNotify.error(error.message)
        }
    }

     /**
     * Recuperer la liste des exports de documents
     * 
     * @param {Boolean} estLoaderAffiche Si on affiche le loader ou non
     */
    const facturationExports_recupererDocumentsExports = async (estLoaderAffiche = true) => {
        try {
            if (estLoaderAffiche) {
                storeLoader.action_launchLoader()
            }

            await storeFD.action__facturationDocuments_recupererListeExport(true)
            
            if (estLoaderAffiche) {
                storeLoader.action_stopLoader()
            }

        } catch (error) {
            if (estLoaderAffiche) {
                storeLoader.action_stopLoader()
            }
            $mcNotify.error(error.message)
        }
    }


   

    /***************************************/
    /**** OPERATIONS DE DOCUMENTS **********/
    /***************************************/
    const facturationExports_pageCharge = ref(false)

    
    /**
     * Charger tous les elements qui sont necessaires pour le composant
     */
     const facturationExports_preparer = async () => {
        try {
            storeLoader.action_launchLoader()
            await facturationExports_recupererDocumentsExports(false)
            storeLoader.action_stopLoader()

            
        } catch (error) {
            storeLoader.action_stopLoader()
            // certains appels d'API peuvent retourner une erreur sans message (ex: recuperer comptes generaux)
            if (error.message) {
                $mcNotify.error(error.message)
            } else {
                $mcNotify.error('Une erreur est survenue lors du chargement des données. Veuillez réessayer.')
            }
        }
    }

    /***************************************/
    /**** CYCLE DE VIE DU COMPOSANT ********/
    /***************************************/
    onMounted(async () => {
        await facturationExports_preparer()
        facturationExports_pageCharge.value = true
    }) 
</script>
