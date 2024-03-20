<script setup>

</script>

<template>
    <div class="sound-wave">
      <div></div>
      <div></div>
      <div></div>
      <div></div>
      <div></div>
    </div>
</template>

<style scoped>
@keyframes  audio-wave {
  0% {
    height: 6px;
    transform: translateY(0px);
    background: #ffd13a;
  }

  25% {
    height: 40px;
    transform: translateY(-5px) scaleY(1.7);
    background: #5d3bda;
  }

  50%  {
    height: 6px;
    transform: translateY(0px);
    background: #17cece;
  }

  100% {
    height: 6px;
    transform: translateY(0px);
    background: #e01854;
  }
}

.sound-wave {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 0.5rem;
  width: 3.75rem;
  height: 2.5rem;

  div {
    display: block;
    width: 0.6rem;
    height: 0.4rem;
    border-radius: 0.5rem;
    background: orange;
    animation: audio-wave 2.2s infinite ease-in-out;


    &:nth-child(2) {
      left: 11px;
      animation-delay: 0.2s;
    }

    &:nth-child(3) {
      left: 22px;
      animation-delay: 0.4s;
    }

    &:nth-child(4) {
      left: 33px;
      animation-delay: 0.6s;
    }

    &:nth-child(5) {
      left: 44px;
      animation-delay: 0.8s;
    }
  }
}
</style>
