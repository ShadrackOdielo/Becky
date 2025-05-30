<template>
    <UCard id="education" :ui="{body:'gap-4',header:'items-center'}" class="w-full h-full flex flex-col items-center min-h-screen ring-0 p-0 md:p-0 rounded-none justify-center gap-4">
       <template #header>
        <h1 class="font-bold text-center text-2xl text-(--ui-secondary)">
            Education
        </h1>
        <p class=" text-center text-gray-600">My Academic Journey</p>
       </template>
        <UCard v-for="(edu, index) in education" :key="index" class="w-full md:w-1/2 p-4 shadow-lg hover:shadow-xl ring-0 transition-shadow duration-300">
            <template #header>
                <h2 class="text-xl font-semibold text-(--ui-secondary)">
                    {{ edu.institution }}
                </h2>
                <p class="text-gray-600">{{ edu.degree }} ({{ edu.year }})</p>
            </template>
            <p class="text-gray-600">{{ edu.description }}</p>
            <p class="text-gray-600 font-bold">Grade: {{ edu.grade }}</p>
            <ul class="list-disc pl-5 mt-2">
                <li v-for="(subject, subIndex) in edu.subjects" :key="subIndex" class="text-gray-600">{{ subject }}</li>
            </ul>
            </UCard>
       
    </UCard>
</template>
<script setup lang="ts">
// 

const education = [
    {
        institution: "Multimedia University",
        degree: "Bachelor of Commerce (Accounting)",
        year: "2020 - 2024",
        description: "Specialized in Accounting with a focus on financial analysis and reporting.",
        grade: "Second Class Honours Upper Division",
        subjects: [
            "Financial Accounting",
            "Management Accounting",
            "Taxation",
            "Auditing",
            "Business Law"
        ]
    },
    {
        institution: "St MAry's lwak Girls High School",
        degree: "Kenya Certificate of Secondary Education (KCSE)",
        year: "2019 - 2020",
        description: "Completed secondary education with a strong emphasis on mathematics and business studies.",
        grade: "B+",
        subjects: [
            "Mathematics",
            "Business Studies",
            "English",
            "Kiswahili",
            "Geography"
        ]

    }
];
</script>