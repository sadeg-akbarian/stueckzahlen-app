<script>
import Papa from "papaparse";

export default {
  data() {
    return {
      benutzerName: "",
      produktionsNummer: "",
      Kuerzel: "Wähle ein Kürzel",
      produkt: "",
      dieStückzahl: "",
      inputBenutzer: false,
      inputProdNummer: false,
      inputKuerzel: false,
      inputProdukt: false,
      inputStückzahl: false,
      valideStückzahl: false,
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
      displayFinalArea: false,
      csvLink: "/stueckzahlen-csv/noteList.csv",
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
          if (this.Kuerzel === "Wähle ein Kürzel") {
            this.inputKuerzel = true;
            this.allInputsFilled = false;
          } else {
            if (this.produkt === "") {
              this.inputProdukt = true;
              this.allInputsFilled = false;
            } else {
              this.allInputsFilled = true;
            }
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
    validateAchievedNumbers() {
      console.log(this.dieStückzahl);
      const regex = /^\d+$/;
      if (this.dieStückzahl === "") {
        this.sendActive = true;
        this.valideStückzahl = false;
      }
      if (regex.test(this.dieStückzahl) === false && this.dieStückzahl !== "") {
        alert("Bitte gebe nur Zahlen/Ziffern für die Stückzahl ein!!!");
        this.inputStückzahl = true;
      } else if (
        regex.test(this.dieStückzahl) === true &&
        this.dieStückzahl !== "" &&
        this.allInputsFilled === true
      ) {
        this.valideStückzahl = true;
      }
    },
    deleteEverything() {
      this.benutzerName = "";
      this.produktionsNummer = "";
      this.Kuerzel = "Wähle ein Kürzel";
      this.produkt = "";
      this.dieStückzahl = "";
      this.inputBenutzer = false;
      this.inputProdNummer = false;
      this.inputKuerzel = false;
      this.inputProdukt = false;
      this.inputStückzahl = false;
      this.valideStückzahl = false;
      this.theSeconds = "00";
      this.theMinutes = "00";
      this.theHours = "00";
      this.playActive = false;
      this.haltActive = true;
      this.stopActive = true;
      this.sendActive = true;
      this.allInputsFilled = false;
      this.intervallRunning = null;
      this.wasItStopped = false;
      this.displayFinalArea = false;
    },
    activateSendButton() {
      if (this.valideStückzahl === true && this.allInputsFilled === true) {
        this.sendActive = false;
      } else {
        alert("Bitte überprüfe nochmal alle Eingaben!");
      }
    },
    sendTheData() {
      function createUniqueId() {
        const dateString = Date.now().toString(36);
        const randomness = Math.random().toString(36).substring(2);
        return dateString + randomness;
      }
      const uniqueId = createUniqueId();

      const wholeDate = new Date();
      const dateAndTime = wholeDate.toLocaleString();
      const date = dateAndTime.slice(0, 10);
      const dateSplitted = date.split(".");

      const quantityNote = {
        id: uniqueId,
        year: dateSplitted[2],
        month: dateSplitted[1],
        day: dateSplitted[0],
        product: this.produkt,
        productionNumber: this.produktionsNummer,
        abbreviation: this.Kuerzel,
        time: this.theHours + ":" + this.theMinutes + ":" + this.theSeconds,
        quantity: this.dieStückzahl,
        user: this.benutzerName,
      };

      console.log("üüüüüüüüüüüüüüüü");
      this.getDataFromServer(quantityNote);
    },
    getDataFromServer(newQuantityNote) {
      fetch("/stueckzahlen-csv/noteList.csv")
        .then((response) => {
          if (response.ok) {
            return response.blob(); // Zuerst als Text abrufen
          } else {
            alert("Data could not be loaded from server!!!");
          }
        })
        .then((data) => {
          this.papaParseFunction(data, newQuantityNote);
        })
        .catch((error) => {
          console.error("Fehler beim Abrufen der Daten:", error);
        });
    },
    papaParseFunction(oldNoteList, theNewQuantityNote) {
      Papa.parse(oldNoteList, {
        header: true, // Wenn du Kopfzeilen in der CSV hast, kannst du diese Option setzen
        skipEmptyLines: true, // Leere Zeilen überspringen
        complete: (results) => {
          console.log(results);
          console.log(theNewQuantityNote);
          const updatedList = [...results.data, theNewQuantityNote];
          console.log(updatedList);
          const updatedCsv = Papa.unparse(updatedList);
          console.log(updatedCsv);

          const blob = new Blob([updatedCsv], {
            type: "text/csv;charset=utf-8;",
          });
          const formData = new FormData();
          formData.append("file", blob, "updatedFile.csv"); // 'file' ist der Parametername für den Upload
          // "file" ist der Parametername, den der Server erwartet, und "data.csv" ist der Dateiname, der mitgesendet wird.
          console.log("################");
          this.sendDataToServer(formData);
        },
        error: (error) => {
          console.error("Error parsing CSV:", error);
        },
      });
    },
    sendDataToServer(newList) {
      fetch("/stueckzahlen-csv/noteList.csv", {
        method: "POST",
        body: newList,
      })
        .then((response) => {
          if (response.ok) {
            console.log("Genau");
            return response.blob();
          } else {
            console.log(response.status);
            alert("Your data could not be added!!!");
          }
        })
        .then((data) => {
          console.log("Erfolgreich gesendet:", data);

          this.displayFinalArea = false;
          this.deleteEverything();
        });
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
          @blur=""
        />
        <div class="fuerProdNummer">
          <input
            type="text"
            placeholder="Produktions-Nummer"
            v-model.trim="produktionsNummer"
            :class="{ 'input-alert': inputProdNummer }"
            @click="inputProdNummer = false"
          />
          <select
            v-model="Kuerzel"
            :class="{ 'input-alert': inputKuerzel }"
            @click="inputKuerzel = false"
          >
            <option disabled selected>Wähle ein Kürzel</option>
            <option value="KH1">KH1</option>
            <option value="USB">USB</option>
            <option value="DRS">DRS</option>
            <option value="CD1">CD1</option>
            <option value="DV1">DV1</option>
          </select>
        </div>
        <input
          type="text"
          placeholder="Produkt"
          v-model.trim="produkt"
          :class="{ 'input-alert': inputProdukt }"
          @click="inputProdukt = false"
        />
      </div>
      <div class="after-programm-stoped">
        <input
          type="text"
          placeholder="Stückzahl"
          v-model.trim="dieStückzahl"
          :disabled="!this.wasItStopped"
          :class="{ 'input-alert': inputStückzahl }"
          @click="inputStückzahl = false"
          @blur="validateAchievedNumbers()"
          @keyup="this.sendActive = true"
        />
        <div class="explanationForSendenButton">
          Wenn alles richtig eingegeben ist, <br />
          klicke bitte auf den grünen Kreis <br />
          um den Senden-Button zu aktivieren!
        </div>
        <div class="green-circle" @click="activateSendButton()"></div>
      </div>
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
      <button
        type="button"
        :disabled="sendActive"
        @click="displayFinalArea = true"
      >
        Senden
      </button>
    </div>
    <div class="time-area">
      <p>
        <span>{{ theHours }}</span
        ><span class="colon">:</span><span>{{ theMinutes }}</span
        ><span class="colon">:</span><span>{{ theSeconds }}</span>
      </p>
    </div>
    <div class="final-question-area" v-show="displayFinalArea">
      <p>
        Hast du alle Eingaben nochmal überpüft? <br />
        Willst du die Daten wirklich abschicken?
      </p>
      <button
        type="button"
        class="final-buttons"
        @click="displayFinalArea = false"
      >
        Nein
      </button>
      <button type="button" class="final-buttons" @click="sendTheData()">
        Ja
      </button>
    </div>
  </div>
</template>

<style scoped>
.whole-area {
  position: relative;
}

.input-area {
  display: flex;
  gap: 4rem;
}

.fuerProdNummer {
  display: flex;
}

input {
  font-size: 1.5rem;
}

select {
  margin-left: 0.5rem;
  padding-inline: 0.25rem;
  font-size: 1.25rem;
}

.input-alert {
  border: red 0.25rem dotted;
}

.before-programm-starts {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.after-programm-stoped {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.explanationForSendenButton {
  margin-top: 1rem;
  font-weight: 800;
  color: red;
}

.green-circle {
  width: 3rem;
  height: 3rem;
  background-color: green;
  border-radius: 50%;
  margin-top: 1.5rem;
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

.final-question-area {
  border: 1rem solid rgb(160, 28, 160);
  padding-inline: 4rem;
  padding-bottom: 4rem;
  background-color: blue;
  color: white;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 5vw;
  left: -7.5vw;
}
</style>
