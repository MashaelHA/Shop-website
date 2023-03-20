<template>
  <div class="card">


    <div class="card-header bg-white border-0">
      <slot name="header" />
    </div>

    <div class="card-body">
      <slot name="content" />
    </div>

    <div class="card-footer bg-white">
      <slot name="fotter" />
    </div>

  </div>
</template>

<script>
export default {

}
</script>

<style scoped></style>