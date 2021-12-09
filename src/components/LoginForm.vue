<template>
  <div class="contents">
    <div class="form-wrapper form-wrapper-sm">
      <form @submit.prevent="submitForm" class="form">
        <div>
          <label for="username">id: </label>
          <input type="text" id="username" v-model="username" />
          <p class="validation-text">
            <span class="warning" v-if="!isUsernameValid && username || username == ''">
              올바른 이메일을 입력해주세요.
            </span>
          </p>
        </div>
        <div>
          <label for="password">pw: </label>
          <input type="password" id="password" v-model="password" />
          <p class="validation-text">
            <span class="warning" v-if="!password">
              비밀번호를 입력해주세요.
            </span>
          </p>
        </div>
        <button v-bind:disabled="!isUsernameValid || !password" type="submit" class="btn">
          로그인
        </button>
        <button disabled type="button" class="btn"><a href="/signup">회원가입</a></button>
        <!--    <button type="reset">취소</button>-->
        <p class="log">{{ logMessage }}</p>
      </form>
    </div>
  </div>
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
        console.log(response.data.user.username);
        this.$router.push('/main');
        // this.logMessage = `${response.data.user.username}님이 로그인했습니다.`;
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
