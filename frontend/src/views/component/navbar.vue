<template>
  <div id="app">
    <nav
      class="flex items-center px-12 py-4 justify-between shadow-lg"
      v-if="tokens == null"
    >
      <div class="space-x-6 flex items-center font-bold">
        <router-link class="text-xl font-bold" to="/"><img width="110px" src="https://cdn.discordapp.com/attachments/958256273592307722/964954378614210680/logo3.png" alt=""></router-link>
        <router-link class="text-sky-700" to="/searchcar">ค้นหารถ</router-link>
        <router-link class="text-sky-700" to="/seller">ขายรถยนต์</router-link>
        <router-link class="text-sky-700" to="/about">เกี่ยวกับ</router-link>
        <router-link class="text-sky-700" to="/question">คำถามพบบ่อย</router-link>
      </div>
      <div class="lg:flex items-center space-x-6 hidden">
        <div class="flex items-center space-x-2 gap-2">
          <router-link class="card-footer-item text-sky-700 font-bold" to="/login"
            >เข้าสู่ระบบ/สมัครสมาชิก</router-link
          >
          <img width="25px" src="https://cdn.discordapp.com/attachments/958256273592307722/968926310451798036/user.png" alt="" />
        </div>
      </div>
    </nav>
    <nav
      class="flex items-center px-12 py-4 justify-between shadow-lg"
      v-else
    >
      <div class="space-x-6 flex items-center">
        <router-link class="text-xl font-bold" to="/"><img width="110px" src="https://cdn.discordapp.com/attachments/958256273592307722/964954378614210680/logo3.png" alt=""></router-link>
        <div class="inline-block relative">
          <button @click="navbrand = navbrand ? false : true" class="inline-flex items-center gap-1">
            <router-link class="text-sky-700 font-bold" to="/searchcar">ค้นหารถ</router-link>
          </button>
        </div>
        <router-link class="text-sky-700 font-bold" to="/seller" v-show="loginuser.role != 'admin'">ขายรถยนต์</router-link>
        <router-link class="text-sky-700 font-bold" to="/about">เกี่ยวกับ</router-link>
        <router-link class="text-sky-700 font-bold" to="/question">คำถามพบบ่อย</router-link>
        <router-link class="text-sky-700 font-bold" to="/manageseller" v-show="loginuser.role == 'admin'">Manage Seller</router-link>
      </div>
      <div class="lg:flex items-center space-x-6 hidden">
        <div class="flex items-center space-x-2">
          <div class="">
            <div class="dropdown inline-block relative">
              <button
                class="text-gray-700 font-semibold rounded inline-flex items-center gap-2"
              >
                <span class="mx-3 text-sky-700">{{ loginuser.user_firstname }} {{loginuser.user_lastname}}</span>
                <img width="25px" src="https://cdn.discordapp.com/attachments/958256273592307722/968926310451798036/user.png" alt="" />
              </button>
              <ul class="dropdown-menu absolute hidden text-gray-700 pt-1 w-full shadow-md rounded-lg z-50">
                <li class="">
                  <router-link class="" to="/profile">
                    <p class="bg-white hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap">
                    Profile
                    </p>
                  </router-link>
                  
                </li>
                <li class="" @click="tohistory()" v-if="this.loginuser.seller_type == 1">
                    <p class="bg-white hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap">
                    My Cars
                    </p>
                </li>
                <li class="" @click="tocussale()" v-if="this.loginuser.customer_type == 1 && this.loginuser.role != 'admin'">
                    <p class="bg-white hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap">
                    My Booking
                    </p>
                </li>
                <li class="" @click="tosellsale()" v-if="this.loginuser.seller_type == 1">
                    <p class="bg-white hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap">
                    Approve Cars
                    </p>
                </li>
                <li class="" @click="tomanagesale()" v-if="this.loginuser.role == 'admin'">
                    <p class="bg-white hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap">
                    Approve Cars
                    </p>
                </li>
                <li class="">
                  <p
                    class="bg-white hover:bg-gray-200 py-2 px-4 block whitespace-no-wrap rounded-b" @click="signout()"
                    >Sign out</p
                  >
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </nav>
    <a id="scrollup" class="hidden" onclick="backToTop()"><img width="50px" class="fixed right-12 bottom-16 z-10" src="http://seenual.com/wp-content/uploads/2016/03/1459083547_ChevronUpCircle.png" alt=""></a>
  </div>
</template>

<script>
import axios from "axios";
// @ is an alias to /src
export default {
  name: "Navbar",
  data() {
    return {
      loginuser: [],
      navbrand : false,
      tokens : JSON.parse(localStorage.getItem('user'))
    };
  },
  mounted() {
    this.getdata();
    
  },
  methods: {
    getdata() {
      let token = JSON.parse(localStorage.getItem('user'))
      axios
          .post(`http://localhost:3000/getuser`, {token : token})
          .then(response => {
            this.loginuser = response.data;
            // console.log(this.loginuser)
          })
          .catch(error => {
            this.error = error.response.data.message;
          });
    },
    signout(){
        localStorage.removeItem("user");
        location.reload();
    },
    tohistory(){
      this.$router.push(`/historyseller/${this.loginuser.user_id}`)
    },
    tocussale(){
      this.$router.push(`/cussalesdata/${this.loginuser.user_id}`)
    },
    tosellsale(){
      this.$router.push(`/sellersalesdata/${this.loginuser.user_id}`)
    },
    tomanagesale(){
      this.$router.push(`/managesalesdata/${this.loginuser.user_id}`)
    }
  },
};
</script>
