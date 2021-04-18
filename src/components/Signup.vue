<template>
  <v-container grid-list-md>
    <v-row align="center" justify="center">
      <v-col class="align-center">
        <v-row class="align-center" justify="center">
          <v-col cols="12" sm="10" md="10" lg="6" class="align-center">
            <v-alert v-if="showMsg === 'error'"
                     dismissible
                     :value="true"
                     type="error"
            >
              {{errormsg}}
            </v-alert>
          </v-col>
        </v-row>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="10" md="10" lg="6" class="align-center">
            <v-card class="login-card">
              <v-card-title>
                <span class="headline">{{pageTitle}}</span>
              </v-card-title>
              <v-spacer/>
 
              <v-card-text>
                <v-form ref="form"  v-model="valid" lazy-validation>
                  <v-container>
 
                    <v-text-field
                    v-model="user.username"
                    label="Username"
                    :counter="70"
                    :rules="rules.username"
                    maxlength="70"
                    required
                    ></v-text-field>
 
                    <v-text-field
                    v-model="user.email"
                    label="Email"
                    :rules="rules.email"
                    maxlength="70"
                    required
                    />
                    <v-text-field
                    v-model="user.first_name"
                    label="First Name"
                    :rules="rules.firstname"
                    maxlength="50"
                    required
                    />
                    <v-text-field
                    v-model="user.last_name"
                    label="Last Name"
                    :rules="rules.lastname"
                    maxlength="50"
                    required
                    />
                    <v-text-field
                     :type="showPassword ? 'text' : 'Password'"
                    v-model="user.password"
                    label="Password"
                    :rules="rules.password"
                    :counter="20"
                    maxlength="20"
                    :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                  @click:append="showPassword = ! showPassword"
                    required
 
                    />
                    <v-text-field
                     :type="showPassword1 ? 'text' : 'Password'"
                    v-model="user.password2"
                    label="Reenter Password"
                    :rules="rules.password2"
                    :counter="20"
                    maxlength="20"
                    :append-icon="showPassword1 ? 'mdi-eye' : 'mdi-eye-off'"
                  @click:append="showPassword1 = ! showPassword1"
                    required
                    />
                   
 
                </v-container>
                <v-btn class="blue white--text" :disabled="!valid" @click="signup">Submit </v-btn>
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
       
        showError: false,
        user: {},
        pageTitle: "Sign Up",
        valid: true,
        showMsg: '',
        rules: {
        username: [
          v => !!v || "Username is required",
          v => (v && v.length > 3) || "A username must be more than 3 characters long",
          v => /^[a-z0-9_]+$/.test(v) || "A username can only contain letters and digits"
        ],
        password: [
          v => !!v || "Password is required",
          v => (v && v.length > 7) || "The password must be longer than 7 characters"
        ],
        password2: [
          v => !!v || "Password is required",
          v => v === this.user.password || "Passwords do not match",
          v => (v && v.length > 7) || "The password must be longer than 7 characters"
        ],
        email:[  
            v => !!v || "Email is required",
            v => /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(v) || "Invalid e-mail."    
        ],
        firstname:[  
            v => !!v || "FirstName is required",
            v => (v && v.length > 3) || "FirstName must be more than 3 characters long",
              
        ],
        lastname:[  
            v => !!v || "LastName is required",
            v => (v && v.length > 3) || "LastName must be more than 3 characters long",
               
        ],
       
      },
      showPassword: false,
      showPassword1: false,
       };
    },
    methods: {
      signup() {
         if (this.$refs.form.validate()) {
            apiService.registerUser(this.user).then(response => {
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
