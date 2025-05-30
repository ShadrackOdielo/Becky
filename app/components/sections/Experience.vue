<template>
<UCard id="experience" :ui="{header:'w-full',body:'w-full justify-between gap-4 flex md:flex-rows-2'}" class="w-full min-h-screen ring-0 p-0 md:p-0 rounded-none">
    <template #header>
            <h1 class="font-bold text-2xl text-center text-(--ui-secondary)">
                Experience Section
            </h1>
            <p class="text-center text-gray-600">My Professional Journey</p>
    </template>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 w-full p-4">
        <UCard v-for="(job, index) in workHistory" :key="index" class="p-4 shadow-lg hover:shadow-xl transition-shadow duration-300">
            <template #header>
                <h2 class="text-xl font-semibold text-(--ui-secondary)">
                    {{ job.company }}
                </h2>
                <p class="text-gray-600">{{ job.position }} ({{ job.duration }})</p>
            </template>
            <ul class="list-disc pl-5 mt-2">
                <li v-for="(responsibility, rIndex) in job.responsibilities" :key="rIndex" class="text-gray-600">{{ responsibility }}</li>
            </ul>
            <p class="text-gray-600 font-bold mt-2">Skills:</p>
            <ul class="list-disc pl-5 mt-2">
                <li v-for="(skill, sIndex) in job.skills" :key="sIndex" class="text-gray-600">{{ skill }}</li>
            </ul>
            </UCard>
        </div>
</UCard>
</template>
<script setup lang="ts">

const workHistory = [
    {
        company: "Diamond Trust Bank",
        position: "Intern",
        duration: "June 2023 - Present",
        responsibilities: [
            "Assisted in daily banking operations and customer service.",
            "Supported the finance team in preparing financial reports.",
            "Conducted market research to identify potential clients."
        ],
        skills: [
            "Customer Service",
            "Financial Reporting",
            "Market Research"
        ]
    },
    {
        company: "National Housing Commision",
        position: "shadowing Intern",
        duration: "January 2023 - May 2023",
        responsibilities: [
            "Shadowed senior accountants to learn about financial management.",
            "Assisted in data entry and record keeping.",
            "Participated in team meetings to understand project workflows."
        ],
        skills: [
            "Data Entry",
            "Record Keeping",
            "Team Collaboration"
        ],
    },
    {
        company: "Heritage of Faith and Hope Children's Home",
        position: "Volunteer ",
        duration: "June 2022 - December 2022",
        responsibilities: [
            "Managed financial records and budgets for the home.",
            "Organized fundraising events to support the home.",
            "Provided mentorship to children in the home."
        ],
        skills: [
            "Financial Management",
            "Event Organization",
            "Mentorship"
        ]
    }
];
</script>