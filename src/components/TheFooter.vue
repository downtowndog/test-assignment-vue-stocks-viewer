<template>
  <section>
    <p class="footer__copyright">
      Built by <a href="#" class="footer__link">Nick Ivankov</a> for Peiko Comp.
      Have a good day!
    </p>
  </section>
</template>

<style lang="scss" scoped>
.footer {
  &__copyright {
    font-weight: 500;
    margin-top: 5rem;
    font-size: 1.2rem;
  }
  &__link {
    &:link,
    &:visited {
      background-color: #fff;
      color: #43bad6;
      text-decoration: none;
      display: inline-block;
      transition: all 0.2s;
    }

    &:hover,
    &:active {
      background-color: #43bad6;
      color: #fff;
      transform: rotate(5deg) scale(1.3);
    }
  }
}
</style>
