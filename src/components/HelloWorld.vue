<script>
export default {
  data() {
    return {
      benutzerName: "",
      produktionsNummer: "",
      produkt: "",
      dieStückzahl: "",
    };
  },
  props: {
    msg: String,
  },
};
</script>

<template>
  <div-whole-area>
    <h1>{{ msg }}</h1>
    <div class="input-area">
      <div class="before-programm-starts">
        <input type="text" placeholder="Benutzer" v-model.trim="benutzerName" />
        <input
          type="text"
          placeholder="Produktions-Nummer"
          v-model.trim="produktionsNummer"
        />
        <input type="text" placeholder="Produkt" v-model.trim="produkt" />
      </div>
      <input type="text" placeholder="Stückzahl" v-model.trim="dieStückzahl" />
    </div>
    <div class="button-area">
      <button type="button">▶</button>
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
  </div-whole-area>
</template>

<style scoped>
.input-area {
  display: flex;
  gap: 3rem;
}

input {
  font-size: 1.5rem;
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
