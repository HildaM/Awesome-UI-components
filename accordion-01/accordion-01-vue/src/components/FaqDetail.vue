<script setup>
import { ref, nextTick, onMounted, watch } from 'vue';
import plusIcon from '@/assets/plus.svg';  // 引入加号图标
import minusIcon from '@/assets/minus.svg';  // 引入减号图标

const props = defineProps({
    faqTitle: String,
    faqContent: String,
})

const detail = ref(null)
const summary = ref(null)
const content = ref(null)
const isOpen = ref(false)
const expandIcon = ref(plusIcon)

// 监控打开与关闭的状态
watch(isOpen, (newVal) => {
    expandIcon.value = newVal ? minusIcon : plusIcon;
    // 确保在 DOM 更新后执行动画
    nextTick(() => {
        newVal ? expand() : shrink();
    });
})

function toggleDetail(event) {
    event.preventDefault()
    isOpen.value = !isOpen.value
}

onMounted(() => {
    content.value.style.display = 'none'
    detail.value.style.overflow = 'hidden';  // 隐藏shrink过程中超出方块的内容。
})


function expand() {
    // 先将display设置为block，然后立即计算contentHeight
    content.value.style.display = 'block';
    content.value.style.opacity = '0';  // 设置初始透明度为0
    const summaryHeight = summary.value.offsetHeight;
    const contentHeight = content.value.offsetHeight;
    // 计算完contentHeight后，立即将display设置回none
    content.value.style.display = 'none';
    
    const endHeight = `${summaryHeight + contentHeight}px`

    detail.value.animate([
        { height: `${summaryHeight}px` },
        { height: endHeight }
    ], {
        duration: 350,
        easing: 'ease-out',
        fill: 'forwards'
    }).onfinish = () => {
        // 在动画完成后设置display为block
        content.value.style.display = 'block'
    }

    // 添加一个新的动画，逐渐改变faq-content的opacity
    content.value.animate([
        { opacity: '0' },
        { opacity: '1' }
    ], {
        duration: 350,
        easing: 'ease-out',
        fill: 'forwards'
    });
}

function shrink() {
    const summaryHeight = summary.value.offsetHeight;
    
    const animation = detail.value.animate([
        { height: `${detail.value.offsetHeight}px` },
        { height: `${summaryHeight}px` }
    ], {
        duration: 400,
        easing: 'ease-out',
        fill: 'forwards'
    });

    animation.onfinish = () => {
        content.value.style.display = 'none';
    };
}

</script>

<template>
    <div class="detail" @click="toggleDetail" :open="isOpen" ref="detail">
        <summary ref="summary">
            <span class="faq-title">{{ props.faqTitle }}</span>
            <img :src=expandIcon class="expand-icon" alt="Plus">
        </summary>
        <div class="faq-content" ref="content">{{ props.faqContent }}</div>
    </div>
</template>

<style scoped>
.detail {
	font-size: 1rem;
	margin: 0 auto;
	width: 100%;
	background: #F6FAFF;
	border-radius: 0.5rem;
	position: relative;
	max-width: 600px;
    border: 1px solid #C3DEFF;
    transition: all 0.3s ease-in-out;

    &:hover {
        border: 1px solid #A4A1FF;
    }
}


summary {
  user-select: none;
  cursor: pointer;
  font-weight: 600;
  display: flex;
  list-style: none;
  padding: 1rem;
  align-items: center;

  &:hover {
    .faq-title,
    .expand-icon {
      opacity: 1;
    }
  }

  &::-webkit-details-marker {
    display: none;
  }

  &:focus {
    outline: none;
  }
}

.faq-title {
  color: #1C2035;
  width: 90%;
  opacity: 0.65;
  transition: all 250ms ease-in-out;
}

.faq-content {
  color: #303651;
  padding: 0.2rem 1rem 1rem 1rem;
  font-weight: 300;
  line-height: 1.5;
}

.expand-icon {
  opacity: 0.65;
}

.expand-icon {
  pointer-events: none;
  position: absolute;
  right: 1rem;
  transition: all 150ms ease-out;
}


</style>