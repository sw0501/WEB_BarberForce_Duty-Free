<template>
  <main class="h-screen w-screen flex flex-col items-center justify-center bg-brand text-white p-3">
    <ValidationObserver v-slot="{ handleSubmit }">
      <form class="text-center" @submit.prevent="handleSubmit(submit)">
        <div class="underline mb-6">회원정보 입력</div>
        <div class="mb-6">
          <label class="block mb-1" for="nickname">닉네임을 입력해주세요.</label>
          <ValidationProvider v-slot="{ errors, classes }" name="닉네임" rules="required">
            <input
              id="nickname"
              v-model="additional.nickname"
              class="
                w-60
                border
                rounded
                p-2
                text-black text-sm
                focus:outline-none focus:border-blue-500 focus:shadow-sm
              "
              type="text"
            />
            <transition name="fade">
              <div v-if="errors.length > 0" class="w-60 text-left text-xs md:text-sm text-red-400" :class="classes">
                {{ errors[0] }}
              </div>
            </transition>
          </ValidationProvider>
        </div>
        <div class="mb-6">
          <label class="block mb-1" for="phone">전화번호를 입력해주세요.</label>
          <ValidationProvider v-slot="{ errors, classes }" name="전화번호" rules="required">
            <input
              id="phone"
              v-model="additional.phone"
              class="
                w-60
                border
                rounded
                p-2
                text-black text-sm
                focus:outline-none focus:border-blue-500 focus:shadow-sm
              "
              type="text"
            />
            <transition name="fade">
              <div v-if="errors.length > 0" class="w-60 text-left text-xs md:text-sm text-red-400" :class="classes">
                {{ errors[0] }}
              </div>
            </transition>
          </ValidationProvider>
        </div>

        <!-- 군번 입력 부분 -->
        <div class="mb-6">
          <label class="block mb-1" for="soldier_id">군번을 입력해주세요.</label>
          <ValidationProvider v-slot="{ errors, classes }" name="군번" rules="required">
            <input
              id="soldier_id"
              v-model="additional.soldier_id"
              class="
                w-60
                border
                rounded
                p-2
                text-black text-sm
                focus:outline-none focus:border-blue-500 focus:shadow-sm
              "
              type="text"
              name="soldier_id"
            />
            <transition name="fade">
              <div v-if="errors.length > 0" class="w-60 text-left text-xs md:text-sm text-red-400" :class="classes">
                {{ errors[0] }}
              </div>
            </transition>
          </ValidationProvider>
        </div>
        <div class="mb-6">
          <label class="block mb-1" for="rank">계급을 입력해주세요.</label>
          <ValidationProvider v-slot="{ errors, classes }" name="계급" rules="required">
            <input
              id="rank"
              v-model="additional.rank"
              class="
                w-60
                border
                rounded
                p-2
                text-black text-sm
                focus:outline-none focus:border-blue-500 focus:shadow-sm
              "
              type="text"
            />
            <transition name="fade">
              <div v-if="errors.length > 0" class="w-60 text-left text-xs md:text-sm text-red-400" :class="classes">
                {{ errors[0] }}
              </div>
            </transition>
          </ValidationProvider>
        </div>
        <div class="flex justify-center">
          <button type="submit" class="rounded bg-blue-500 text-white py-2 px-3">가입하기</button>
        </div>
      </form>
    </ValidationObserver>
  </main>
</template>

<script>
export default {
  layout: 'empty',
  data() {
    return {
      additional: {
        soldier_id: '',
        phone: '',
        nickname: '',
        rank: '',
      },
    };
  },
  methods: {
    async submit() {
      try {
        await this.$api.auth.additional(this.additional);
        this.$toast.success('정상적으로 처리되었습니다!');
        this.$router.replace('/');
      } catch (e) {
        const errorType = e.response.data.error;
        if (errorType === 'EXISTING_SOLDIER_ID') {
          this.$toast.error('이미 사용중인 군번입니다!');
        } else if (errorType === 'INVALID_SOLDIER_ID') {
          this.$toast.error('등록되어 있지 않은 군번입니다!');
        } else {
          this.$toast.error(`에러가 발생했습니다!`);
        }
      }
    },
  },
};
</script>

<style></style>
