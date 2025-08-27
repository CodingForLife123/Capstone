<script setup lang="ts">
import AppLayout from '@/layouts/AppLayout.vue';
import { dashboard } from '@/routes';
import { type BreadcrumbItem } from '@/types';
import { Head, usePage } from '@inertiajs/vue3';
import { computed } from 'vue';
// Get page props containing user role and name
const page = usePage();
const userRole = computed(() => page.props.role as string);
const userName = computed(() => page.props.name as string);

// Role check helpers
const isAdmin = computed(() => userRole.value === 'admin');
const isStaff = computed(() => userRole.value === 'staff');
const isCustomer = computed(() => userRole.value === 'customer');

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Dashboard',
        href: dashboard().url,
    },
];
</script>

<template>
    <Head title="Dashboard" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="flex h-full flex-1 flex-col gap-4 overflow-x-auto rounded-xl p-4">
            <!-- Welcome Section - All Users -->
            <div class="mb-6">
                <h1 class="text-2xl font-bold text-gray-900 dark:text-white">Welcome back, {{ userName }}!</h1>
                <p class="text-sm text-gray-600 capitalize dark:text-gray-400">{{ userRole }} Dashboard</p>
            </div>

            <!-- =============================== -->
            <!-- ADMIN ONLY SECTION -->
            <!-- =============================== -->
            <template v-if="isAdmin">
                <!-- Admin Stats Cards -->
                <div class="grid auto-rows-min gap-4 md:grid-cols-3">
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-blue-50 to-blue-100 dark:from-blue-900/20 dark:to-blue-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-blue-900 dark:text-blue-100">Total Users</h3>
                                <p class="text-3xl font-bold text-blue-600 dark:text-blue-400">1,234</p>
                            </div>
                        </div>
                    </div>
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-green-50 to-green-100 dark:from-green-900/20 dark:to-green-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-green-900 dark:text-green-100">System Health</h3>
                                <p class="text-3xl font-bold text-green-600 dark:text-green-400">98%</p>
                            </div>
                        </div>
                    </div>
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-purple-50 to-purple-100 dark:from-purple-900/20 dark:to-purple-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-purple-900 dark:text-purple-100">Revenue</h3>
                                <p class="text-3xl font-bold text-purple-600 dark:text-purple-400">$50K</p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Admin Management Panel -->
                <div class="relative min-h-[50vh] flex-1 rounded-xl border border-sidebar-border/70 md:min-h-min dark:border-sidebar-border">
                    <div class="absolute inset-0 p-6">
                        <h2 class="mb-4 text-xl font-semibold text-gray-900 dark:text-white">Admin Control Panel</h2>
                        <div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">User Management</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Manage all system users</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">System Settings</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Configure system parameters</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Analytics</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">View detailed reports</p>
                            </div>
                        </div>
                    </div>
                </div>
            </template>

            <!-- =============================== -->
            <!-- STAFF ONLY SECTION -->
            <!-- =============================== -->
            <template v-else-if="isStaff">
                <!-- Staff Stats Cards -->
                <div class="grid auto-rows-min gap-4 md:grid-cols-3">
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-orange-50 to-orange-100 dark:from-orange-900/20 dark:to-orange-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-orange-900 dark:text-orange-100">Active Tasks</h3>
                                <p class="text-3xl font-bold text-orange-600 dark:text-orange-400">24</p>
                            </div>
                        </div>
                    </div>
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-teal-50 to-teal-100 dark:from-teal-900/20 dark:to-teal-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-teal-900 dark:text-teal-100">Completed Today</h3>
                                <p class="text-3xl font-bold text-teal-600 dark:text-teal-400">8</p>
                            </div>
                        </div>
                    </div>
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-indigo-50 to-indigo-100 dark:from-indigo-900/20 dark:to-indigo-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-indigo-900 dark:text-indigo-100">Team Members</h3>
                                <p class="text-3xl font-bold text-indigo-600 dark:text-indigo-400">12</p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Staff Work Area -->
                <div class="relative min-h-[50vh] flex-1 rounded-xl border border-sidebar-border/70 md:min-h-min dark:border-sidebar-border">
                    <div class="absolute inset-0 p-6">
                        <h2 class="mb-4 text-xl font-semibold text-gray-900 dark:text-white">Staff Workspace</h2>
                        <div class="grid grid-cols-1 gap-4 md:grid-cols-2">
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">My Tasks</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">View and manage assigned tasks</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Customer Support</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Handle customer inquiries</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Reports</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Generate work reports</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Schedule</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">View work schedule</p>
                            </div>
                        </div>
                    </div>
                </div>
            </template>

            <!-- =============================== -->
            <!-- CUSTOMER ONLY SECTION -->
            <!-- =============================== -->
            <template v-else-if="isCustomer">
                <!-- Customer Stats Cards -->
                <div class="grid auto-rows-min gap-4 md:grid-cols-3">
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-pink-50 to-pink-100 dark:from-pink-900/20 dark:to-pink-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-pink-900 dark:text-pink-100">My Orders</h3>
                                <p class="text-3xl font-bold text-pink-600 dark:text-pink-400">5</p>
                            </div>
                        </div>
                    </div>
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-cyan-50 to-cyan-100 dark:from-cyan-900/20 dark:to-cyan-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-cyan-900 dark:text-cyan-100">Wallet Balance</h3>
                                <p class="text-3xl font-bold text-cyan-600 dark:text-cyan-400">$125</p>
                            </div>
                        </div>
                    </div>
                    <div class="relative aspect-video overflow-hidden rounded-xl border border-sidebar-border/70 dark:border-sidebar-border">
                        <div
                            class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-emerald-50 to-emerald-100 dark:from-emerald-900/20 dark:to-emerald-800/20"
                        >
                            <div class="text-center">
                                <h3 class="text-lg font-semibold text-emerald-900 dark:text-emerald-100">Loyalty Points</h3>
                                <p class="text-3xl font-bold text-emerald-600 dark:text-emerald-400">2,450</p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Customer Services -->
                <div class="relative min-h-[50vh] flex-1 rounded-xl border border-sidebar-border/70 md:min-h-min dark:border-sidebar-border">
                    <div class="absolute inset-0 p-6">
                        <h2 class="mb-4 text-xl font-semibold text-gray-900 dark:text-white">Customer Services</h2>
                        <div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3">
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Order History</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">View past purchases</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Profile Settings</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Update your information</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Support Tickets</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Get help and support</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Wishlist</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Save items for later</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Notifications</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Manage your alerts</p>
                            </div>
                            <div class="rounded-lg bg-gray-50 p-4 dark:bg-gray-800">
                                <h3 class="font-medium text-gray-900 dark:text-white">Rewards</h3>
                                <p class="text-sm text-gray-600 dark:text-gray-400">Redeem loyalty points</p>
                            </div>
                        </div>
                    </div>
                </div>
            </template>

            <!-- =============================== -->
            <!-- FALLBACK FOR UNKNOWN ROLES -->
            <!-- =============================== -->
            <template v-else>
                <div class="py-8 text-center">
                    <h2 class="mb-2 text-xl font-semibold text-gray-900 dark:text-white">Access Denied</h2>
                    <p class="text-gray-600 dark:text-gray-400">Your role is not recognized. Please contact support.</p>
                </div>
            </template>
        </div>
    </AppLayout>
</template>
