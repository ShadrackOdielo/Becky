<template>
    <UCard class="min-h-screen w-screen relative " :ui="{body: 'w-full h-full'}">
        <template #header>
            <NavbarMenu />
        </template>
        <SideNav/>
        <slot />
        <template #footer>
            <AppFooter />
        </template>
    </UCard>
</template>
<script setup lang="ts">
</script>