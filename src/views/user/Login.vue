<template>
  <div class="login-background">
    <div class="login-container">
      <h2>로그인</h2>
      <form @submit.prevent="submitLogin">
        <div class="form-group">
          <label for="email"></label>
          <input type="email" id="email" v-model="loginRequest.email" required placeholder="이메일 주소">
        </div>
        <div class="form-group">
          <label for="password"></label>
          <input type="password" id="password" v-model="loginRequest.password" required placeholder="비밀번호">
        </div>
        <div class="login-button-container">
          <button type="submit" class="login-button">Login</button>
        </div>
        <div class="sub-login-links">
          <div class="email-find-link">
            <a href="#" class="email-find-link" @click.prevent="openSearchEmailPopup">이메일 찾기</a>
          </div>
          <span class="separator">|</span>
          <div class="forgot-password-link">
            <a href="#" class="forgot-password-link" @click.prevent="openReissuePasswordPopup">비밀번호 찾기</a>
          </div>
          <span class="separator">|</span>
          <div class="sign-up-link">
            <router-link to="/terms-of-service" class="terms-of-service-link">회원가입</router-link>
          </div>
        </div>
        <div class="social-login-buttons">
          <button class="google-login-button"><img src="@/assets/img/GOOGLE.png" class="google" alt="Google Logo"></button>
          <button class="kakao-login-button"><img src="@/assets/img/KAKAO.png" class="kakao" alt="Kakao Logo"></button>
        </div>
      </form>
      <ReissuePasswordPopup v-if="showReissuePasswordPopup" @close="closeReissuePasswordPopup"/>
      <SearchEmailPopup v-if="showSearchEmailPopup" @close="closeSearchEmailPopup"/>
    </div>
  </div>
</template>

<script>
import ReissuePasswordPopup from "../../components/user/ReissuePasswordPopup.vue";
import SearchEmailPopup from "@/components/user/SearchEmailPopup.vue";
import TermsOfService from "@/components/user/TermsOfService.vue";
import { apiClient } from "@/index";
import { mapMutations } from "vuex";
// import axios from "axios";

export default {
  data() {
    return {
      loginRequest: {
        email: "",
        password: "",
      },
      showReissuePasswordPopup: false,
      showSearchEmailPopup: false,
      showTermsOfServide: false,
    };
  },
  components: {
    ReissuePasswordPopup,
    SearchEmailPopup,
    TermsOfService,
  },
  methods: {
    ...mapMutations(['login']),
    async submitLogin() {
      // 로그인 요청 로직을 여기에 구현
      const response = await apiClient.post("/api/users/login", {
        email: this.loginRequest.email,
        password: this.loginRequest.password,
      })
      console.log(response)
      const token = await response.data.accessToken;
      await localStorage.setItem("accessToken", token);
      console.log(token)
      const userInfoResponse = await apiClient.get("/api/users/myinfo", {
        headers: {
          Authorization: `Bearer ${localStorage.getItem("accessToken")}`,
        },
      });
      console.log("회원정보 : ", userInfoResponse);
      await this.$router.push("/");
      await this.login();
    },
    closeReissuePasswordPopup() {
      this.showReissuePasswordPopup = false;
    },
    openReissuePasswordPopup() {
      this.showReissuePasswordPopup = true;
    },
    closeSearchEmailPopup() {
      this.showSearchEmailPopup = false;
    },
    openSearchEmailPopup() {
      this.showSearchEmailPopup = true;
    },
  },
};
</script>

<style src="../../assets/css/user/Login.css" lang="css"></style>index