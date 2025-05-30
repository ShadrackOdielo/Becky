<template>
    <UCard id="about" :ui="{header:'w-full',body:'w-full justify-between gap-4 flex md:flex-rows-2'}" class="  w-full min-h-screen ring-0 p-0 md:p-0 rounded-none">
        <template #header>
                <h1 class="font-bold text-2xl text-center  text-(--ui-secondary)">
                    About Section
                </h1>
                <p class="text-center text-gray-600">Who I Am</p>
            
        </template>
            <img src="/imgs/profile.png" class="object-cover w-96 ring animate-pulse"/>
        <UCard class="w-1/2 p-4">
            <template #header>
                <h2 class="text-xl font-semibold text-(--ui-secondary)">
                   About Rebecca
                </h2>
            </template>
            <p class="text-gray-600">
                Rebecca Auma Odielo holds a Bachelors Degree in Commerce from Multimedia University with a specialization in Accounting. She is also fluent in English and swahili.
            </p>
            <p class="text-gray-600">
                With a passion for numbers and a keen eye for detail, Rebecca has honed her skills in financial analysis, budgeting, and reporting. She is dedicated to helping businesses achieve their financial goals through accurate and timely accounting practices.
            </p>
            <UButton
                label="Download Resume"
                to="/resume.pdf"
                color="secondary"
                download
                icon="i-lucide:file-text"
                variant="outline"
                target="_blank"
                class="mt-4"
            />
            <UButton
                label="linkedin profile"
                icon="i-lucide:linkedin"
                to="https://www.linkedin.com/in/rebecca-odielo/"
                color="secondary"
                variant="outline"
                target="_blank"
                class="mt-2"
            />

            </UCard>
       
    </UCard>
</template>