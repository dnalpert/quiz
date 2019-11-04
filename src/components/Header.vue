<template>
  <div class="header-container">
    <b-nav tabs>
      <b-nav-item disabled><b>Sweet Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>