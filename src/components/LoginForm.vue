<template>
  <div class="contents">
    <div class="form-wrapper form-wrapper-sm">
      <form @submit.prevent="submitForm" class="form">
        <div style="display: flex; align-items: center; flex-direction: row">
          <label for="username" style="font-size: 10px; margin-right: 10px">이메일 </label>
          <input type="text" id="username" v-model="username" style="width: 4000px; margin-right: 40px" />
          <p class="validation-text">
            <span class="warning" v-if="!isUsernameValid && username || username == ''" style="width: 25px; height: 25px;" >
<!--              올바른 이메일을 입력해주세요.-->
              <img src="@/assets/warning.png" style="width: 25px; height: 25px;" />
            </span>
            <span v-else class="success">
              <img src="@/assets/success.png" style="width:25px; height: 25px;" />
            </span>
          </p>
        </div>
        <div style="display: flex; align-items: center; flex-direction: row">
          <label for="password" style="font-size: 10px; margin-right: 10px">비밀번호 </label>
          <input type="password" id="password" v-model="password" style="width: 4000px; margin-right: 40px" />
          <p class="validation-text">
            <span class="warning" v-if="!password" style="width: 25px; height: 25px;">
<!--              비밀번호를 입력해주세요.-->
              <img src="@/assets/warning.png" style="width: 25px; height: 25px;" />
            </span>
            <span v-else class="success">
              <img src="@/assets/success.png" style="width:25px; height: 25px;" />
            </span>
          </p>
        </div>
        <div style="display: flex; flex-direction: column">
          <button v-bind:disabled="!isUsernameValid || !password" type="submit" class="btn" style="margin-bottom: 2px; background: #e5e5e5; font-weight: 400">
            로그인
          </button>
          <button disabled type="button" class="btn" style="background: #797974; font-weight: 400;"><a href="/signup">회원가입</a></button>
        </div>
        <!--    <button type="reset">취소</button>-->
        <div>
<!--          <p class="log">{{ logMessage }}</p>-->
          <p class="log" :class="{'warn': logMessage === '로그인에 실패했습니다.'}"></p>
        </div>

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
        const { data } = await loginUser(userData);
        // console.log(response.data.user.username);
        this.$store.commit('setUsername', data.user.username);
        this.$store.commit('setToken', data.token);
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

<style>
  .warn {
    color: tomato;
  }
</style>