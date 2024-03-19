<script setup>
import FaqDetail from '@/components/FaqDetail.vue';

const faqItems = [
    {
        title: "How long does the course take?",
        content: "The video content takes more than 4.5 hours. ..."
    },
    {
        title: "Who teaches courses on Atheros Learning?",
        content: "The authors of the courses are mostly ..."
    },
    {
        title: "How is the course different from other UX/UI design courses?",
        content: "The key aspect is that this course provides a clear overview of the whole design process and principles, that represent necessary information for being successful within the industry. You will also get direct support from the author of this course, who is ready to answer all your questions and care about your next steps. Last but not least is the fact, that it's not only about video content, but you will also get access to the unique database of design resources and special offers from the partners of the course."
    },
    {
        title: "Do I get a certificate after completing a course?",
        content: "Yes, after successfully finishing the quizzes within the course, you can download a certificate, proving all gained knowledge and skills."
    },
    {
        title: "Are there any hidden fees within the course?",
        content: " Absolutely not! You will gain all benefits and features with the one-time payment, unlocking the course."
    },
]

</script>

<template>
    <div class="faq-container">
        <FaqDetail
            v-for="(item, index) in faqItems"
            :key="index"
            :faqTitle="item.title"
            :faqContent="item.content"
        />
    </div>
</template>

<style scoped>
.faq-container {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 12rem;
  max-width: 37.5rem;
  padding: 2rem;
  border-radius: 1em;
  background-color: var(--background);
}

</style>