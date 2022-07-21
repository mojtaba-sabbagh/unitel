<script>
  import Search from './components/Search.vue';
  export default {
        data() {
          return {
          }
        },
        components: {
          Search,
        },
  }
</script>

<template>
  <div dir="rtl" class="md:mr-10 md:my-7 md:grid grid-cols-3 gap-6 items-center">
      <img class="invisible md:visible" src="@/assets/logo-new.png"  width="75%"/>
      <p class="mt-1 md:mb-5 text-xl text-gray-600 p-2 text-center">
            <span class="bg-red-100 py-2 px-6 md:text-2xl font-farsi hover:text-gray-900 hover:ring-1">
                سامانه دفتر تلفن دانشکاه
            </span>
      </p>
      <img src="@/assets/IT-logo2.png" width="75%" class="items-right invisible md:visible"/>
   </div>
  
  <Search />
      
</template>

<style>
@import '@/assets/base.css';

body {
  height: 100%;
  width: 100%;
  z-index: -1;
  background: var(--color-darkblue);
  background-image: linear-gradient(
      115deg,
      rgba(58, 58, 158, 0.8),
      rgba(136, 136, 206, 0.7)
    ),
    url(@/assets/bacground-1.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
</style>
