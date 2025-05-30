<template>
<UCard id="contact"  :ui="{header:'w-full',body:'w-full justify-between gap-4 flex justify-center items-center'}" class="w-full min-h-screen ring-0 p-0 md:p-0 rounded-none">
    <template #header>
            <h1 class="font-bold text-2xl text-center  text-(--ui-secondary)">
                Contact Section
            </h1>
            <p class="text-center text-gray-600">Get in Touch</p>
    </template>
    <UCard class="w-1/2  p-4">
        <template #header>
            <h2 class="text-xl font-semibold text-(--ui-secondary)">
                Get in Touch
            </h2>
        </template>
        <p class="text-gray-600">
            If you have any questions or would like to get in touch, feel free to reach out via email or connect with me on LinkedIn.
        </p>
        <UButton
            label="Email Me"
            to="mailto:odielorebecca@gmail.com"
            icon="i-lucide:mail"
            color="secondary"
            variant="solid"
            class="mt-4"
        />
        <UButton
            label="Connect on LinkedIn"
            to="https://www.linkedin.com/in/rebecca-odielo/"
            icon="i-lucide:linkedin"
            color="secondary"
            variant="outline"
            target="_blank"
            class="mt-2"
        />
    </UCard>
</UCard>
</template>