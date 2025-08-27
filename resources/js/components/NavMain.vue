<script setup lang="ts">
import { SidebarGroup, SidebarGroupLabel, SidebarMenu, SidebarMenuButton, SidebarMenuItem } from '@/components/ui/sidebar';
import { type NavItem } from '@/types';
import { Link, usePage } from '@inertiajs/vue3';
import { ChevronDown, ChevronRight } from 'lucide-vue-next';
import { ref } from 'vue';

defineProps<{
    items: NavItem[];
}>();

const page = usePage();

// Track open dropdowns
const openMenus = ref<Record<string, boolean>>({});
const toggleMenu = (title: string) => {
    openMenus.value[title] = !openMenus.value[title];
};
</script>

<template>
    <SidebarGroup class="px-2 py-0">
        <SidebarGroupLabel>Platform</SidebarGroupLabel>
        <SidebarMenu>
            <template v-for="item in items" :key="item.title">
                <!-- Dropdown parent -->
                <SidebarMenuItem v-if="item.children">
                    <SidebarMenuButton
                        as-child
                        :tooltip="item.title"
                        :is-active="item.children.some((child) => child.href === page.url)"
                        @click="toggleMenu(item.title)"
                    >
                        <div
                            :class="[
                                'flex w-full cursor-pointer items-center justify-between rounded-md px-3 py-2',
                                item.children.some((child) => child.href === page.url) ? 'bg-gray-900 text-white' : 'hover:bg-gray-700',
                            ]"
                        >
                            <div class="flex items-center gap-2">
                                <component v-if="item.icon" :is="item.icon" class="h-4 w-4" />
                                <span>{{ item.title }}</span>
                            </div>
                            <component :is="openMenus[item.title] ? ChevronDown : ChevronRight" class="h-4 w-4" />
                        </div>
                    </SidebarMenuButton>

                    <!-- Animated dropdown children -->
                    <transition
                        name="dropdown"
                        enter-active-class="transition-all duration-300 ease-out"
                        leave-active-class="transition-all duration-200 ease-in"
                    >
                        <div v-show="openMenus[item.title]" class="mt-1 ml-6 space-y-1 overflow-hidden">
                            <SidebarMenuItem v-for="child in item.children" :key="child.title">
                                <SidebarMenuButton as-child :tooltip="child.title" :is-active="child.href === page.url">
                                    <Link
                                        :href="child.href"
                                        :class="[
                                            'flex items-center gap-2 rounded-md px-3 py-2',
                                            child.href === page.url ? 'bg-gray-900 text-white' : 'hover:bg-gray-700',
                                        ]"
                                    >
                                        <component v-if="child.icon" :is="child.icon" class="h-4 w-4" />
                                        <span>{{ child.title }}</span>
                                    </Link>
                                </SidebarMenuButton>
                            </SidebarMenuItem>
                        </div>
                    </transition>
                </SidebarMenuItem>

                <!-- Normal single item -->
                <SidebarMenuItem v-else>
                    <SidebarMenuButton as-child :tooltip="item.title" :is-active="item.href === page.url">
                        <Link
                            :href="item.href"
                            :class="[
                                'flex items-center gap-2 rounded-md px-3 py-2',
                                item.href === page.url ? 'bg-gray-900 text-white' : 'hover:bg-gray-700',
                            ]"
                        >
                            <component v-if="item.icon" :is="item.icon" class="h-4 w-4" />
                            <span>{{ item.title }}</span>
                        </Link>
                    </SidebarMenuButton>
                </SidebarMenuItem>
            </template>
        </SidebarMenu>
    </SidebarGroup>
</template>

<style scoped>
/* Smooth slide-down/up animation for dropdown */
.dropdown-enter-from,
.dropdown-leave-to {
    max-height: 0;
    opacity: 0;
}
.dropdown-enter-to,
.dropdown-leave-from {
    max-height: 500px; /* adjust as needed */
    opacity: 1;
}
.dropdown-enter-active,
.dropdown-leave-active {
    transition: all 0.3s ease;
}
</style>
