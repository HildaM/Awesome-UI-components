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
    // 阻止事件的默认行为
    event.preventDefault();

    // 切换 isOpen 的值，如果 isOpen 是 true，那么将其设置为 false，反之亦然
    isOpen.value = !isOpen.value;
}


onMounted(() => {
    content.value.style.display = 'none'
    detail.value.style.overflow = 'hidden';  // 隐藏shrink过程中超出方块的内容。
})


function expand() {
    // 设置 content 的样式为 block，使其可见，但透明度为 0，使其看起来是隐藏的
    content.value.style.display = 'block';
    content.value.style.opacity = '0';

    // 获取 summary 和 content 的高度
    const summaryHeight = summary.value.offsetHeight;
    const contentHeight = content.value.offsetHeight;

    // 再次将 content 设置为隐藏
    content.value.style.display = 'none';

    // 计算最终的高度（summary 的高度加上 content 的高度）
    const endHeight = `${summaryHeight + contentHeight}px`;

    // 创建一个动画，使 detail 的高度从 summary 的高度逐渐增加到最终的高度
    detail.value.animate([
        { height: `${summaryHeight}px` },
        { height: endHeight }
    ], {
        duration: 350,  // 动画持续 350 毫秒
        easing: 'ease-out',  // 使用 ease-out 缓动函数
        fill: 'forwards'  // 动画结束后保持最后一帧的样式
    }).onfinish = () => {
        // 动画结束后，将 content 设置为可见
        content.value.style.display = 'block';
    };

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
    // 获取 summary 元素的高度
    const summaryHeight = summary.value.offsetHeight;

    // 创建一个动画，使 detail 元素的高度从当前高度逐渐减小到 summary 元素的高度
    const animation = detail.value.animate([
        { height: `${detail.value.offsetHeight}px` },
        { height: `${summaryHeight}px` }
    ], {
        duration: 400,  // 动画持续 400 毫秒
        easing: 'ease-out',  // 使用 ease-out 缓动函数
        fill: 'forwards'  // 动画结束后保持最后一帧的样式
    });

    // 动画结束后，将 content 元素隐藏
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