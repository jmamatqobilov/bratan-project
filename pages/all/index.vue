<script setup>
import Layout from '~/layout/Layout.vue';
import Select from 'primevue/select';
import Card from 'primevue/card';
import Button from 'primevue/button';
import InputText from 'primevue/inputtext';
import Rating from 'primevue/rating';
import Image from 'primevue/image';
import TreeTable from 'primevue/treetable';
import Column from 'primevue/column';
import IconField from 'primevue/iconfield';
import InputIcon from 'primevue/inputicon';
import Paginator from 'primevue/paginator';
// import { useAsyncData } from 'nuxt/dist/app/composables/asyncData';
// import { useAsyncData } from 'nuxt/dist/app/composables/asyncData';
import { BASE_URL } from '../../utils';
import Sidebar from './Sidebar.vue';
import { ref } from 'vue';

const filters = ref({
    title: null,
    description: null,
    degree: null,
    type_id: null,
    category_id: null,
    lang_id: null,
    page: 1,
    rows: 10,
})

const rating = ref(5);

const { data: educationDegreeList, status: educationDegreeListStatus } = await useAsyncData(
    'education-degree-list',
    () => $fetch('enum/education-degree', { baseURL: BASE_URL })
);

const { data: subjectType, status: subjectTypeStatus } = await useAsyncData(
    'subject-type',
    () => $fetch('web/subject_type', {
        baseURL: BASE_URL, params: {
            pg: 0
        }
    })
);

const { data: category, status: categoryStatus } = await useAsyncData(
    'category',
    () => $fetch('web/category', {
        baseURL: BASE_URL, params: {
            pg: 0
        }
    })
);

const { data: categoryParents } = await useAsyncData(
    'categoryParents',
    () => $fetch('web/category', {
        baseURL: BASE_URL, params: {
            pg: 0,
            parents: 1
        }
    })
);

const { data: language, status: languageStatus } = await useAsyncData(
    'language',
    () => $fetch('web/language', {
        baseURL: BASE_URL, params: {
            pg: 0
        }
    })
);

const { data: subject, status: subjectStatus, refresh } = await useAsyncData(
    'subject',
    () => $fetch('web/subject', {
        baseURL: BASE_URL, params: {
            ...filters.value
        }
    })
);


function selectPage(event) {
    filters.value.page = event.page + 1
    filters.value.rows = event.rows;
    refresh()
}



</script>

