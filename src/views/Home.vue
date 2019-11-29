<template>
  <div class="hero">
    <HeroList />
    <HeroProfile />
  </div>
</template>

<script>
import HeroList from "@/components/HeroList.vue";
import HeroProfile from "@/components/HeroProfile.vue";

export default {
  name: "home",
  components: {
    HeroList,
    HeroProfile
  }
};
</script>

<style>
.hero {
  width: 100%;
  max-width: 1400px;
}
</style>
