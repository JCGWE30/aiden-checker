<script>
export default{
  data(){
    return{
      data: {},
      currentTime: Date.now()
    }
  },

  computed: {
    seconds() {
      return Math.floor((this.currentTime - this.data.timestamp) / 1000);
    },
    minutes() {
      return Math.floor(this.seconds / 60);
    },
    hours() {
      return Math.floor(this.minutes / 60);
    },
    days() {
      return Math.floor(this.hours / 24);
    },
    status(){
      switch(this.data.activity){
        case "message_create":
          return `He last sent a message in #${this.data.payload}`
        case "message_delete":
          return `He last deleted a message in #${this.data.payload}`
        case "voice_join":
          return `He last joined the "${this.data.payload}" voice channel`
        case "voice_leave":
          return `He last left the "${this.data.payload}" voice channel`
      }
    }
  },

  async mounted() {
      this.interval = setInterval(async () => {
          this.currentTime = Date.now();
          this.data = await (await fetch("https://aiden-api.coolingloopmc.com")).json()
      }, 1000);
  },

  beforeUnmount() {
      clearInterval(this.interval);
  }
}
</script>

<template>
  <img src="/aiden.png">
  <p class="titletext">When was Aiden last active in The LAST Server?</p>
  <div class="timerbox">
    <div class="wordbox unit">
      <p>Days</p>
      <p>Hours</p>
      <p>Minutes</p>
      <p>Seconds</p>
    </div>
    <div class="wordbox time">
      <p>{{ days }}</p>
      <p>{{ hours % 24 }}</p>
      <p>{{ minutes % 60 }}</p>
      <p>{{ seconds % 60 }}</p>
    </div>
    <div class="wordbox info">
      <p>Ago...</p>
    </div>
  </div>
  <div class="actionbox">
    <p>{{ status }}</p>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Chelsea+Market&family=M+PLUS+U:wght@100..900&family=Pirata+One&family=Playpen+Sans+Deva:wght@527&display=swap');

.wordbox p{
  text-align: center;
  width:7vw;
  margin: 0;
}

.info p{
  font-family: "Playpen Sans Deva", cursive;
  font-size: 1vw;
  font-weight: 400;
  font-style: normal;
}

.unit p{
  font-size: 2vw;
  font-family: "Pirata One", system-ui;
  font-style: normal;
}

.time p{
  font-size: 6vw;
  line-height: 5vw;
  height:5vw;
  text-align: center;
  text-justify: center;
  font-family: "Chelsea Market", system-ui;
  font-style: normal;
}

.wordbox{
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 4vw;
}

.titletext{
  font-family: "M PLUS U", sans-serif;
  font-size: 2vw;
  font-weight: 600;

  opacity: 0%;

  position: fixed;
  left: 50%;
  top: 5%;

  transform: translate(-50%, -50%);

  animation: fadeIn 4s ease 0s forwards;
}

.timerbox{
  background-color: #ffffff5e;
  position: fixed;
  opacity: 0%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  width: 45vw;
  height: 12vw;

  left: 40%;
  top: 65%;

  border-radius: 100px;

  transform: translate(0%, -55%);

  animation: popIn 4s ease 4.5s forwards;
}

.actionbox p{
  font-family: "Playpen Sans Deva", cursive;
  font-size: 2vw;
  font-weight: 400;
  font-style: normal;
  margin-left: 1vw;
  margin-right: 1vw;
}

.actionbox{
  background-color: #ffffff23;
  position: fixed;
  opacity: 0%;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  width: max-content;
  height: 4vw;

  left: 62.5%;
  top: 70%;

  border-radius: 100px;

  transform: translate(-50%, -50%);


  animation: fadeIn 4s ease 8.5s forwards
}

img {
  position: fixed;
  opacity: 0%;

  width: 25vw;
  height: 25vw;

  left: 50%;
  top: 50%;

  transform: translate(-50%, -50%);
  mask-image: radial-gradient(black 70%, transparent 0%);


  animation: fadeIn 4s ease 0s forwards, moveLeft 2s ease 4s forwards;
}

@keyframes moveLeft{
  to{
    left: 20%
  }
}

@keyframes popIn{
  to{
    opacity: 100%;
    top: 50%
  }
}

@keyframes fadeIn{
  to{
    opacity: 100%;
  }
}
</style>
