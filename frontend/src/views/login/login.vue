 <template>
  <div id="app">
    <div class="flex justify-center items-center h-screen ">
      <div class="h-full text-gray-800">
        <div
          class="flex xl:justify-center lg:justify-between justify-center items-center flex-wrap h-full g-6"
        >
          <div
            class="grow-0 shrink-1 md:shrink-0 basis-auto xl:w-6/12 lg:w-6/12 md:w-9/12 mb-12 md:mb-0"
          >
            <img
              src="https://cdn.discordapp.com/attachments/958256273592307722/964466329652441158/unknown.png"
              class="w-full"
              alt="Sample image"
            />
          </div>
          <div class="mt-10 px-24 w-5/12">
            <div class="flex gap-3">
              <h2
                class="text-center text-4xl    font-display font-semibold lg:text-left xl:text-5xl
                          xl:text-bold"
              >
                Sign in
              </h2>
            </div>
            <div class="mt-12">
              <div>
                <div>
                  <div class="text-sm font-bold text-gray-700 tracking-wide">
                    Username
                  </div>
                  <input
                    v-model="$v.username.$model"
                    class="w-full text-lg py-2 border-b border-gray-300 focus:outline-none focus:border-indigo-500"
                    :class="{'border-red-500' : $v.username.$error}"
                    type=""
                    placeholder="Enter your Username"
                  />
                  <p v-if="$v.username.$error && !$v.username.required" class="text-red-500 mt-1">required</p>
                </div>
                <div class="mt-8">
                  <div class="flex justify-between items-center">
                    <div class="text-sm font-bold text-gray-700 tracking-wide">
                      Password
                    </div>
                  </div>
                  <input
                    v-model="$v.password.$model"
                    class="w-full text-lg py-2 border-b border-gray-300 focus:outline-none focus:border-indigo-500"
                    :class="{'border-red-500' : $v.password.$error || error}"
                    type="password"
                    placeholder="Enter your password"
                  />
                  <p v-if="$v.password.$error && !$v.password.required" class="text-red-500 mt-1">required</p>
                  <p v-show="error" class="text-red-500 pt-1 mt-1">Username or Password Incorrect</p>
                </div>
                <div class="float-right">
                  <router-link to="/forgetpassword" class="text-xs font-display font-semibold text-indigo-600 hover:text-indigo-800 cursor-pointer">Forgot Password?</router-link>
                </div>
                <div class="mt-10">
                  <button
                    @click="login()"
                    class="bg-indigo-500 text-gray-100 p-4 w-full rounded-full tracking-wide font-semibold font-display focus:outline-none focus:shadow-outline hover:bg-indigo-600 shadow-lg"
                  >
                    Sign In
                  </button>
                </div>
              </div>
              <div
                class="mt-8 text-sm font-display font-semibold text-gray-700 text-center items-center flex flex-col"
              >
                <router-link to="/register">
                  Don't have an account ?
                  <a
                    class="cursor-pointer text-indigo-600 hover:text-indigo-800"
                    >Sign up</a
                  >
                </router-link>
                <router-link class="mt-5" to="/">
                  <img width="50px" src="https://cdn.discordapp.com/attachments/958256273592307722/971440161411964978/home_new.png" alt="">
                </router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {required} from 'vuelidate/lib/validators'
import axios from "axios";
// @ is an alias to /src
export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
      user: [],
      error: false
    };
  },
  validations:{
    username:{
      required
    },
    password:{
      required
    }
  },
  methods: {
    login() {
      if(this.$v.$invalid){
        this.$v.$touch()
      }else{
        axios
          .post(`http://localhost:3000/connected`, {
            username: this.username,
            password: this.password
          })
          .then(response => {
            this.user = response.data;
            if (this.user != "error") {
              localStorage.setItem("user", JSON.stringify(this.user));
              this.toindex();
            } else {
              this.error = true;
            }
          })
          .catch(error => {
            this.error = error.response.data.message;
          });
      }
    },
    toindex(){
      this.$router.push("/");
    }
  }
};
</script>
