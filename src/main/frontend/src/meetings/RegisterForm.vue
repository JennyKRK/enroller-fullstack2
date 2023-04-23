<template>

  <form @submit.prevent="enter()">
    <label>Your login</label>
    <input type="text">
    <label>Your password</label>
    <input type="password">
    <button type="submit">{{ buttonLabel || 'Potwierdz rejestracje' }}</button>
  </form>

</template>

<script>
export default {
  name: "RegisterForm"
}
</script>

<style scoped>

</style>


