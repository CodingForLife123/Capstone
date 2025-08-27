<script setup lang="ts">
import NavFooter from '@/components/NavFooter.vue';
import NavMain from '@/components/NavMain.vue';
import NavUser from '@/components/NavUser.vue';
import { Sidebar, SidebarContent, SidebarFooter, SidebarHeader, SidebarMenu, SidebarMenuButton, SidebarMenuItem } from '@/components/ui/sidebar';
import { dashboard } from '@/routes';
import { type NavItem } from '@/types';
import { Link, usePage } from '@inertiajs/vue3';
import { LayoutGrid } from 'lucide-vue-next';
import { computed } from 'vue';
import AppLogo from './AppLogo.vue';

// get props from Inertia
const page = usePage();
const userRole = computed(() => page.props.role as string);
const userName = computed(() => page.props.name as string);

// role check helpers
const isAdmin = computed(() => userRole.value === 'admin');
const isStaff = computed(() => userRole.value === 'staff');
const isCustomer = computed(() => userRole.value === 'customer');

// role-based nav items
const adminNavItems: NavItem[] = [
    {
        title: 'Dashboard',
        href: dashboard(),
        icon: LayoutGrid,
    },
    {
        title: 'User Management',
        href: '/admin/users',
        icon: LayoutGrid,
    },
    {
        title: 'System Settings',
        href: '/admin/settings',
        icon: LayoutGrid,
    },
    {
        title: 'Reports',
        icon: LayoutGrid,
        children: [
            {
                title: 'Sales, Inventory & Reorder',
                href: '/admin/reports/sales-inventory',
                icon: LayoutGrid,
            },
            {
                title: 'Customer Payments & Balances',
                href: '/admin/reports/customer-payments',
                icon: LayoutGrid,
            },
            {
                title: 'Supplier Transactions & Returns',
                href: '/admin/reports/supplier-transactions',
                icon: LayoutGrid,
            },
        ],
    },
    {
        title: 'Dashboard',
        href: dashboard(),
        icon: LayoutGrid,
    },
];

const staffNavItems: NavItem[] = [
    {
        title: 'Dashboard',
        href: dashboard(),
        icon: LayoutGrid,
    },
    {
        title: 'Tasks',
        href: '/staff/tasks',
        icon: LayoutGrid,
    },
];

const customerNavItems: NavItem[] = [
    {
        title: 'Dashboard',
        href: dashboard(),
        icon: LayoutGrid,
    },
    {
        title: 'My Orders',
        href: '/orders',
        icon: LayoutGrid,
    },
];

const footerNavItems: NavItem[] = [];
</script>

<template>
    <Sidebar collapsible="icon" variant="inset">
        <SidebarHeader>
            <SidebarMenu>
                <SidebarMenuItem>
                    <SidebarMenuButton size="lg" as-child>
                        <Link :href="dashboard()">
                            <AppLogo />
                        </Link>
                    </SidebarMenuButton>
                </SidebarMenuItem>
            </SidebarMenu>
        </SidebarHeader>

        <SidebarContent>
            <!-- show sidebar items based on role -->
            <NavMain v-if="isAdmin" :items="adminNavItems" />
            <NavMain v-else-if="isStaff" :items="staffNavItems" />
            <NavMain v-else-if="isCustomer" :items="customerNavItems" />
        </SidebarContent>

        <SidebarFooter>
            <NavFooter :items="footerNavItems" />
            <NavUser />
        </SidebarFooter>
    </Sidebar>
    <slot />
</template>
