<template>
    <section class="documents_dashboard_export_list">
        <mc-card
            card-class="documents_dashboard_export_list_card"
            card-width="max-content"
            card-max-width="700px"
        >
            <div class="documents_dashboard_export_list_card_titre" test-id="test_ddch">
                    <mc-title-heading heading-type="heading_3" 
                    :heading-text="`${documentDasboardExportListTotal} exports`"
                    ></mc-title-heading>
                                
            </div>
            <div>
                 <p class="documents_dashboard_export_list_card_titre_description" >
                        Les exports de documents concernent des téléchargements
                        <mc-link 
                            link-color="pink" 
                            link-text="des téléchargements et envois de plus de 10 documents." 
                            link-href="https://help.macompta.fr/fr/articles/136042-modeles-de-documents-a-telecharger"
                        ></mc-link>
                        Le traitement d'un export prend généralement 1 à 2 minutes, mais peut potentiellement durer d'avantage.
                        <mc-link 
                            link-color="pink" 
                            link-text="Une fois l'export traité." 
                            link-href="https://help.macompta.fr/fr/articles/124677-600-generation-de-l-ecriture-de-paie"
                        ></mc-link>
                        celui-ci 
                        <mc-link 
                            link-class="{text-decoration: none;}"
                            link-color="pink" 
                            link-text="est disponible pendant trois jours sous forme d'une archive Zip." 
                            link-href="https://help.macompta.fr/fr/articles/124845-conservation-des-donnees"
                        ></mc-link>
                    </p>  
            </div>

            <div test-id="test_ddel">
                <slot></slot>
            </div>
        </mc-card>
    </section>
    
</template>
<script setup>
    defineProps({
        documentDasboardExportListTotal: {
            type: Number,
            required: true
        }   
    })

</script>

<style lang="scss" scoped>
    .documents_dashboard_export_list {
        ::v-deep(.documents_dashboard_export_list_card) {
            border-radius: 9999px;
            background-color: #f8f8fc;
            border: 1px solid #d1d1d1;
            display: flex;
            flex-direction: column;   
            justify-content: center;  
            text-align: center;  
        }
        @include mobile {
            @include centerWithFlex(rowHorizontal, $gap: rem(40));
            flex-wrap: wrap;
            margin-bottom: rem(40);
        }
        &_card {
            &_titre {
                @include mobile {
                    @include allFlexCenter();
                    gap: rem(5);
                    border-bottom: solid rem(1) $greyMedium;
                    margin-bottom: rem(10);
                    padding-bottom: rem(5);
                    text-align: center; 
                }
                &_description {
                    display: block; 
                    margin-bottom: rem(10);
                    line-height: 1.5;
                }
            }
            
        }
        
    }
</style>
