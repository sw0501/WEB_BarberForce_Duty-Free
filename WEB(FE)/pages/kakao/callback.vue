<template>
  <div class="container">
    당신의 토큰: {{ token }}<br />
    입니다 하하하하하하하<br />
    <br />
    여기는 두번째 이후 카카오 로그인부터 오는 페이지
  </div>
</template>

<script>
export default {
  async middleware({ route, store, redirect }) {
    const token = route.query.token;
    store.commit('auth/setToken', token);
    await store.dispatch('auth/load');
    if (route.query.first === '1') {
      redirect('/kakao/additional');
    } else {
      redirect('/');
    }
  },
  computed: {
    token() {
      return this.$route.query.token;
    },
  },
};
</script>

<style></style>
