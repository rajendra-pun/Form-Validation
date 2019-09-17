<template>
  <div class="card">
    <h3 class="card-header text-center">Register Area</h3>
    <div class="card-body">
      <form @submit.prevent="submitForm">
        <div class="form-row">
          <div class="form-group col-md-6">
            <label>First Name</label>
            <input
              type="text"
              class="form-control"
              v-model.trim="$v.firstname.$model"
              :class="{'is-invalid':$v.firstname.$error, 'is-valid':!$v.firstname.$invalid}"
            />
            <div class="valid-feedback">Your name is valid!</div>
            <div class="invalid-feedback">
              <span v-if="!$v.firstname.required">First name is required.</span>
              <span
                v-if="!$v.firstname.minLength"
              >Frist name must have at least {{ $v.firstname.$params.minLength.min }} letters.</span>
              <span
                v-if="!$v.firstname.maxLength"
              >Frist name must have at most {{ $v.firstname.$params.maxLength.max }} letters.</span>
            </div>
          </div>
          <div class="form-group col-md-6">
            <label>Last Name</label>
            <input
              type="text"
              class="form-control"
              v-model.trim="$v.lastname.$model"
              :class="{'is-invalid':$v.lastname.$error, 'is-valid':!$v.lastname.$invalid}"
            />
            <div class="valid-feedback">Your name is valid!</div>
            <div class="invalid-feedback">
              <span v-if="!$v.lastname.required">Last name is required.</span>
              <span
                v-if="!$v.lastname.minLength"
              >Last name must have at least {{ $v.lastname.$params.minLength.min }} letters.</span>
              <span
                v-if="!$v.lastname.maxLength"
              >Last name must have at most {{ $v.lastname.$params.maxLength.max }} letters.</span>
            </div>
          </div>
          <div class="form-group col-12">
            <label>Username</label>
            <input
              type="text"
              class="form-control"
              v-model.trim="$v.username.$model"
              :class="{'is-invalid':$v.username.$error, 'is-valid':!$v.username.$invalid}"
            />
            <div class="valid-feedback">Your username is valid!</div>
            <div class="invalid-feedback">
              <span v-if="!$v.username.required">Username is required.</span>
              <span v-if="!$v.username.isUnique">This Username is already registered.</span>
            </div>
          </div>
          <div class="form-group col-12">
            <label>Email</label>
            <input
              type="email"
              class="form-control"
              v-model.trim="$v.email.$model"
              :class="{'is-invalid':$v.email.$error, 'is-valid':!$v.email.$invalid}"
            />
            <div class="valid-feedback">Your email is valid!</div>
            <div class="invalid-feedback">
              <span v-if="!$v.email.required">Email is required.</span>
              <span v-if="!$v.email.isUnique">This Email is wrong or already registered.</span>
            </div>
          </div>
          <div class="form-group col-12">
            <label>Password</label>
            <input
              type="password"
              id="password"
              class="form-control"
              v-model.trim="$v.password.$model"
              :class="{'is-invalid':$v.password.$error, 'is-valid':!$v.password.$invalid}"
              placeholder="password"
            />
            <div class="valid-feedback">Your password is valid!</div>
            <div class="invalid-feedback">
              <span v-if="!$v.password.required">Password is required.</span>
              <span
                v-if="!$v.password.minLength"
              >{{ $v.password.$params.minLength.min}}characters minimum.</span>
            </div>
          </div>
          <div class="form-gropu form-check">
            <input
              type="checkbox"
              id="showpassword"
              class="form-check-input"
              @click="toggleShowPassword"
              v-model="showpassword"
            />
            <label class="form-check-label" for="showpassword">show password</label>
          </div>
          <div class="form-group col-12">
            <label>Repeat Password</label>
            <input
              type="password"
              class="form-control"
              v-model.trim="$v.repeatpassword.$model"
              :class="{'is-invalid':$v.repeatpassword.$error, 'is-valid':(password != '') ? !$v.repeatpassword.$invalid: ''}"
              placeholder="repeat password"
            />
            <div class="valid-feedback">Your password is valid!</div>
            <div class="invalid-feedback">
              <span v-if="!$v.repeatpassword.sameAsPassword">Passwords must be identical.</span>
            </div>
          </div>
          <div class="form-group col-12">
            <label>Phone Number</label>
            <input
              type="number"
              class="form-control"
              v-model.trim="$v.phone.$model"
              :class="{'is-invalid':$v.phone.$error, 'is-valid':!$v.phone.$invalid}"
            />
            <div class="valid-feedback">Your phone is valid!</div>
            <div class="invalid-feedback">
              <span v-if="!$v.phone.required">Phone is required.</span>
              <span v-if="!$v.phone.numeric">This phone is wrong or already registered.</span>
            </div>
            <button type="submit" class="btn btn-success">Submit {{ submitstatus }}</button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
// repeatpassword = sameAs ho, numeric = phone, url = url
import {
  required,
  minLength,
  maxLength,
  between,
  sameAs,
  numeric,
  url
} from "vuelidate/lib/validators";
// import { resolve } from 'dns';
export default {
  name: "FormValidation",
  data() {
    return {
      firstname: "",
      lastname: "",
      username: "",
      email: "",
      password: "",
      repeatpassword: "",
      showpassword: false,
      phone: "",
      url: "",
      submitstatus: null
    };
  },
  validations: {
    firstname: {
      required,
      minLength: minLength(3),
      maxLength: maxLength(10)
    },
    lastname: {
      required,
      minLength: minLength(5),
      maxLength: maxLength(12)
    },
    username: {
      required,
      isUnique(value) {
        if (value === "") return true;

        return new Promise(resolve => {
          setTimeout(() => {
            resolve(typeof value === "string" && value.length % 2 !== 0);
          }, 350 + Math.random() * 300);
        });
      }
    },
    email: {
      required,
      // email,
      isUnique(value) {
        if (value === "") return true;

        //eslint-disable-next-line
        var email_regex = /(^$|^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$)/;
        return new Promise(resolve => {
          setTimeout(() => {
            // resolve (typeof value === 'string' && value.length %2 !== 0)
            resolve(email_regex.test(value));
          }, 350 + Math.random() * 300);
        });
      }
    },
    password: {
      required,
      minLength: minLength(8)
    },
    repeatpassword: {
      sameAsPassword: sameAs("password")
    },
    phone:{
        required,
        numeric,
        minLength:minLength(10)
    },
    url:{
        required,
        url
    }
  },

  methods: {
    toggleShowPassword() {
      var show = document.getElementById("password");
      if (this.showpassword == false) {
        this.showpassword = true;
        show.type = "text"; //if checkbox with id="password" click vo bhne password as text dekhauxa
      } else {
        this.showpassword = false;
        show.type = "password"; // but if we dont select then its type will be password and willbe hidden password
      }
    },
    submitForm(){
        this.$v.$touch()
        if(this.$v.$invalid){
            this.submitstatus = "FAIL"
        } else{
            this.submitstatus = "SUCCESS"
        }
    }
  }
};
</script>
<style>
</style>