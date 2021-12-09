<template>
  <div class="contents">
    <div class="form-wrapper form-wrapper-sm">
      <form @submit.prevent="submitForm" class="form">
        <div style="display: flex; align-items: center; flex-direction: row">
          <label for="username" style="font-size: 10px; margin-right: 10px">아이디 </label>
          <input type="text" id="username" v-model="username" style="width: 3800px; margin-right: 40px" />
          <p class="validation-text">
            <span class="warning" v-if="!isUsernameValid && username || username == ''" >
<!--              이메일을 입력해주세요.-->
              <img src="@/assets/warning.png" style="width: 25px; height: 25px;" />
            </span>
            <span v-else class="success">
              <img src="@/assets/success.png" style="width:25px; height: 25px;" />
            </span>
          </p>
        </div>
        <div style="display: flex; align-items: center; flex-direction: row">
          <label for="password" style="font-size: 10px; margin-right: 10px">비밀번호 </label>
          <input type="text" id="password" v-model="password" style="width: 4000px; margin-right: 40px" />
          <p class="validation-text">
            <span class="warning" v-if="!password">
<!--              비밀번호를 입력해주세요.-->
              <img src="@/assets/warning.png" style="width: 25px; height: 25px;" />
            </span>
            <span v-else class="success">
              <img src="@/assets/success.png" style="width:25px; height: 25px;" />
            </span>
          </p>
        </div>
        <div style="display: flex; align-items: center; flex-direction: row">
          <label for="nickname" style="font-size: 10px; margin-right: 10px">닉네임 </label>
          <input type="text" id="nickname" v-model="nickname" style="width: 4000px; margin-right: 40px" />
          <p class="validation-text">
            <span class="warning" v-if="!nickname" style="width: 25px; height: 25px;" >
<!--              닉네임을 입력해주세요.-->
              <img src="@/assets/warning.png" style="width: 25px; height: 25px;" />
            </span>
            <span v-else class="success">
              <img src="@/assets/success.png" style="width:25px; height: 25px;" />
            </span>
          </p>
        </div>
        <div style="display: flex; flex-direction: column">
          <button v-bind:disabled="!isUsernameValid || !password || !nickname" type="submit" class="btn" style="margin-bottom: 2px; background: #e5e5e5; font-weight: 400">
            회원가입
          </button>
          <button type="reset" class="btn" style="background: #797974; font-weight: 400;">취소</button>
        </div>
          <template v-if="submitForm">
            <p class="log">{{ logMessage }}</p>
          </template>
          <template v-else>
            <p class="log">{{ failMessage }}</p>
          </template>
<!--        <p class="log">{{ logMessage }}</p>-->
<!--        <p class="log" :class="{'warn': logMessage === '회원가입에 실패했습니다.'}"></p>-->
      </form>
    </div>
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
      failMessage: '회원가입에 실패했습니다.',
    };
  },
  methods: {
    async submitForm() {
      try {
        const userData = {
          username: this.username,
          password: this.password,
          nickname: this.nickname,
        };
        const response = await registerUser(userData);
        console.log(response.data.username);
        this.logMessage = `${response.data.username}님이 가입되었습니다.`;
      } catch (error) {
        console.log(error.response);
        // this.logMessage = '회원가입에 실패했습니다.';
      } finally {
        this.initForm();
      }
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

<style>
.warn {
  color: tomato;
}
</style>
