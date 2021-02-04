<template>
  <button>
    <slot></slot>
  </button>
</template>

<style lang="scss" scoped>
button {
  font: inherit;
  font-weight: 500;
  border: 1px solid #ededed;
  border-radius: 2px;
  background-color: #f5f5f5;
  color: black;
  padding: .5rem 3rem;
  cursor: pointer;
  transition: .2s;
  margin: 2rem;
}

button:hover,
button:active {
  background-color: #43bad6;
  border-color: #43bad6;
  color: #fff;
  box-shadow: 0 8px 12px 0 rgba(0, 0, 0, 0.2);
  transform: scale(1.02);
}
</style>