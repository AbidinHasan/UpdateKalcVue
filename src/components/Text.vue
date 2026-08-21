<style scoped>
.loader {
  position: relative;
}

.loader span {
  position: absolute;
  color: #cf2121;
  transform: translate(-50%, -50%);
  font-size: 50px;
  letter-spacing: 1px;
  line-height: 1;
  white-space: nowrap;
  @media (max-width: 600px) {
    font-size: 30px;
  }
}

.loader span:nth-child(1) {
  color: transparent;
  -webkit-text-stroke: 0.3px rgb(250, 250, 250);
}

.loader span:nth-child(2) {
  color: rgb(222, 59, 198);
  -webkit-text-stroke: 1px rgb(218, 7, 179);
  animation: uiverse723 3s ease-in-out infinite;
}

@keyframes uiverse723 {
  0%,
  100% {
    clip-path: polygon(
      0% 45%,
      15% 44%,
      32% 50%,
      54% 60%,
      70% 61%,
      84% 59%,
      100% 52%,
      100% 100%,
      0% 100%
    );
  }

  50% {
    clip-path: polygon(
      0% 60%,
      16% 65%,
      34% 66%,
      51% 62%,
      67% 50%,
      84% 45%,
      100% 46%,
      100% 100%,
      0% 100%
    );
  }
}
</style>

<template>
  <div class="loader">
    <span>Kalkulator Bayaran</span>
    <span>Kalkulator Bayaran</span>
  </div>
</template>
