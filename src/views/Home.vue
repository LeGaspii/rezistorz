<template>
<div class="container">
    <calc-resist :key="1"/>
    <img src="../assets/resvalue.jpg">
</div>
</template>

<script>
import CalcResist from '../components/CalcResist.vue';

export default ({
  components: {
    CalcResist,
  },
});
</script>
