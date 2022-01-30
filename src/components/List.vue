<template>
  <div class="row">
    <!-- users' info -->
    <div
      v-show="info.users.length"
      class="card"
      v-for="user in info.users"
      :key="user.id"
    >
      <a :href="user.html_url" target="_blank">
        <img :src="user.avatar_url" style="width: 100px" />
      </a>
      <p class="card-text">{{ user.login }}</p>
    </div>
    <!-- welcome message -->
    <div id="welcome-container" v-show="info.isWelcome">
      <div id="flip">
        <div><div>Welcome!</div></div>
        <div><div>Welcome!</div></div>
        <div><div>Welcome!</div></div>
      </div>
    </div>
    <!-- loading page -->
    <div class="load" v-show="info.isLoading">Loading . . .</div>
    <!-- error message -->
    <div class="error" v-show="info.errorMsg">{{ info.errorMsg }}</div>
  </div>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      info: {
        isWelcome: true,
        isLoading: false,
        errorMsg: "",
        users: [],
      },
    };
  },
  mounted() {
    this.$bus.$on("updateData", (updatedData) => {
      this.info = { ...this.info, ...updatedData };
    });
  },
  beforeDestroy() {
    this.$bus.$off("updateData");
  },
};
</script>

<style scoped>
.row {
  margin-top: 170px;
}

.card {
  float: left;
  width: 10%;
  padding: 30px 2px;
  margin-left: 60px;
  margin-bottom: 30px;
  border: 1px solid #4e4848;
  border-radius: 10px;
  text-align: center;
  font-size: 20px;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: white;
}

.card > img {
  margin-bottom: 0.75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}

#welcome-container {
  color: #999;
  text-transform: uppercase;
  font-size: 36px;
  font-weight: bold;
  padding-top: 200px;
  position: fixed;
  left: 45%;
  width: 100%;
  bottom: 45%;
  display: block;
}

#flip {
  height: 50px;
  overflow: hidden;
}

#flip > div > div {
  color: #fff;
  padding: 4px 12px;
  height: 45px;
  margin-bottom: 45px;
  display: inline-block;
}

#flip div:first-child {
  animation: show 5s linear infinite;
}

#flip div div {
  background: #42c58a;
}

#flip div:first-child div {
  background: #4ec7f3;
}

#flip div:last-child div {
  background: #dc143c;
}

@keyframes show {
  0% {
    margin-top: -270px;
  }
  5% {
    margin-top: -180px;
  }
  33% {
    margin-top: -180px;
  }
  38% {
    margin-top: -90px;
  }
  66% {
    margin-top: -90px;
  }
  71% {
    margin-top: 0px;
  }
  99.99% {
    margin-top: 0px;
  }
  100% {
    margin-top: -270px;
  }
}

.load {
  position: absolute;
  width: 80px;
  height: 64px;
  text-align: left;
  line-height: 64px;
  margin: -10px auto;
  font-size: 60px;
  font-weight: 400;
  color: rgba(240, 220, 220, 1);
  left: 43%;
  top: 50%;
  animation: fontAnim 3.75s infinite;
  animation-timing-function: ease-out;
  overflow: hidden;
}

@keyframes fontAnim {
  0% {
    width: 7ch;
  }
  16% {
    width: 8ch;
  }
  32% {
    width: 9ch;
  }
  48% {
    width: 10ch;
  }
  64% {
    width: 11ch;
  }
  80% {
    width: 12ch;
  }
  100% {
    width: 13ch;
  }
}

/* error message */
.error {
  border: 1px solid;
  margin: 10px 0px;
  padding: 15px 10px 15px 50px;
  background-repeat: no-repeat;
  background-position: 10px center;
  color: #d8000c;
  background-color: #ffbaba;
  background-image: url("https://i.imgur.com/GnyDvKN.png");
}
</style>
