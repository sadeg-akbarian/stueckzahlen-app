<script>
export default {
  data() {
    return {
      benutzerName: "",
      produktionsNummer: "",
      produkt: "",
      dieStückzahl: "",
      inputBenutzer: false,
      inputProdNummer: false,
      inputProdukt: false,
      inputStückzahl: false,
      theSeconds: "00",
      theMinutes: "00",
      theHours: "00",
      playActive: false,
      haltActive: true,
      stopActive: true,
      sendActive: true,
      allInputsFilled: false,
      intervallRunning: null,
      wasItStopped: false,
    };
  },
  methods: {
    checkRequirements() {
      if (this.benutzerName === "") {
        this.inputBenutzer = true;
        this.allInputsFilled = false;
      } else {
        const regex = /^\d+$/;
        if (
          this.produktionsNummer.length !== 5 ||
          regex.test(this.produktionsNummer) === false
        ) {
          this.inputProdNummer = true;
          this.allInputsFilled = false;
          alert(
            "Die Produktionsnummer muss genau 5-stellig sein und darf nur Zahlen/Ziffern enthalten!"
          );
        } else {
          if (this.produkt === "") {
            this.inputProdukt = true;
            this.allInputsFilled = false;
          } else {
            this.allInputsFilled = true;
          }
        }
      }
    },
    startFunction() {
      this.wasItStopped = false;
      if (this.allInputsFilled === true) {
        this.intervallRunning = setInterval(this.increaseTheTime, 1000);
      }
    },
    haltStopFunction(whatWasClicked) {
      clearInterval(this.intervallRunning);
      if (whatWasClicked === "halt") {
        this.wasItStopped = false;
      } else {
        this.wasItStopped = true;
      }
    },
    increaseTheTime() {
      if (this.theSeconds === "59") {
        this.theSeconds = "00";
        if (this.theMinutes === "59") {
          this.theMinutes = "00";
          let changedHours = Number(this.theHours);
          changedHours++;
          this.theHours = "" + changedHours;
          if (this.theHours.length === 1) {
            this.theHours = "0" + this.theHours;
          }
        } else {
          let changedMinutes = Number(this.theMinutes);
          changedMinutes++;
          this.theMinutes = "" + changedMinutes;
          if (this.theMinutes.length === 1) {
            this.theMinutes = "0" + this.theMinutes;
          }
        }
      } else {
        let changedSeconds = Number(this.theSeconds);
        changedSeconds++;
        this.theSeconds = "" + changedSeconds;
        if (this.theSeconds.length === 1) {
          this.theSeconds = "0" + this.theSeconds;
        }
      }
    },
    changeButtonStatus(whichButton) {
      if (this.allInputsFilled === true) {
        if (whichButton === "playButton") {
          this.playActive = true;
          this.haltActive = false;
          this.stopActive = false;
          console.log("play");
        } else if (whichButton === "haltButton") {
          this.playActive = false;
          this.haltActive = true;
          this.stopActive = false;
          console.log("halt");
        } else if (whichButton === "stopButton") {
          this.playActive = false;
          this.haltActive = true;
          this.stopActive = true;
          console.log("stop");
        }
      }
    },
  },
  props: {
    msg: String,
  },
};
</script>

<template>
  <div class="whole-area">
    <h1>{{ msg }}</h1>
    <div class="input-area">
      <div class="before-programm-starts">
        <input
          type="text"
          placeholder="Benutzer"
          v-model.trim="benutzerName"
          :class="{ 'input-alert': inputBenutzer }"
          @click="inputBenutzer = false"
        />
        <input
          type="text"
          placeholder="Produktions-Nummer"
          v-model.trim="produktionsNummer"
          :class="{ 'input-alert': inputProdNummer }"
          @click="inputProdNummer = false"
        />
        <input
          type="text"
          placeholder="Produkt"
          v-model.trim="produkt"
          :class="{ 'input-alert': inputProdukt }"
          @click="inputProdukt = false"
        />
      </div>
      <input
        type="text"
        placeholder="Stückzahl"
        v-model.trim="dieStückzahl"
        :class="{ 'input-alert': inputStückzahl }"
        @click="inputStückzahl = false"
      />
    </div>
    <div class="button-area">
      <button
        type="button"
        :disabled="playActive"
        @click="
          checkRequirements(), changeButtonStatus('playButton'), startFunction()
        "
      >
        ▶
      </button>
      <button
        type="button"
        :disabled="haltActive"
        @click="
          checkRequirements(),
            changeButtonStatus('haltButton'),
            haltStopFunction('halt')
        "
      >
        ||
      </button>
      <button
        type="button"
        :disabled="stopActive"
        @click="
          checkRequirements(),
            changeButtonStatus('stopButton'),
            haltStopFunction('stop')
        "
      >
        ■
      </button>
      <button type="button" :disabled="sendActive">Senden</button>
    </div>
    <div class="time-area">
      <p>
        <span>{{ theHours }}</span
        ><span class="colon">:</span><span>{{ theMinutes }}</span
        ><span class="colon">:</span><span>{{ theSeconds }}</span>
      </p>
    </div>
  </div>
</template>

<style scoped>
.input-area {
  display: flex;
  gap: 3rem;
}

input {
  font-size: 1.5rem;
}

.input-alert {
  border: red 0.25rem dotted;
}

.before-programm-starts {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.input-area > input {
  height: fit-content;
}

.button-area {
  margin-top: 3rem;
}

button {
  font-size: 2rem;
  border: 0.2rem solid black;
}

button + button {
  margin-left: 1rem;
}

p {
  font-size: 3rem;
  position: relative;
}

span {
  margin-right: 0.25rem;
}

.colon {
  position: relative;
  top: -3px;
}
</style>
