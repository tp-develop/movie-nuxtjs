<template>
  <div class="container-progress d-flex">
    <v-progress-circular
      :size="70"
      :width="7"
      color="info"
      indeterminate
    ></v-progress-circular>
  </div>
</template>

<style>
.container-progress {
  flex-direction: column;
  justify-content: center;
}
</style>