<template>
    <Layout class="">
        <NuxtLoadingIndicator/>
        <div class="xl:mx-auto xl:w-[1400px]">
            <div class="flex flex-col md:flex-row mt-10">
    
                <div class="w-full">
                    <Card class="shadow-md border" style=" ">
                        <template #content>
                            <div class="grid grid-cols-none md:gap-10 md:grid-cols-2 my-5">
                                <div class="flex flex-col">
                                    <label for="title">Nomi</label>
                                    <div class="w-full">
                                        <IconField iconPosition="right">
                                            <InputIcon v-if="filters.title"
                                                @click="() => { filters.title = null; refresh(filters) }"
                                                class="cursor-pointer ">
                                                <i class="pi pi-times-circle" />
                                            </InputIcon>
                                            <InputText class="w-full" v-model="filters.title"
                                                @update:model-value="refresh(filters)" placeholder="Search..." />
                                        </IconField>
                                    </div>
                                </div>
                                <div class="flex flex-col">
                                    <label for="title">Tavsif</label>
                                    <div class="w-full">
                                        <IconField iconPosition="right">
                                            <InputIcon v-if="filters.description"
                                                @click="() => { filters.description = null; refresh(filters) }"
                                                class="cursor-pointer ">
                                                <i class="pi pi-times-circle" />
                                            </InputIcon>
                                            <InputText class="w-full" v-model="filters.description"
                                                @update:model-value="refresh(filters)" placeholder="Search..." />
                                        </IconField>
                                    </div>
                                </div>
                            </div>
                            <div class="grid grid-cols-none md:gap-10 md:grid-cols-2 my-5">
                                <div class="flex flex-col">
                                    <label for="title">Ilmiy daraja</label>
                                    <Select @change="() => refresh(filters)" show-clear variant="filled"
                                        v-model="filters.degree" option-value="code" :options="educationDegreeList"
                                        optionLabel="name" placeholder="Daraja" class="w-full" />
                                </div>
                                <div class="flex flex-col">
                                    <label for="title">Turi</label>
                                    <Select @change="() => refresh(filters)" show-clear variant="filled"
                                        v-model="filters.type_id" option-value="id" :options="subjectType"
                                        optionLabel="translation.name" placeholder="Turi" class="w-full" />
                                </div>
                            </div>
    
                            <div class="grid grid-cols-none md:gap-10 md:grid-cols-2 my-5">
                                <div class="flex flex-col">
                                    <label for="title">Kategoriya</label>
                                    <Select @change="() => refresh(filters)" show-clear variant="filled"
                                        v-model="filters.category_id" option-value="id" :options="category"
                                        optionLabel="translation.name" placeholder="Kategoriya" class="w-full" />
                                </div>
                                <div class="flex flex-col">
                                    <label for="title">Tili</label>
                                    <Select @change="() => refresh(filters)" show-clear variant="filled"
                                        v-model="filters.lang_id" option-value="id" :options="language" optionLabel="name"
                                        placeholder="Tili" class="w-full" />
                                </div>
                            </div>
                        </template>
                    </Card>
                </div>
            </div>
            <!-- <div>{{ filters }}</div> -->
            <div class="grid grid-cols-none xl:grid-cols-4 mt-10">
                <div class="col-span-1 md:block hidden">
                    <Sidebar @selected="refresh" :filters="filters" :category="categoryParents" />
                </div>
                <div v-if="subject?.data.length > 0" class="col-span-4 md:col-span-3  shadow-lg p-4 m-2 md:p-10 border rounded-[10px]">
                    <div class="grid grid-cols-1 gap-10">
                        <div v-for="item in subject?.data" class="p-5  shadow-xl border-slate-200 border">
                            <div class="font-bold text-slate-800 text-center px-10">{{ item?.title }}</div>
                            <div class="flex flex-col md:flex-row ">
                                <div class="md:w-1/4 flex flex-col justify-center md:block ">
                                    <div class="mb-5 md:mb-0 md:max-w-[150px] flex justify-center">
                                        <Image :src="item?.imagepath" preview />
                                    </div>
                                    <div class="hidden md:flex items-center mt-2">
                                        <a target="_blank" :href="item?.documentpath" download>
                                            <Button icon="pi pi-download" severity="success" class="mr-2" />
                                        </a>
                                        <a target="_blank" :href="item?.documentpath">
                                            <Button icon="pi pi-eye" severity="info" class="mr-2" />
                                        </a>
                                    </div>
                                </div>
                                <div class="md:w-3/4 flex flex-col">
                                    <div class="flex border-b border-slate-100 pb-1 mb-2">
                                        <div class="w-1/2 md:w-1/3 font-bold text-slate-600 ">Tavsif:
                                        </div>
                                        <div class="w-1/2 md:w-2/3 text-slate-600">{{ item?.description?.slice(0, 100) }}
                                        </div>
                                    </div>
                                    <div class="flex border-b border-slate-100 pb-1 mb-2">
                                        <div class="w-1/2 md:w-1/3 font-bold text-slate-600 ">Turi:
                                        </div>
                                        <div class="w-1/2 md:w-2/3 text-slate-600 ">{{ item?.type?.translation?.name
                                            }}
                                        </div>
                                    </div>
                                    <div class="flex border-b border-slate-100 pb-1 mb-2">
                                        <div class="w-1/2 md:w-1/3 font-bold text-slate-600 ">Daraja:
                                        </div>
                                        <div class="w-1/2 md:w-2/3 text-slate-600 ">{{ item?.degree?.name
                                            }}
                                        </div>
                                    </div>
                                    <div class="flex border-b border-slate-100 pb-1 mb-2">
                                        <div class="w-1/2 md:w-1/3 font-bold text-slate-600 ">Kategoriya:
                                        </div>
                                        <div class="w-1/2 md:w-2/3 text-slate-600 ">{{ item?.category?.translation?.name
                                            }}
                                        </div>
                                    </div>
                                    <div class="flex border-b border-slate-100 pb-1 mb-2">
                                        <div class="w-1/2 md:w-1/3 font-bold text-slate-600 ">Til:
                                        </div>
                                        <div class="w-1/2 md:w-2/3 text-slate-600 ">{{ item?.language?.name
                                            }}
                                        </div>
                                    </div>
                                    <div class="flex border-b border-slate-100 pb-1 mb-2">
                                        <div class="w-1/2 md:w-1/3 font-bold text-slate-600 ">Aftor:
                                        </div>
                                        <div class="w-1/2 md:w-2/3 text-slate-600 ">{{ item?.author?.firstname + ' '
                                                + item?.author?.lastname
                                            }}
                                        </div>
                                    </div>
                                    <div class="py-2">
                                        <Rating v-model="rating" />
                                    </div>
                                    <div>
                                        <div class="mt-5 md:hidden items-center ">
                                            <a target="_blank" :href="item?.documentpath" download>
                                                <Button icon="pi pi-download" severity="success" class="mr-2" />
                                            </a>
                                            <a target="_blank" :href="item?.documentpath">
                                                <Button icon="pi pi-eye" severity="info" class="mr-2" />
                                            </a>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <Paginator v-if="subject?.meta" @page="selectPage" :pageLinkSize="10" :rows="subject?.meta?.per_page"
                        :totalRecords="subject?.meta?.total" :rowsPerPageOptions="[2, 10, 15, 20, 30, 50]"></Paginator>
    
                </div>
            </div>
        </div>
    </Layout>
</template>