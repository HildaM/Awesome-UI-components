<script setup>
import { ref } from 'vue';

const isOpen = ref(false)
function onClick() {
    isOpen.value = !isOpen.value
}

</script>

<template>
    <nav class="accordion-container">
        <div class="main-item" :class="{'main-item--open': isOpen}" @click="onClick">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                stroke-width="2"
                stroke="currentColor"
                fill="none"
                stroke-linecap="round"
                stroke-linejoin="round"
            >
                <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                <path d="M12 4l-8 4l8 4l8 -4l-8 -4"></path>
                <path d="M4 12l8 4l8 -4"></path>
                <path d="M4 16l8 4l8 -4"></path>
            </svg>
          My Skills
            <svg
                xmlns="http://www.w3.org/2000/svg"
                class="expand-icon"
                width="44"
                height="44"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                fill="none"
                stroke-linecap="round"
                stroke-linejoin="round"
            >
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <path d="M9 6l6 6l-6 6" />
            </svg>
        </div>

        <ul v-show="isOpen" class="menu-transition">
            <li>Golang</li>
            <li>Python</li>
            <li>Pytorch</li>
            <li>Vue</li>
            <li>Linux</li>
            <li>Windows 11</li>
        </ul>

    </nav>


</template>

<style scoped>

.accordion-container {
  margin: 0 auto;
  min-width: 330px;
  overflow: hidden;

}

.main-item {
  border-radius: 8px;
  padding: 12px;
  display: flex;
  cursor: pointer;
  background: white;
  align-items: center;
  justify-content: start;
  gap: 8px;
  transition: all ease-in-out 0.25s;
}

.main-item:hover {
  background-color: var(--background-violet-light);
  color: var(--primary-violet);
}

.main-item svg {
  color: var(--background);
  transition: all ease-in-out 0.25s;
}

.main-item:hover svg {
  color: var(--primary-violet);
}

.main-item--open {
  border-radius: 8px 8px 0px 0px;
  background-color: var(--primary-violet);
  transition: all 200ms;
  color: white;
}

.main-item--open:hover {
  color: white;
  border-radius: 8px 8px 0px 0px;
  background-color: var(--primary-violet);
}

.main-item--open svg {
  color: white;
}

.main-item--open:hover svg {
  color: white;
}

.accordion-container > ul {
  max-height: 0;
  background-color: var(--background);
  list-style-type: none;
  margin: 0;
  padding-left: 14px;
  opacity: 0;
  background-color: var(--background-violet-light);
  visibility: hidden;
  transition: all ease-in-out 0.2s;
}

.main-item--open + ul {
  visibility: visible;
  max-height: 400px;
  opacity: 1;
  border-radius: 0px 0px 8px 8px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.accordion-container > ul > li {
  height: 48px;
  display: flex;
  width: 100%;
  align-items: center;
}

.accordion-container > ul > li:hover {
  color: var(--primary-violet);
  cursor: pointer;
}

.expand-icon {
  width: 24px;
  height: 24px;
  margin-left: auto;
}

.main-item--open > .expand-icon {
  transform: rotate(90deg);
}

</style>