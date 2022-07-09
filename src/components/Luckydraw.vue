<template>
  <div class="lucky-draw">
    <!-- header ui -->
    <div class="header">
      <img src="@/assets/pcf/pdf_game/header.png" style="width: 100vw;" />
    </div>

    <div class="prize-placeholder pa-8">
      <div class="prize-title pt-4" style="font-size: 2rem; font-weight: 600"></div>
    </div>

    <!-- draw page -->
    <div class="draw-page" v-show="state === 1">
      <!-- draw tickets -->

      <div class="draw-box shimmer">
        <ul id="candidateList" ref="candidateList" @animationend="celebrate">
          <li class="item-1" v-for="(token, index) in tokenList" :key="index">{{ token.id }}</li>
        </ul>
      </div>
      <div>
        <img
          class="start-button"
          @click="
            activateClass();
            getUserList();
          "
          src="@/assets/pcf/pdf_game/start_button.png"
          style="height: 8vh"
        />
      </div>
    </div>

    <div class="winning-page" v-show="state === 4">
      <div class="winning-box">
        <div class="winning-token shimmer">Congratulations!</div>
        <div class="winning-plate">
          <div class="winning-token shimmer">{{ winningToken.id }}</div>
          <div class="winning-token shimmer">{{ winningToken.name }}</div>
        </div>
      </div>

      <div>
        <img
          class="start-button"
          @click="
            changeState(1);
            getUserList();
          "
          src="@/assets/pcf/pdf_game/back_button.png"
          style="height: 8vh"
        />
      </div>
    </div>

    <!-- footer ui -->
    <div class="pt-4 footer">
      <img src="@/assets/pcf/pdf_game/footer.png" style="width: 100vw" />
    </div>
  </div>
</template>

<script>
import userList from '../data/data.json';

export default {
  name: 'Luckydraw_v2',
  components: {},
  props: {
    restart: Boolean,
  },
  data() {
    return {
      apiKey: 'keySLF7Ph3gg0FEuk',
      pvtData: '',
      secret: 'fukCoup@2021',
      dialog: false,
      state: 1,
      winningToken: '',
      tokenList: [],
      prizeList: [],
      prizeImg: '',
      counter: 0,
      infoText: '',
      userList: userList,
      animate: false,
    };
  },
  watch: {
    restart() {
      if (this.restart) this.state = 1;
    },
  },
  mounted() {
    this.onInit();
  },
  methods: {
    onInit() {
      this.spin;
      this.state = 1;
      this.getUserList();
    },

    getUserList() {
      this.userList = this.shuffle(userList);
      this.tokenList = [];
      console.log('Total candidates:', this.userList.length);

      //do while
      let i = 0;
      do {
        let random = Math.floor(Math.random() * this.userList.length);
        let id = this.userList[random];

        this.tokenList.push(id);
        i++;
      } while (i < 500);

      // console.log(this.tokenList);
      this.tokenList = this.shuffle(this.tokenList);

      let tmp = Math.floor(Math.random() * this.tokenList.length);
      this.winningToken = this.tokenList[452] = this.tokenList[tmp];
    },

    celebrate() {
      this.state = 4;
      setTimeout(() => {
        console.log(`>> ${this.winningToken.id}, ${this.winningToken.name}`);
      }, 1000);
    },

    changeState(state) {
      this.state = state;
      this.$emit('clicked', state, this.counter);
    },

    shuffle(array) {
      return array.sort(() => Math.random() - 0.5);
    },

    activateClass() {
      this.$refs.candidateList.classList.remove('scrollUp');
      void this.$refs.candidateList.offsetWidth;
      this.$refs.candidateList.classList.add('scrollUp');
    },
  },
};
</script>

<style scoped>
.lucky-draw {
  position: relative;
  /* border: #272522 10px solid; */
  background-image: url('../assets/pcf/pdf_game/bg.png');
  background-color: #dc8d18;
  height: 60%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 100vh;
}

.intro-page {
  position: inherit;
  bottom: 0;
}

.ready-page {
  position: inherit;
  bottom: 0;
}

.draw-page,
.winning-page {
  display: inline-grid;
}

/* .winning-page {
  position: inherit;
  bottom: 0;
} */

.winning-token {
  /* position: relative; */
  /* top: -10.2vh; */
  font-size: 3vw;
  font-weight: bold;
  /* color: white; */
  /* width: 28vw; */
}

