<template>
  <v-container grid-list-md>
    <v-row align="center" justify="center">
      <v-col class="align-center">
        <v-row class="align-center" justify="center">
          <v-col cols="12" sm="10" md="10" lg="6" class="align-center">
          </v-col>
        </v-row>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="10" md="10" lg="6" class="align-center">
            <v-card class="login-card">
              <v-card-title>
                <span class="headline">Sign Up</span>
              </v-card-title>
              <v-spacer/>

              <v-card-text>
                <v-form ref="form"  v-model="validation" lazy-validation>
                  <v-container>

                    <v-text-field
                    v-model="user.username"
                    label="Username"
                    :counter="70"
                    :rules="validationRules.username"
                    maxlength="70"
                    required
                    ></v-text-field>

                    <v-text-field
                     :type="'Password'"
                    v-model="user.password"
                    label="Password"
                    :rules="validationRules.password"
                    :counter="20"
                    maxlength="20"
                    required
                    />

                    <v-text-field
                    v-model="user.email"
                    label="Email"
                    :rules="validationRules.email"
                    maxlength="70"
                    required
                    />

                    <v-text-field
                    v-model="user.first_name"
                    label="First Name"
                    :rules="validationRules.firstname"
                    maxlength="50"
                    required
                    />

                    <v-text-field
                    v-model="user.last_name"
                    label="Last Name"
                    :rules="validationRules.lastname"
                    maxlength="50"
                    required
                    />

                </v-container>
                <v-btn class="blue white--text" :disabled="!validation" @click="signup">Submit </v-btn>
                <v-btn class="white black--text" @click="cancelOperation">Cancel</v-btn>
                </v-form>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
  import router from '../router';
  import {APIService} from '../http/APIService';
  const apiService = new APIService();

  export default {
    name: 'Signup',
    components: {},
    data() {
      return {
        user: {},
        validation: true,
        validationRules: {
        username: [userRes => !!userRes || "Username is required"],
        password: [passRes => !!passRes || "Password is required"],
        email:[ emailRes => !!emailRes || "Email is required"],
        firstname:[firstRes => !!firstRes || "FirstName is required"],
        lastname:[lastRes => !!lastRes || "LastName is required"],
        }
       };
    },
    methods: {
      signup() {
         if (this.$refs.form.validate()) {
            apiService.signupUser(this.user).then(response => {
            this.user = response.data;
            this.showMsg = "";
            router.push("/auth");
          }).catch(e => {
           this.showMsg = 'error';
           this.errormsg = e.response.data
          })

      }
      },
      cancelOperation(){
         router.push("/");
      },
    },

  }
</script>
<style scoped>
  .aform {
    margin-left: auto;
    width: 60%;
  }
</style>
