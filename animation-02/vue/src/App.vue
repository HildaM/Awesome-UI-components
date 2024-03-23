<script setup>

</script>

<template>
  <div class="box">
    <a class="button" href="https://github.com/hildam" title="Better Live, Better Study">
      <svg height="60px" width="100%" viewBox="0 0 300 60" xmlns="http://www.w3.org/2000/svg">
            <rect class="shape"/>
            <text x="50%" y="50%" class="text">Better Live, Better Study</text>
      </svg>
    </a>
  </div>
</template>

<style scoped>

.box {
  display: flex;
  align-items: center;
  justify-content: center; /* 修改：确保内容水平和垂直居中 */
  height: 60px; /* 修改：指定高度与 SVG 一致 */
}

.button {
  display: inline-block;
  width: 29rem;
  height: 60px; /* 修改：确保按钮高度与 SVG 一致 */
  margin: 0 auto;
  text-align: center;
  text-transform: uppercase;
  text-decoration: none;
  position: relative;
} 

.shape {
  fill: transparent;
  justify-content: center;
  stroke: cyan;
  height: 60px; 
  width: 430px;

  stroke-dasharray: 120 720;
  stroke-dashoffset: -454;
  stroke-width: 8px;
  transition: 
    stroke-width 0.9s,
    stroke-dashoffset 0.9s,
    stroke-dasharray 0.9s;

  &:hover {
    stroke-width: 3px;
    stroke-dashoffset: 0;
    stroke-dasharray: 720;
  }
}

.text {
    font-size: 1.4rem;
    line-height: 2rem;
    letter-spacing: 0.3rem;
    fill: #fff;
    top: -3rem;
    position: relative;
    pointer-events: none;
    font-family: 'Mulish', sans-serif;
}

</style>
