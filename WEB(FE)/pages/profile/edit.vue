<template>
  <main class="container px-2 pt-2 pb-20">
    <CommonHeading class="mb-4">PROFILE</CommonHeading>

    <div class="w-full mx-auto px-6 max-w-sm md:max-w-lg">
      <div class="text-center mb-6">
        <span class="tracking-widest font-bold text-lg">{{ $store.state.auth.user.name }}</span>
        님,
      </div>
      <ValidationObserver v-slot="{ handleSubmit }">
        <form @submit.prevent="handleSubmit(submit)">
          <div class="mb-6">
            <label class="block mb-1" for="name">이름을 입력해주세요.</label>
            <ValidationProvider v-slot="{ errors, classes }" class="block" name="이름" rules="required|max:10">
              <input
                id="name"
                v-model="profile.name"
                class="
                  w-full
                  border border-gray-500
                  rounded
                  p-2
                  text-black text-sm
                  focus:outline-none focus:border-blue-500 focus:shadow-sm
                "
                type="text"
              />
              <transition name="fade">
                <div v-if="errors.length > 0" class="text-left text-xs md:text-sm text-red-400" :class="classes">
                  {{ errors[0] }}
                </div>
              </transition>
            </ValidationProvider>
          </div>
          <div class="mb-6">
            <label class="block mb-1" for="nickname">닉네임을 입력해주세요.</label>
            <ValidationProvider v-slot="{ errors, classes }" class="block" name="닉네임" rules="required|max:10">
              <input
                id="nickname"
                v-model="profile.nickname"
                class="
                  w-full
                  border border-gray-500
                  rounded
                  p-2
                  text-black text-sm
                  focus:outline-none focus:border-blue-500 focus:shadow-sm
                "
                type="text"
              />
              <transition name="fade">
                <div v-if="errors.length > 0" class="text-left text-xs md:text-sm text-red-400" :class="classes">
                  {{ errors[0] }}
                </div>
              </transition>
            </ValidationProvider>
          </div>
          <div class="mb-6">
            <label class="block mb-1" for="email">이메일을 입력해주세요.</label>
            <ValidationProvider v-slot="{ errors, classes }" class="block" name="이메일" rules="required|email">
              <input
                id="email"
                v-model="profile.email"
                class="
                  w-full
                  border border-gray-500
                  rounded
                  p-2
                  text-black text-sm
                  focus:outline-none focus:border-blue-500 focus:shadow-sm
                "
                type="text"
              />
              <transition name="fade">
                <div v-if="errors.length > 0" class="text-left text-xs md:text-sm text-red-400" :class="classes">
                  {{ errors[0] }}
                </div>
              </transition>
            </ValidationProvider>
          </div>
          <div class="mb-6">
            <label class="block mb-1" for="phone">전화번호를 입력해주세요.</label>
            <ValidationProvider v-slot="{ errors, classes }" class="block" name="전화번호" rules="required">
              <input
                id="phone"
                v-model="profile.phone"
                class="
                  w-full
                  border border-gray-500
                  rounded
                  p-2
                  text-black text-sm
                  focus:outline-none focus:border-blue-500 focus:shadow-sm
                "
                type="text"
              />
              <transition name="fade">
                <div v-if="errors.length > 0" class="text-left text-xs md:text-sm text-red-400" :class="classes">
                  {{ errors[0] }}
                </div>
              </transition>
            </ValidationProvider>
          </div>
          <div class="mb-6">
            <label class="block mb-1" for="rank">계급을 입력해주세요.</label>
            <ValidationProvider v-slot="{ errors, classes }" class="block" name="계급" rules="required">
              <input
                id="rank"
                v-model="profile.rank"
                class="
                  w-full
                  border border-gray-500
                  rounded
                  p-2
                  text-black text-sm
                  focus:outline-none focus:border-blue-500 focus:shadow-sm
                "
                type="text"
              />
              <transition name="fade">
                <div v-if="errors.length > 0" class="text-left text-xs md:text-sm text-red-400" :class="classes">
                  {{ errors[0] }}
                </div>
              </transition>
            </ValidationProvider>
          </div>
          <div class="flex justify-end">
            <NuxtLink to="/profile" class="rounded bg-red-500 text-white py-2 px-4">취소</NuxtLink>
            <button type="submit" class="rounded bg-brand text-white py-2 px-4">수정</button>
          </div>
        </form>
      </ValidationObserver>
    </div>
  </main>
</template>

<script>
export default {
  middleware: 'auth',
  data() {
    return {
      profile: {
        name: this.$store.state.auth.user.name,
        nickname: this.$store.state.auth.user.nickname,
        email: this.$store.state.auth.user.email,
        phone: this.$store.state.auth.user.phone,
        rank: this.$store.state.auth.user.rank,
      },
    };
  },
  methods: {
    async submit() {
      try {
        await this.$api.profile.edit(this.profile);
        this.$toast.success('내 정보를 수정했습니다!');
        this.$router.replace('/profile');
      } catch (e) {
        const errorType = e.response.data.error;
        if (errorType === 'EXISTING_EMAIL') {
          this.$toast.error('이미 사용중인 이메일입니다!');
        } else if (errorType === 'EXISTING_SOLDIER_ID') {
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

<style lang="scss" scoped></style>
