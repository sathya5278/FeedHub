<template>
  <div class="col-md-12">
    <div class="card card-container">
      <h3 class="text-center p-2 mb-2 font-weight-bold">Login</h3>
      <img id="profile-img" src="../assets/user.png" class="profile-img-card" />
      <form name="form" @submit.prevent="userLogin">
        <div class="form-group">
          <label for="username">Email address</label>
          <div class="input-group">
            <div class="input-group-prepend">
              <span class="input-group-text"
                ><i class="fas fa-envelope"></i
              ></span>
            </div>
            <input v-model="user.email" type="email" class="form-control" />
          </div>
        </div>
        <div class="form-group">
          <label for="password">Password</label>
          <div class="input-group">
            <div class="input-group-prepend">
              <span class="input-group-text"><i class="fas fa-lock"></i></span>
            </div>
            <input
              v-model="user.password"
              type="password"
              class="form-control"
            />
          </div>
        </div>
        <div class="form-group">
          <button class="btn btn-primary btn-block">
            Login
          </button>
        </div>
        <div class="form-group">
          <p class="forgot-password text-right mt-2 mb-4">
            <router-link to="/forgot-password">Forgot password ?</router-link>
          </p>
        </div>
      </form>
      <button
        @click="googleLogin"
        class="btn btn-outline-dark btn-lg btn-block"
      >
        <img
          width="20px"
          style="margin-bottom:3px; "
          alt="Google sign-in"
          src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/Google_%22G%22_Logo.svg/512px-Google_%22G%22_Logo.svg.png"
        />
        Sign In With Google
      </button>
      <button
        @click="microsoftLogin"
        class="btn btn-outline-dark btn-lg btn-block"
      >
        <img
          width="20px"
          style="margin-bottom:3px; "
          alt="Microsoft sign-in"
          src="../assets/MSlogo.svg"
        />
        Sign In With Microsoft
      </button>
    </div>
  </div>
</template>

<script>
import firebase from "firebase";

export default {
  data() {
    return {
      user: {
        email: "",
        password: "",
      },
    };
  },
  methods: {
    userLogin() {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.user.email, this.user.password)
        .then(() => {
          this.$router.push("/dashboard");
        })
        .catch((error) => {
          alert(error.message);
        });
    },
    googleLogin() {
      const provider = new firebase.auth.GoogleAuthProvider();
      firebase
        .auth()
        .signInWithRedirect(provider)
        .then(function() {})
        .catch((error) => {
          alert(error);
        });

      firebase
        .auth()
        .getRedirectResult()
        .then((result) => {
          // This gives you a Google Access Token. You can use it to access the Google API.
          //    var token = result.credential.accessToken;

          // The signed-in user info.
          //   var user = result.user;
          console.log("user object" + result.user);

          //console.log(result);

          this.$router.push("/dashboard");
        })
        .catch((error) => {
          // Handle Errors here.
          //var errorCode = error.code;
          //   var errorMessage = error.message;

          // The email of the user's account used.
          //  var email = error.email;

          // The firebase.auth.AuthCredential type that was used.
          // var credential = error.credential;

          alert(
            error.code +
              " " +
              error.message +
              " " +
              error.email +
              " " +
              error.credential
          );
        });
    },
    microsoftLogin() {
      const provider = new firebase.auth.OAuthProvider("microsoft.com");
      firebase
        .auth()
        .signInWithRedirect(provider)
        .then(function() {})
        .catch((error) => {
          alert(error);
        });

      firebase
        .auth()
        .getRedirectResult()
        .then((result) => {
          // IdP data available in result.additionalUserInfo.profile.
          // ...

          /** @type {firebase.auth.OAuthCredential} */
          var credential = result.credential;

          // OAuth access and id tokens can also be retrieved:
          console.log("user credential", credential);

          this.$router.push("/dashboard");
        })

        .catch((error) => {
          // Handle Errors here.
          //var errorCode = error.code;
          //   var errorMessage = error.message;

          // The email of the user's account used.
          //  var email = error.email;

          // The firebase.auth.AuthCredential type that was used.
          // var credential = error.credential;

          alert(
            error.code +
              " " +
              error.message +
              " " +
              error.email +
              " " +
              error.credential
          );
        });
    },
  },
};
</script>
<style scoped>
label {
  display: block;
  margin-top: 10px;
}

.card-container.card {
  max-width: 350px !important;
  padding: 20px 20px;
}

.card {
  background-color: #f7f7f7;
  padding: 20px 25px 30px;
  margin: 0 auto 25px;
  margin-top: 50px;
  -moz-border-radius: 2px;
  -webkit-border-radius: 2px;
  border-radius: 2px;
  -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
}

.profile-img-card {
  width: 96px;
  height: 96px;
  margin: 0 auto 10px;
  display: block;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
}
</style>
