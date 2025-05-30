<template>
    <section id="hero" class="flex flex-col items-center justify-center w-full min-h-screen">
        <div class="text-center">
            <UAvatar src="/imgs/grad.png" alt="Rebecca's Avatar" class="w-32 h-32 mb-4 ring-secondary ring-4  rounded-full shadow-lg" />
            <h1 class="text-4xl font-bold text-(--ui-secondary) mb-4">
                Hi, I'm Rebecca!
            </h1>
            <p class="text-lg text-gray-600 mb-8">
                Certied Accountant at your service.
             </p>
            <div class="flex justify-center">
               <UButton
                    label="Get in Touch"
                    to="#contact"
                    icon="i-lucide:mail"
                    color="secondary"
                    variant="solid"
                    />
                <UButton
                    label="View Resume"
                    to="/resume.pdf"
                    color="secondary"
                    download
                    icon="i-lucide:file-text"
                    variant="outline"
                    target="_blank"
                    class="ml-4"
                />
                </div>
        </div>
    </section>
</template>
<script setup lang="ts">
</script>