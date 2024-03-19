<script setup>
import { ref, onMounted, nextTick, watch } from 'vue';
import plusIcon from '@/assets/plus.svg';  // 引入加号图标
import minusIcon from '@/assets/minus.svg';  // 引入减号图标

const props = defineProps({
    faqTitle: String,
    faqContent: String,
})

// 页面元素组件
const detail = ref(null)
const content = ref(null)
const summary = ref(null)
const isOpen = ref(false)
const expandIcon = ref(plusIcon)

function troggleDetail(event) {
    event.preventDefault()  // 阻止默认事件
    isOpen.value = !isOpen.value
}

onMounted(() => {
    content.value.style.display = 'none'
    detail.value.style.overflow = 'hidden'
})

watch(isOpen, (newVal) => {
    expandIcon.value = newVal ? minusIcon : plusIcon
    nextTick(() => {
        newVal ? expand() : shrink()
    })
})


function expand() {
    // 将detail盒子展开，但是不要让用户知道。这样可以获取盒子展开后的大小，方便后续绘制动画
    content.value.style.display = 'block'
    content.value.style.opacity = '0'   // 透明度为0

    // 获取展开后的高度
    const summaryHeight = summary.value.offsetHeight
    const contentHeight = content.value.offsetHeight
    // 计算最终的高度
    const endHeight = `${summaryHeight + contentHeight}px`

    // 将盒子还原
    content.value.style.display = 'none'

    // 绘制动画
    detail.value.animate([
        {height: `${summaryHeight}px`},
        {height: endHeight}
    ], {
        duration: 350,
        easing: 'ease-out',
        fill: 'forwards'
    }).onfinish = () => {
        // 动画结束时，将coontent内容置为可见
        content.value.style.display = 'block'
    }

    // 创建另一个动画，使 content 的透明度从 0 逐渐增加到 1
    content.value.animate([
        { opacity: '0' },
        { opacity: '1' }
    ], {
        duration: 350,  // 动画持续 350 毫秒
        easing: 'ease-out',  // 使用 ease-out 缓动函数
        fill: 'forwards'  // 动画结束后保持最后一帧的样式
    });
}

function shrink() {
    const summaryHeight = summary.value.offsetHeight

    detail.value.animate([
        {height: `${detail.value.offsetHeight}px`},
        {height: `${summaryHeight}px`}
    ], {
        duration: 400,  // 动画持续 400 毫秒
        easing: 'ease-out',  // 使用 ease-out 缓动函数
        fill: 'forwards'  // 动画结束后保持最后一帧的样式
    }).onfinish = () => {
        content.value.style.display = 'none'
    }
}

</script>

<template>

    <details ref="detail" @click="troggleDetail" :open="isOpen">
      <summary ref="summary">
        <span class="faq-title">
          {{ props.faqTitle }}
        </span>
        <svg xmlns="http://www.w3.org/2000/svg" class="expand-icon" width="24" height="24" viewBox="0 0 24 24"
          stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
          <path d="M9 6l6 6l-6 6"></path>
        </svg>
      </summary>
      <div class="faq-content" ref="content">
        {{ props.faqContent }}
      </div>
    </details>

</template>


<style scoped>
details {
  font-size: 1rem;
  margin: 0 auto;
  width: 100%;
  border-radius: 0.5rem;
  position: relative;
  max-width: 37.5rem;
  transition: all 0.3s ease-in-out;
}

details:hover {
  background-color: var(--background-hover);
}

details:hover svg {
  stroke: var(--primary-blue);
}

details[open] {
  background-color: var(--background-hover);
}

details[open] .faq-title {
  color: var(--primary-blue);
}

summary {
  user-select: none;
  cursor: pointer;
  font-weight: 600;
  display: flex;
  list-style: none;
  padding: 1rem;
  align-items: center;
}

summary svg {
  stroke: var(--white);
}

details[open] summary svg {
  stroke: var(--primary-blue);
  transform: rotate(90deg);
}

summary:hover .faq-title {
  color: var(--primary-blue);
}

summary::-webkit-details-marker {
  display: none;
}

summary:focus {
  outline: none;
}

.faq-title {
  color: var(--white);
  width: 90%;
  transition: all 250ms ease-in-out;
}

.faq-content {
  color: var(--white);
  padding: 0.2rem 1rem 1rem 1rem;
  font-weight: 300;
  line-height: 1.5;
}

.expand-icon {
  pointer-events: none;
  position: absolute;
  right: 1rem;
  transition: all 150ms ease-out;
}summary {
  user-select: none;
  cursor: pointer;
  font-weight: 600;
  display: flex;
  list-style: none;
  padding: 1rem;
  align-items: center;
}

summary svg {
  stroke: var(--white);
}

details[open] summary svg {
  stroke: var(--primary-blue);
  transform: rotate(90deg);
}

summary:hover .faq-title {
  color: var(--primary-blue);
}

summary::-webkit-details-marker {
  display: none;
}

summary:focus {
  outline: none;
}

.faq-title {
  color: var(--white);
  width: 90%;
  transition: all 250ms ease-in-out;
}

.faq-content {
  color: var(--white);
  padding: 0.2rem 1rem 1rem 1rem;
  font-weight: 300;
  line-height: 1.5;
}

.expand-icon {
  pointer-events: none;
  position: absolute;
  right: 1rem;
  transition: all 150ms ease-out;
}

</style>