.winning-plate {
  padding-top: 2rem;
  animation: scale 0.5s alternate infinite ease-in;
}

.next-button {
  position: relative;
  top: -10.5vh;
  left: 40vw;
}

.footer {
  position: fixed;
  bottom: -1vh;
  z-index: 1;
}

@keyframes scale {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(1.05);
  }
}

.shimmer {
  color: rgba(255, 255, 255, 0.1);
}
.shimmer {
  /* the shimmer magic */
  background: -webkit-gradient(linear, left top, right top, from(#222), to(#222), color-stop(0.5, rgb(255, 191, 0)));
  background: -moz-gradient(linear, left top, right top, from(#222), to(#222), color-stop(0.5, rgb(255, 191, 0)));
  background: gradient(linear, left top, right top, from(#222), to(#222), color-stop(0.5, rgb(255, 191, 0)));
  -webkit-background-size: 125px 100%;
  -moz-background-size: 125px 100%;
  background-size: 125px 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  background-clip: text;
  -webkit-animation-name: shimmer;
  -moz-animation-name: shimmer;
  -webkit-animation-name: shimmer;
  animation-name: shimmer;
  -webkit-animation-duration: 5s;
  -moz-animation-duration: 5s;
  -webkit-animation-duration: 5s;
  animation-duration: 5s;
  -webkit-animation-iteration-count: infinite;
  -moz-animation-iteration-count: infinite;
  -webkit-animation-iteration-count: infinite;
  animation-iteration-count: infinite;
  background-repeat: no-repeat;
  background-position: 0 0;
  background-color: #222;
}

@-moz-keyframes shimmer {
  0% {
    background-position: top left;
  }

  100% {
    background-position: top right;
  }
}

@-webkit-keyframes shimmer {
  0% {
    background-position: top left;
  }

  100% {
    background-position: top right;
  }
}

@-o-keyframes shimmer {
  0% {
    background-position: top left;
  }

  100% {
    background-position: top right;
  }
}

@-ms-keyframes shimmer {
  0% {
    background-position: top left;
  }

  100% {
    background-position: top right;
  }
}

@keyframes shimmer {
  0% {
    background-position: top left;
  }

  100% {
    background-position: top right;
  }
}

.draw-box,
.winning-box {
  height: 32vh;
  width: 30vw;
  max-width: 30vw;
  position: relative;
  overflow: hidden;
  font-size: 2.4rem;
  font-weight: bold;
  line-height: 4.8rem;
  margin: 0px;
  text-align: center;
  padding: 1rem;
  border-radius: 1rem;
  color: #1d2227;
  margin-bottom: 2rem;
}

.draw-box {
  border: 0.5rem solid #1d2227;
  background: #fc9a213d;
  /* letter-spacing: 0.3rem; */
}

.winning-box {
  background: #ff920ea6;
  display: grid;
  align-content: center;
}

li {
  font-size: larger;
  letter-spacing: 1rem;
}

.draw-box::before {
  top: 0;
  left: 0;
  z-index: 1;
  content: '';
  width: 100%;
  height: 15vh;
  position: absolute;
  font-size: 2.4rem;
  color: white;
  background: linear-gradient(180deg, #ff9800, rgba(255, 142, 113, 0));
}
.draw-box::after {
  bottom: 0;
  left: 0;
  z-index: 1;
  content: '';
  width: 100%;
  height: 15vh;
  position: absolute;
  font-size: 2.4rem;
  color: white;
  background: linear-gradient(180deg, rgba(255, 142, 113, 0), #ff9800);
}

.animationList,
ul {
  padding: 0;
}

/* scrolling list */

ul {
  opacity: 1;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.scrollUp {
  -webkit-animation: scrollUp 15s 0.2s ease-in-out 1 normal forwards;
  animation: scrollUp 15s 0.2s ease-in-out 1 normal forwards;
}

@-webkit-keyframes scrollUp {
  from {
    -webkit-transform: translateY(0);
    transform: translateY(0);
  }
  to {
    -webkit-transform: translateY(-90%);
    transform: translateY(-90%);
  }
}

@keyframes scrollUp {
  from {
    -webkit-transform: translateY(0);
    transform: translateY(0);
  }
  to {
    -webkit-transform: translateY(-90%);
    transform: translateY(-90%);
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.1s cubic-bezier(1, 0.2, 0.8, 1);
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
