<template>
    <AdminLayout>
        <PageBreadcrumb :pageTitle="currentPageTitle" />
        <div
            class="overflow-hidden rounded-2xl border border-gray-200 bg-white px-4 pb-3 pt-4 dark:border-gray-800 dark:bg-white/[0.03] sm:px-6">

            <div class="flex flex-col gap-2 mb-4 sm:flex-row sm:items-center sm:justify-between">
                <div>
                    <h3 class="text-lg font-semibold text-gray-800 dark:text-white/90">Usuários do Sistema</h3>
                    <p class="text-gray-500 text-theme-xs dark:text-gray-400 mt-1">Gerencie todos os usuários
                        cadastrados
                    </p>
                </div>

                <div class="flex items-center gap-3">
                    <button
                        class="inline-flex items-center gap-2 rounded-lg border border-gray-300 bg-white px-4 py-2.5 text-theme-sm font-medium text-gray-700 shadow-theme-xs hover:bg-gray-50 hover:text-gray-800 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:hover:bg-white/[0.03] dark:hover:text-gray-200">
                        <svg class="stroke-current fill-white dark:fill-gray-800" width="20" height="20"
                            viewBox="0 0 20 20">
                            <path d="M2.29 5.90393H17.7067" stroke="" stroke-width="1.5" stroke-linecap="round" />
                            <path d="M17.7075 14.0961H2.29085" stroke="" stroke-width="1.5" stroke-linecap="round" />
                            <path
                                d="M12.0826 3.33331C13.5024 3.33331 14.6534 4.48431 14.6534 5.90414C14.6534 7.32398 13.5024 8.47498 12.0826 8.47498C10.6627 8.47498 9.51172 7.32398 9.51172 5.90415C9.51172 4.48432 10.6627 3.33331 12.0826 3.33331Z"
                                fill="" stroke="" stroke-width="1.5" />
                            <path
                                d="M7.91745 11.525C6.49762 11.525 5.34662 12.676 5.34662 14.0959C5.34661 15.5157 6.49762 16.6667 7.91745 16.6667C9.33728 16.6667 10.4883 15.5157 10.4883 14.0959C10.4883 12.676 9.33728 11.525 7.91745 11.525Z"
                                fill="" stroke="" stroke-width="1.5" />
                        </svg>
                        Filtro
                    </button>

                    <button
                        class="inline-flex items-center gap-2 rounded-lg border border-gray-300 bg-white px-4 py-2.5 text-theme-sm font-medium text-gray-700 shadow-theme-xs hover:bg-gray-50 hover:text-gray-800 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:hover:bg-white/[0.03] dark:hover:text-gray-200">
                        Novo Usuário
                    </button>
                </div>
            </div>

            <div class="max-w-full overflow-x-auto custom-scrollbar">
                <table class="min-w-full">
                    <thead>
                        <tr class="border-t border-gray-100 dark:border-gray-800">
                            <th class="py-3 text-left">
                                <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Usuário</p>
                            </th>
                            <th class="py-3 text-left">
                                <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">E-mail</p>
                            </th>
                            <th class="py-3 text-left">
                                <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Cargo</p>
                            </th>
                            <th class="py-3 text-left">
                                <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Status</p>
                            </th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr v-for="(user, index) in users" :key="index"
                            class="border-t border-gray-100 dark:border-gray-800">
                            <td class="py-3 whitespace-nowrap">
                                <div class="flex items-center gap-3">
                                    <div class="h-[45px] w-[45px] overflow-hidden rounded-full">
                                        <img :src="user.image" :alt="user.name" />
                                    </div>
                                    <div>
                                        <p class="font-medium text-gray-800 text-theme-sm dark:text-white/90">
                                            {{ user.name }}
                                        </p>
                                        <span class="text-gray-500 text-theme-xs dark:text-gray-400">{{ user.role
                                            }}</span>
                                    </div>
                                </div>
                            </td>

                            <td class="py-3 whitespace-nowrap">
                                <p class="text-gray-600 text-theme-sm dark:text-gray-400">{{ user.email }}</p>
                            </td>

                            <td class="py-3 whitespace-nowrap">
                                <p class="text-gray-600 text-theme-sm dark:text-gray-400">{{ user.cargo }}</p>
                            </td>

                            <td class="py-3 whitespace-nowrap">
                                <span :class="[
                                    'px-3 py-1 rounded-full text-xs font-medium',
                                    user.status === 'Ativo'
                                        ? 'bg-green-100 text-green-700 dark:bg-green-900/30 dark:text-green-300'
                                        : 'bg-red-100 text-red-700 dark:bg-red-900/30 dark:text-red-300'
                                ]">
                                    {{ user.status }}
                                </span>
                            </td>
                        </tr>
                    </tbody>

                </table>
            </div>
        </div>

    </AdminLayout>
