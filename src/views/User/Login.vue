<template>
  <div class="main">
    <div class="loaderContainer" v-if="isLoading">
      <hollow-dots-spinner
          :animation-duration="1000"
          :dot-size="15"
          :dots-num="3"
          color="#ff1d5e"
          class="loader"
      />
    </div>
    <div>
      <div class="loginwith">
        <h4>Login</h4>
        <div class="loginopt my-4">
          <router-link to="/Dashboard/0/" class="btn btn-link">
            <facebook-icon></facebook-icon>
            Facebook
          </router-link>

          <router-link to="/Dashboard/0/" class="btn btn-link">
            <twitter-icon></twitter-icon>
            Twitter
          </router-link>

          <router-link to="/Dashboard/0/" class="btn btn-link">
            <google-icon></google-icon>
            Google
          </router-link>
        </div>
      </div>
      <div class="spacer"></div>
      <div class="login-form">
        <div class="form-group">
          <textinp name="username" type="text" v-model="Username" placeholder="Username"></textinp>
        </div>
        <div class="form-group">
          <textinp name="password" type="password" v-model="Password" placeholder="Password"></textinp>
        </div>
        <div class="form-group form-check">
          <input type="checkbox" class="form-check-input" id="KeepSignedIn"/>
          <label class="form-check-label" for="KeepSignedIn">Remember me</label>
        </div>
        <button class="btn btn-outline-light flex-end" v-on:click="login">Login</button>
      </div>
      <div class="alternate">
        <div class="forgot">
          <router-link to="/User/Forget/" class="btn btn-link">Forgot Username?</router-link>
          <router-link to="/User/Forget/" class="btn btn-link">Forgot password?</router-link>
        </div>
        <router-link to="/User/Register/" class="btn btn-link" style="width:100px; margin:auto">Sign Up</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import textinp from "@/components/TextInputGroup"
import {FacebookIcon, GoogleIcon, TwitterIcon} from 'vue-simple-icons'
import {HollowDotsSpinner} from 'epic-spinners'

export default {
  name: "Login",
  data() {
    return {
      Username: "",
      Password: "",
      isLoading: false
    }
  },
  components: {
    textinp,
    FacebookIcon,
    TwitterIcon,
    GoogleIcon,
    HollowDotsSpinner
  },
  methods: {
    login() {
      this.isLoading = true;
      fetch("https://speedwagonmailback.herokuapp.com/account", {
        method: "POST",
        body: JSON.stringify({
          usern: this.Username,
          pass: this.Password
        }),
        headers: {
          "content-type": "application/json"
        }
      }).then(response => response.json()
      ).then(result => {
        if (result.status == "OK" && result.content != null) {
          localStorage.user = JSON.stringify(result.content);
          this.$store.commit("changeuser")
          this.isLoading = false
          this.$router.push("/dashboard")
        } else {
          console.log(result)
        }
      }).finally(() => {
        this.isLoading = false
      })
    }
  },
}
</script>

<style scoped lang=scss>
.flex-end {
  align-self: center;
}

.main {
  display: flex;
  flex-direction: column;
  height: 580px;
  margin: auto;
  padding: 10px;
  border: solid 1px rgba($color: gray, $alpha: 0.7);
  border-radius: 10px;
  background-color: $brown-400;
  color: white;
  position: relative;

  .loader {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .loaderContainer {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background: rgba(black, 0.2);
  }

  input {
    background-color: rgba($color: white, $alpha: 0.8);
  }

  .btn-link {
    color: $gray-100;
  }

  .loginwith {
    margin-top: 10px;
    text-align: center;

    .loginopt {
      display: flex;
      width: 100%;
      justify-content: space-around;
    }
  }

  .loginform {
    display: flex;
    flex-direction: column;
  }

  .alternate {
    /*margin-top: 20px;*/
    margin-top: auto;
    display: flex;
    flex-direction: column;

    .forgot {
      display: flex;
      align-self: center;
      justify-content: center;
      width: 100%;
    }

    .btn {
      font-size: small;
    }
  }
}

</style>