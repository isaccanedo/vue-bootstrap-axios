<template>
    <div>
    <!--ref: https://bootstrap-vue.js.org/docs/components/nav-->
    <b-nav>
        <b-nav-item active>opentdb.com Quiz App</b-nav-item>
        <b-nav-item>Counter: {{ numCorrect }} / {{ numTotal }}</b-nav-item>
    </b-nav>
    </div>
</template>

<script>
  export default {
    props: [
      "numCorrect",
      "numTotal"
    ]
  }
</script>