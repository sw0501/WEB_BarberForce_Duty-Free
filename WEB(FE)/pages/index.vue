<template>
  <main class="container pb-20">
    <!-- 미용실 carousel -->
    <ReservationPreviewCarousel v-if="undoneReservations.length > 0" :reservations="undoneReservations" />
    <BarberCarousel v-else />
    <div class="p-3">
      <!-- '머리깎고 뭐하지?' section -->
      <section class="mb-12">
        <CommonHeading class="mb-2">머리깎고 뭐하지?</CommonHeading>
        <!-- 게시글 carousel (머리깎고 뭐하지?) -->
        <ArticleCarousel />
        <!-- view more link -->
        <div class="flex justify-center">
          <NuxtLink
            class="flex justify-center rounded bg-brand font-bold text-white w-full max-w-md px-1 py-2"
            to="/board/2"
          >
            더보기
          </NuxtLink>
        </div>
      </section>
      <!-- quick menu icons -->
      <section class="flex justify-center items-center mb-8">
        <!-- 미용실 정보 -->
        <NuxtLink class="flex flex-col items-center mx-3 md:mx-8" to="/barbers">
          <!-- 원형 이미지 or 아이콘 -->
          <!-- || dummy placeholder || -->
          <div class="flex justify-center items-center w-16 md:w-20 h-16 md:h-20 mb-2">
            <img src="@/assets/img/doldol.png" />
          </div>
          <!-- text -->
          <span class="font-bold">미용실 정보</span>
        </NuxtLink>
        <!-- 맛집 리스트 -->
        <NuxtLink class="flex flex-col items-center mx-3 md:mx-8" to="/board/2">
          <!-- 원형 이미지 or 아이콘 -->
          <!-- || dummy placeholder || -->
          <div class="flex justify-center items-center w-16 md:w-20 h-16 md:h-20 mb-2">
            <img src="@/assets/img/food.svg" />
          </div>
          <!-- text -->
          <span class="font-bold">맛집 리스트</span>
        </NuxtLink>
        <!-- 커뮤니티 -->
        <NuxtLink class="flex flex-col items-center mx-3 md:mx-8" to="/board/1">
          <!-- 원형 이미지 or 아이콘 -->
          <!-- || dummy placeholder || -->
          <div class="flex justify-center items-center w-16 md:w-20 h-16 md:h-20 mb-2">
            <img src="@/assets/img/chat.svg" />
          </div>
          <!-- text -->
          <span class="font-bold">자유 게시판</span>
        </NuxtLink>
      </section>
      <!-- search form section -->
      <div class="flex justify-center">
        <div class="relative w-full max-w-md">
          <img
            class="absolute left-3 top-1/2 w-5 h-auto"
            style="transform: translate(0, -50%)"
            src="@/assets/img/search.svg"
          />
          <input
            class="
              w-full
              rounded
              border
              bg-white
              font-semibold
              text-lg text-gray-400
              placeholder-gray-200
              py-2
              pl-12
              pr-4
              focus:outline-none focus:ring focus:ring-white focus:ring-opacity-30
            "
            placeholder="찾으시는 것이 있나요?"
            type="text"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  middleware: 'auth',
  data() {
    return {
      undoneReservations: [],
    };
  },
  async fetch() {
    const { data } = await this.$api.reservations.list();
    this.undoneReservations = data.reservations.filter(reservation => {
      return !reservation.done;
    });
  },
  methods: {
    logout() {
      this.$auth.logout();
    },
  },
};
</script>

<style scoped></style>
