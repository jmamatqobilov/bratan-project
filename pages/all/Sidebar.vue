<script setup>
import Accordion from 'primevue/accordion';
import AccordionTab from 'primevue/accordiontab';
import AccordionPanel from 'primevue/accordionpanel';
import AccordionHeader from 'primevue/accordionheader';
import AccordionContent from 'primevue/accordioncontent';
import Toggle from './Toggle.vue'
const { category, filters } = defineProps(['category', 'filters'])
const emit = defineEmits(['selected'])

function selectCategory(tab) {
    filters.category_id = tab?.id;
    emit('selected', tab?.id)
}
</script>

<template>
    <div class="border">
        <Accordion value="0">
            <AccordionPanel v-for="tab in category" :key="tab.id" :value="tab.translation?.name">
                <AccordionHeader @click="selectCategory(tab)">{{ tab.translation?.name }}</AccordionHeader>
                <AccordionContent v-if="tab.children.length > 0">

                    <Accordion value="1">
                        <AccordionPanel v-for="tab1 in tab.children" :key="tab1.id" :value="tab1.translation?.name">
                            <AccordionHeader @click="selectCategory(tab1)">
                                <template #default>
                                    {{ tab1.translation?.name }}
                                </template>
                                <template #toggleicon>
                                    <div>
                                        <div v-if="tab1.children.length > 0">
                                            <Toggle />
                                        </div>
                                    </div>
                                </template>
                            </AccordionHeader>
                            <AccordionContent v-if="tab1.children.length > 0">
                                <Accordion value="2">
                                    <AccordionPanel v-for="tab2 in tab1.children" :key="tab2.id"
                                        :value="tab2.translation?.name">
                                        <AccordionHeader @click="selectCategory(tab2)">
                                            <template #default>
                                                {{ tab2.translation?.name }}
                                            </template>
                                            <template #toggleicon>
                                                <div>
                                                    <div v-if="tab2.children.length > 0">
                                                        <Toggle />
                                                    </div>
                                                </div>
                                            </template>
                                        </AccordionHeader>

                                    </AccordionPanel>
                                </Accordion>

                            </AccordionContent>
                        </AccordionPanel>
                    </Accordion>
                </AccordionContent>
            </AccordionPanel>
        </Accordion>

        <!-- <Accordion v-for="data in category" >
            <AccordionTab :header="data?.translation?.name">
                <Accordion v-if="data?.children">
                    <AccordionTab v-for="ch1 in data?.children" :header="ch1?.translation?.name">
                        <Accordion v-if="ch1?.children">
                            <AccordionTab v-for="ch2 in ch1?.children" :header="ch2?.translation?.name">
                                <Accordion v-if="ch2?.children">
                                    <AccordionTab v-for="ch3 in ch3?.children" :header="ch3?.translation?.name">
                                        <Accordion v-if="ch3?.children">
                                            <AccordionTab v-for="ch4 in ch3?.children" :header="ch4?.translation?.name">
                                                <Accordion v-if="ch4?.children">
                                                    <AccordionTab v-for="ch5 in ch4?.children"
                                                        :header="ch5?.translation?.name">
                                                    </AccordionTab>
                                                </Accordion>
                                            </AccordionTab>
                                        </Accordion>
                                    </AccordionTab>
                                </Accordion>
                            </AccordionTab>
                        </Accordion>
                    </AccordionTab>
                </Accordion>
            </AccordionTab>
        </Accordion> -->
    </div>
</template>