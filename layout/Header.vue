
<template>
    <div class="card">
        <Menubar :model="items">
            <template #start>
                <img src="/logobrand.png" width="42px"/>
                <span style="color: blue; font-size: 24px;">Library</span>
            </template>
            <template #item="{ item, props, hasSubmenu, root }">
                <RouterLink :to="item?.link">
                    <a v-ripple class="!flex items-center" v-bind="props.action">
                    <span :class="item.icon" />
                    <span class="ml-2">{{ item.label }}</span>
                    <Badge v-if="item.badge" :class="{ 'ml-auto': !root, 'ml-2': root }" :value="item.badge" />
                    <span v-if="item.shortcut" class="ml-auto border border-surface rounded bg-emphasis text-muted-color text-xs p-1">{{ item.shortcut }}</span>
                    <i v-if="hasSubmenu" :class="['pi pi-angle-down', { 'pi-angle-down ml-2': root, 'pi-angle-right ml-auto': !root }]"></i>
                </a>
                </RouterLink>
            </template>
            <template #end>
                <div class="flex items-center gap-2 border-spacing-1">
                    <!-- <ThemeToggle /> -->
                    <!-- <InputText placeholder="Search" type="text" class="w-32 sm:w-auto" /> -->
                    <div class="!cursor-pointer " style="cursor: pointer;" >
                        <Avatar image="/user.png" shape="circle" style="background-color: grey;padding: 4px"  />
                        
                    </div>
                </div>
            </template>
        </Menubar>
    </div>
</template>

<script setup>
import ThemeToggle from './ThemeToggle.vue';
import Menubar from 'primevue/menubar';
import { ref } from "vue";

const items = ref([
    {
        label: 'Home',
        icon: 'pi pi-home',
        link: '/'
    },
    {
        label: 'Libraries',
        icon: 'pi pi-star',
        link: '/all'
    },
    // {
    //     label: 'Projects',
    //     icon: 'pi pi-search',
    //     items: [
    //         {
    //             label: 'Core',
    //             icon: 'pi pi-bolt',
    //             shortcut: '⌘+S'
    //         },
    //         {
    //             label: 'Blocks',
    //             icon: 'pi pi-server',
    //             shortcut: '⌘+B'
    //         },
    //         {
    //             label: 'UI Kit',
    //             icon: 'pi pi-pencil',
    //             shortcut: '⌘+U'
    //         },
    //         {
    //             separator: true
    //         },
    //         {
    //             label: 'Templates',
    //             icon: 'pi pi-palette',
    //             items: [
    //                 {
    //                     label: 'Apollo',
    //                     icon: 'pi pi-palette',
    //                     badge: 2
    //                 },
    //                 {
    //                     label: 'Ultima',
    //                     icon: 'pi pi-palette',
    //                     badge: 3
    //                 }
    //             ]
    //         }
    //     ]
    // },
    // {
    //     label: 'Contact',
    //     icon: 'pi pi-envelope',
    //     badge: 3
    // }
]);
</script>
