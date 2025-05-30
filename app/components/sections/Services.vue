<template>
    <UCard id="services" class="flex flex-col items-center justify-center ring-0 p-0 md:p-0 rounded-none w-full min-h-screen " :ui="{}">
        <template #header>
            <h1 class="font-bold text-center text-2xl text-(--ui-secondary)">
                My Services
            </h1>
            <p class="text-center">What I do</p>
        </template>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 w-full p-4">
            <UCard v-for="service in services" :key="service.title" class="p-4 shadow-lg hover:shadow-xl transition-shadow duration-300">
                <template #header>
                    <div class="flex items-center gap-2">
                        <UIcon :name="service.icon" class="text-(--ui-secondary) text-2xl" />
                        <h2 class="text-lg font-semibold text-(--ui-secondary)">
                            {{ service.title }}
                        </h2>
                        </div>
                </template>
                <p class="text-gray-600">
                    {{ service.description }}
                </p>
    </UCard>
        </div>
    </UCard>
</template>
<script setup lang="ts">

const services = [
    {
        title: "Financial Analysis",
        description: "Providing detailed financial analysis to help businesses make informed decisions.",
        icon: "i-lucide:bar-chart-2"
    },
    {
        title: "Budgeting and Forecasting",
        description: "Assisting in creating budgets and forecasts to ensure financial stability.",
        icon: "i-lucide:calendar-check"
    },
    {
        title: "Tax Preparation",
        description: "Preparing and filing taxes to ensure compliance with regulations.",
        icon: "i-lucide:file-tax"
    },
    {
        title: "Bookkeeping",
        description: "Maintaining accurate financial records for businesses.",
        icon: "i-lucide:file-text"
    }
];
</script>