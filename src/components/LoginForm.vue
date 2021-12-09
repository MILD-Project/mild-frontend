<template>
  <form @submit.prevent="submitForm">
    <div>
      <label for="username">id: </label>
      <input type="text" id="username" v-model="username" />
    </div>
    <div>
      <label for="password">pw: </label>
      <input type="password" id="password" v-model="password" />
    </div>
    <button v-bind:disabled="!isUsernameValid || !password" type="submit">
      로그인
    </button>
    <button disabled type="button"><a href="/signup">회원가입</a></button>
    <!--    <button type="reset">취소</button>-->
    <p>{{ logMessage }}</p>
  </form>
</template>

<script>
import { loginUser } from '@/api/index';
import { validateEmail } from '@/utils/validation';

export default {
  data() {
    return {
      username: '',
      password: '',
      logMessage: '',
    };
  },
  methods: {
    async submitForm() {
      try {
        const userData = {
          username: this.username,
          password: this.password,
        };
        const response = await loginUser(userData);
        // console.log(response);
        this.logMessage = `${response.data.user.username}님이 로그인했습니다.`;
      } catch (error) {
        console.log(error.response);
        this.logMessage = '로그인에 실패했습니다.';
      } finally {
        this.initForm();
      }
    },
    initForm() {
      this.username = '';
      this.password = '';
    },
  },
  computed: {
    isUsernameValid() {
      return validateEmail(this.username);
    },
  },
};
</script>
