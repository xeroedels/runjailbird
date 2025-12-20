<template>
  <div>
    <Index />
    <Section1/>
    <Section2/>
  </div>
</template>

<script>
import Index from "@/pages/index.vue"
import Section1 from "./components/section-1.vue";
import Section2 from "./components/section-2.vue";

export default{
  name: "App",
  components: {
    Index,
    Section2
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
