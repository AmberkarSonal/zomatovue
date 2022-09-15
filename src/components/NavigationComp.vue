

<template>
  <dialog open v-if="showForm" class="showOnTop">
    <the-auth-form @handle-submit="handleFormSubmit" />
  </dialog>
  <nav class="navbar navbar-dark bg-danger justify-content-between">
    <span class="navbar-text mx-3">Zomato Clone</span>
    <!-- <b-nav-text></b-nav-text> -->
    <div class="mx-3">
      <button v-if="!isLoggedIn" type="button" class="btn btn-outline-light"  @click="showLoginForm">Login</button>
      <!--
        <b-button v-if="!isLoggedIn" variant="light" @click="showLoginForm"
      >Login</b-button> -->
    <!--  -->
    <!-- <b-button v-if="isLoggedIn" variant="success" @click="logout">Logout
    </b-button
    > -->
    <h2 v-if="isLoggedIn">Welcome {{userDetails?.userName}}</h2>
    <button v-if="isLoggedIn" type="button" class="btn btn-link" @click="logout">Logout</button>
    </div>
  </nav>
</template>

<script>
import TheAuthForm from "./TheAuthForm.vue";
import { LOGIN } from "./../api/endpoints";
import { axiosInstance } from "./../api/axios";
import jwt_decode from "jwt-decode";
import { mapGetters, mapActions } from "vuex";
export default {
  components: {
    "the-auth-form": TheAuthForm,
  },
  data() {
    return {
      showForm: false,
      token:null,
      decodedToken:null,
    };
  },
  mounted(){
    this.$data.token = sessionStorage.getItem("token")
  },
  methods: {
    ...mapActions({
      updateLoginState: "authentication/updateLoginState",
      setAuthorization: "authentication/setAuthorization",
      setUserDetails: "authentication/setUserDetails",
    }),
    logout(){
      sessionStorage.clear();
      this.updateLoginState(false)
    },
    handleFormSubmit(authData) {
      axiosInstance.post(LOGIN, { ...authData }).then((res) => {
        if(res.result == 'success'){
          sessionStorage.setItem("token", res.data.token);
          console.log(res.data.token);
          const decodedToken = jwt_decode(res.data.token);
          console.log(decodedToken);
          this.$data.showForm= false
          this.updateLoginState(true)
          this.setAuthorization(decodedToken.authorizationDetails);
          this.setUserDetails(decodedToken.userDetails)
        }
      });
    },
    showLoginForm() {
      this.$data.showForm = true;
    },
  },
  computed: {
    ...mapGetters('authentication',['isLoggedIn','userDetails'])
  },
};
</script>

<style scoped>
.danger {
  background: red;
}
.showOnTop {
  z-index: 999;
}
</style>