<template>
  <div>
    <form @submit.prevent="submitForm">
      <div>
        <label for="username">id: </label>
        <input type="text" id="username" v-model="username" />
      </div>
      <div>
        <label for="password">pw: </label>
        <input type="text" id="password" v-model="password" />
      </div>
      <div>
        <label for="nickname">nickname: </label>
        <input type="text" id="nickname" v-model="nickname" />
      </div>
      <button v-bind:disabled="!isUsernameValid || !password || !nickname" type="submit">
        회원가입
      </button>
      <button type="reset">취소</button>
      <p>{{ logMessage }}</p>
    </form>
  </div>
</template>

<script>
import { registerUser } from '@/api/index';
import { validateEmail } from '@/utils/validation';

export default {
  name: 'SignUpForm',
  data() {
    return {
      username: '',
      password: '',
      nickname: '',
      logMessage: '',
    };
  },
  methods: {
    async submitForm() {
      console.log('submit');
      const userData = {
        username: this.username,
        password: this.password,
        nickname: this.nickname,
      };
      const response = await registerUser(userData);
      console.log(response.data.username);
      this.logMessage = `${response.data.username}님이 가입되었습니다.`;
      this.initForm();
    },
    initForm() {
      this.username = '';
      this.password = '';
      this.nickname = '';
    },
  },
  computed: {
    isUsernameValid() {
      return validateEmail(this.username);
    },
  },
};
</script>
