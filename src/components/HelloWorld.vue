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
    };
  },
  methods: {
    playFunction() {
      if (this.benutzerName === "") {
        this.inputBenutzer = true;
      } else {
        const regex = /^\d+$/;
        if (
          this.produktionsNummer.length !== 5 ||
          regex.test(this.produktionsNummer) === false
        ) {
          this.inputProdNummer = true;
          alert(
            "Die Produktionsnummer muss genau 5-stellig sein und darf nur Zahlen/Ziffern enthalten!"
          );
        } else {
          if (this.produkt === "") {
            this.inputProdukt = true;
          } else {
            console.log("ööööööööööö");
          }
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
      <button type="button" @click="playFunction()">▶</button>
      <button type="button">||</button>
      <button type="button">■</button>
      <button type="button">Senden</button>
    </div>
    <div class="time-area">
      <p>
        <span>00</span><span class="colon">:</span><span>00</span
        ><span class="colon">:</span><span>00</span>
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
