<template>
    <div class="documents_telechargement_zip">

        <div class="documents_telechargement_zip_wrapper ">
            <div class="documents_telechargement_zip_wrapper_grid_item">
                <mc-link 
                        link-color="pink" 
                        :link-text="documentsTelechargementZipDonnee?.archive || ''" 
                        link-href=""
                        ></mc-link>
                <mc-button
                    button-style="secondary"
                    button-class="documents_telechargement_zip_wrapper_grid_item_download_button"
                    :add-button-icon-left="true"
                    button-text="Télécharger"
                    button-icon-left-name="download"
                    test-id="test_dtzt"
                    @button-click="documentsTelechargementZip_telecharger"
                ></mc-button>
            </div>
            <div class="documents_telechargement_zip_wrapper_grid_item_cancel_button">
                <mc-button
                    button-style="primary"
                    :add-button-icon-left="true"
                    button-text="Annuler"
                    button-icon-left-name="close"
                    test-id="test_dtza"
                    @button-click="$emit('DocumentsTelechargementZipAnnulerEmise')"
                ></mc-button>
            </div>
        </div>
    </div>
</template>

<script setup>
    /*******import*******/
    import { inject } from 'vue'

    import { useFacturationDocumentsStore } from '../../../stores/facturation/store-facturation-documents'
    
    /***** Props *****/
    const props = defineProps({
        /**
         * @description Model du composant
          /**
     * @type {{
     *   id: Number,
     *   uuid: String,
     *   userId: {
     *     uuid: String
     *   },
     *   extension: String,
     *   type: String,
     *   isDelete: Boolean,
     *   statut: Boolean,
     *   error: Boolean,
     *   send: Boolean,
     *   merge: Boolean,
     *   mail: Boolean,
     *   cc: Boolean,
     *   documents: String[],
     *   destinataires: (String[] ),
     *   mailContent: (String ),
     *   archive: (String ),
     *   createdAt: String,
     *   updatedAt: (String ),
     *   hash: String,
     *   partenaires: String[],
     *   userMail: String
     * }}
     */
         
        documentsTelechargementZipDonnee: {
            type: Object,
            required: true
        }
    })

    defineEmits(['DocumentsTelechargementZipAnnulerEmise'])

    /***** Store *****/
    const storeFD = useFacturationDocumentsStore()

     /***** les inject *****/  
    const $mcNotify = inject('$mcNotify')

    const documentsTelechargementZip_telecharger = async () => {
        try {
            await storeFD.action__facturationDocuments_telechargerArchive(props.documentsTelechargementZipDonnee?.archive)
        } catch (error) {
            $mcNotify.error(error.message)
        }
    }

</script>

<style lang="scss" scoped>
    .documents_telechargement_zip {
        &_wrapper {
            display: grid;
            grid-template-columns: 1fr; 
            row-gap: rem(10); 
            &_grid_item {
                display: grid;
                grid-template-columns: auto 1fr;
                align-items: center;
                column-gap: rem(20);
                &_download_button {
                    justify-self: end;
                }
                &_cancel_button {
                    margin-top: rem(10);
                    justify-self: center;
                }
                


            }
        }

        
    }
</style>
