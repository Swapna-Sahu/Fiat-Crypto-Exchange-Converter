<template>
  <v-carousel
  cycle
  height="350"
    :hide-delimiter-background="false"
    :show-arrows="false"
  >
    <v-carousel-item v-for="(slide, i) in slides" :key="i" :src="slide.src" class="image">
    </v-carousel-item>
  </v-carousel>
</template>
<script>
export default {
  name: "Slider",
  data() {
    return {
      slides: [
        {
          src:
            "https://images.pexels.com/photos/12619/house-money-capitalism-fortune-12619.jpg?auto=compress&cs=tinysrgb&dpr=2&w=500",
        },
        {
          src:
            "https://public.bnbstatic.com/image/cms/blog/20201218/837b40dc-f095-47d4-9e85-c437817ea470.jpg",
        },
        {
          src:
            "https://i0.wp.com/cryptokiemtien.com/wp-content/uploads/2020/04/SOL1.jpg?resize=640%2C360&ssl=1",
        },
        {
          src:
            "https://couponance.com/wp-content/uploads/2020/04/BINANCE_FUTURES_REFERRAL_CODE-FREEDAY.jpg",
        },
        {
          src:
            "https://bitcoinke.io/wp-content/uploads/2020/11/Binance-Instant-Cash-Transfers-in-Nigerian-Naira.jpg?x63648",
        },
      ],
    };
  },
};
</script>
<style scoped>
.v-carousel {
  width: 95% !important;

  overflow: hidden;
}
.image {
  
  background-repeat: no-repeat;
  background-position:center !important;
  height:250px !important;
  width:65%!important;
  margin-left:150px;
}
</style>