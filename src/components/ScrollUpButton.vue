<style scoped>
.tab {
  display: flex;
  align-items: center;
  justify-content: center;
  width: fit-content;
  height: fit-content;
  overflow: hidden;
  padding: 10px;
  cursor: pointer;
  background-color: #00000000;
  opacity: 0.5;
  position: fixed;
  z-index: 9999;
}
.tab:hover {
  opacity: 1;
}
</style>

<template>
  <div class="tab">
    <img src="../assets/scrollUp.png" alt="scroll UP" style="width: 70px" />
  </div>
</template>