</template>

<script setup>
import PageBreadcrumb from '@/components/common/PageBreadcrumb.vue';
import AdminLayout from '@/components/layout/AdminLayout.vue';
import { ref } from 'vue'
const currentPageTitle = ref('Todos os usuários')

const users = ref([
    {
        name: 'Victor Souza',
        image: 'https://i.pravatar.cc/100?img=1',
        email: 'victor.souza@example.com',
        cargo: 'Administrador',
        role: 'Full Access',
        status: 'Ativo'
    },
    {
        name: 'Marina Costa',
        image: 'https://i.pravatar.cc/100?img=48',
        email: 'marina.costa@example.com',
        cargo: 'Recepcionista',
        role: 'Atendimento',
        status: 'Ativo'
    },
    {
        name: 'Rafael Lima',
        image: 'https://i.pravatar.cc/100?img=22',
        email: 'rafael.lima@example.com',
        cargo: 'Dentista',
        role: 'Profissional',
        status: 'Inativo'
    },
    {
        name: 'Laura Mendes',
        image: 'https://i.pravatar.cc/100?img=14',
        email: 'laura.mendes@example.com',
        cargo: 'Assistente',
        role: 'Auxiliar',
        status: 'Ativo'
    },
    {
        name: 'João Henrique',
        image: 'https://i.pravatar.cc/100?img=5',
        email: 'joao.henrique@example.com',
        cargo: 'Dentista',
        role: 'Profissional',
        status: 'Ativo'
    },
    {
        name: 'Tamara Silva',
        image: 'https://i.pravatar.cc/100?img=29',
        email: 'tamara.silva@example.com',
        cargo: 'Recepcionista',
        role: 'Atendimento',
        status: 'Ativo'
    },
    {
        name: 'Eduardo Matos',
        image: 'https://i.pravatar.cc/100?img=7',
        email: 'eduardo.matos@example.com',
        cargo: 'Administrador',
        role: 'Full Access',
        status: 'Inativo'
    },
    {
        name: 'Paula Rodrigues',
        image: 'https://i.pravatar.cc/100?img=30',
        email: 'paula.rodrigues@example.com',
        cargo: 'Assistente',
        role: 'Auxiliar',
        status: 'Ativo'
    },
    {
        name: 'Lucas Andrade',
        image: 'https://i.pravatar.cc/100?img=9',
        email: 'lucas.andrade@example.com',
        cargo: 'Dentista',
        role: 'Profissional',
        status: 'Ativo'
    },
    {
        name: 'Camila Nunes',
        image: 'https://i.pravatar.cc/100?img=15',
        email: 'camila.nunes@example.com',
        cargo: 'Recepcionista',
        role: 'Atendimento',
        status: 'Inativo'
    },
    {
        name: 'Fábio Correia',
        image: 'https://i.pravatar.cc/100?img=11',
        email: 'fabio.correia@example.com',
        cargo: 'Administrador',
        role: 'Full Access',
        status: 'Ativo'
    },
    {
        name: 'Fernanda Dias',
        image: 'https://i.pravatar.cc/100?img=34',
        email: 'fernanda.dias@example.com',
        cargo: 'Assistente',
        role: 'Auxiliar',
        status: 'Ativo'
    },
    {
        name: 'Gustavo Freitas',
        image: 'https://i.pravatar.cc/100?img=17',
        email: 'gustavo.freitas@example.com',
        cargo: 'Dentista',
        role: 'Profissional',
        status: 'Ativo'
    },
    {
        name: 'Bruna Teixeira',
        image: 'https://i.pravatar.cc/100?img=36',
        email: 'bruna.teixeira@example.com',
        cargo: 'Recepcionista',
        role: 'Atendimento',
        status: 'Ativo'
    },
    {
        name: 'Henrique Moraes',
        image: 'https://i.pravatar.cc/100?img=3',
        email: 'henrique.moraes@example.com',
        cargo: 'Adm Financeiro',
        role: 'Financeiro',
        status: 'Inativo'
    }
])

</script>
