<template>
  <main class="h-screen w-screen flex flex-col items-center justify-center bg-brand p-3">
    <!-- logo -->
    <img src="@/assets/img/1_logo+title_figma.svg" width="203" alt="BarberForce" />
    <!-- links -->
    <div class="flex flex-col items-stretch w-full max-w-sm text-white text-center px-12 mt-10">
      <!-- login via kakao -->
      <a class="flex mb-2" :href="kakaoLoginURL">
        <img class="h-full w-auto mr-2" src="~/assets/img/kakao.png" alt="Kakao talk" />
        <span class="rounded bg-kakao text-black flex-1 flex items-center justify-center">카카오톡으로 로그인</span>
      </a>
      <!-- login via email -->
      <NuxtLink class="flex mb-2" to="/login">
        <img class="h-full w-auto mr-2" src="~/assets/img/email.png" alt="E-mail" />
        <span class="rounded bg-email text-white flex-1 flex items-center justify-center">이메일로 로그인</span>
      </NuxtLink>
      <!-- register -->
      <NuxtLink class="rounded border border-white p-3" to="/register/1">회원 가입</NuxtLink>
    </div>
  </main>
</template>

<script>
export default {
  layout: 'empty',
  middleware: 'guest',
  computed: {
    kakaoLoginURL() {
      return `https://kauth.kakao.com/oauth/authorize?client_id=${process.env.kakaoRestKey}&redirect_uri=${process.env.kakaoRedirectURI}&response_type=code`;
    },
  },
};
</script>
