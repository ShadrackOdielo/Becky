<template>
    <UCard class="min-h-screen w-screen relative ring-0 rounded-none top-0 " :ui="{body: 'w-full ring-0 top-0 h-full'}">
            <NavbarMenu />
        
        <SideNav class="md:visible invisible"/>
        <slot />
        <template #footer>
            <AppFooter />
        </template>
    </UCard>
</template>
<script setup lang="ts">
</script>