<template>
    <div class="documents_actions_document">

        <div class="documents_actions_document_wrapper ">
            <!-- Documents à fusionner -->
            <div class="documents_actions_document_wrapper_grid_item">
                <mc-label label-text="Documents à fusionner :"></mc-label>
                <span test-id="test_daddfu"
                    >
                    {{  documentsActionDocumentDonnee?.merge ? 'Oui': 'Non' }}
                </span>
            </div>
        
            <!-- Documents à envoyé -->
            <div class="documents_actions_document_wrapper_grid_item">
                <mc-label label-text="Documents à envoyé : "></mc-label>
                <span test-id="test_dadden"
                    >
                {{  documentsActionDocumentDonnee?.send ? 'Oui': 'Non' }}
                </span>
            </div>

            <!-- Documents à envoyé en copie -->
            <div class="documents_actions_document_wrapper_grid_item">
                <mc-label label-text="Documents à envoyé en copie :"></mc-label>
                <span test-id="test_daddcc"
                    >
                    {{  documentsActionDocumentDonnee?.cc ? 'Oui': 'Non' }}
                </span>
            </div>
        </div>
    </div>
</template>

<script setup>

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
         
        documentsActionDocumentDonnee: {
            type: Object,
            required: true
        }
    })
</script>

<style lang="scss" scoped>
    .documents_actions_document {
        &_wrapper {
            display: grid;
            grid-template-columns: 1fr; 
            row-gap: rem(10); 
            &_grid_item {
                display: grid;
                grid-template-columns: auto 1fr;
                align-items: center;
                column-gap: rem(20);
                span {
                    justify-self: end;
                }              

            }
        }        
    }
</style>
