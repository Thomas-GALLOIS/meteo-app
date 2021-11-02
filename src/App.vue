<template>
  <div class="titre"><h1>Météo.app</h1></div>
  <div class="choix">
    <p>Saisissez votre ville :</p>
    <div class="boubou">
      <input
        class="saisie"
        placeholder="votre ville"
        v-model="ville"
        @keyup.enter="meteoParVille"
      />

      <button class="bouton" @click="meteoParVille">valider</button>
    </div>
    <div class="partie-geoloc">
      <p>Ou</p>
      <button class="boutonloc" @click="meteoParLoc">
        Utiliser la géolocalisation
      </button>
    </div>
  </div>
  <div class="cadre-meteo">
    <h1 class="ville">Météo à : {{ ville }}</h1>
  </div>

  <div :class="card">
    <Meteo
      v-for="(element, index) in tableaumeteo"
      :key="index"
      :dateTime="element.dt_txt"
      :temp="element.main.temp"
      :tempMin="element.main.temp_min"
      :tempMax="element.main.temp_max"
      :speed="element.wind.speed"
      :description="element.weather[0].description"
      :icon="element.weather[0].icon"
    />
  </div>
</template>

<script>
import Meteo from "./components/meteo.vue";

export default {
  components: {
    Meteo: Meteo,
  },

  /* Data properties */
  data() {
    return {
      ville: "",

      tableaumeteo: [],
      temperature: 0,
      card: "",
    };
  },

  /* Mounted */
  async mounted() {
    const response = await fetch(
      "https://api.openweathermap.org/data/2.5/forecast?zip=06000,fr&appid=5f818b8bda9ed6f2b175b4aa0d1dc6e8&units=metric&lang=fr"
    );
    const weatherData = await response.json();
    console.log(weatherData);
    this.ville = weatherData.city.name;
    this.tableaumeteo = weatherData.list;
    this.temperature = weatherData.list[0].main.temp;

    if (this.temperature >= 25) {
      this.card = "cardHot";
    } else if (this.temperature >= 10 && this.temperature < 25) {
      this.card = "card";
    } else {
      this.card = "cardCold";
    }
  },

  methods: {
    meteoParLoc() {
      navigator.geolocation.getCurrentPosition(async (position) => {
        const lat = position.coords.latitude;
        const lng = position.coords.longitude;

        const response = await fetch(
          "https://api.openweathermap.org/data/2.5/forecast?lat=" +
            lat +
            "&lon=" +
            lng +
            "&appid=5f818b8bda9ed6f2b175b4aa0d1dc6e8&units=metric&lang=fr"
        );

        const weatherData = await response.json();
        console.log(weatherData);

        this.ville = weatherData.city.name;
        this.tableaumeteo = weatherData.list;
        this.temperature = weatherData.list[0].main.temp;

        if (this.temperature >= 25) {
          this.card = "cardHot";
        } else if (this.temperature >= 10 && this.temperature < 25) {
          this.card = "card";
        } else {
          this.card = "cardCold";
        }
      });
    },

    async meteoParVille() {
      const response = await fetch(
        "https://api.openweathermap.org/data/2.5/forecast?q=" +
          this.ville +
          "&appid=5f818b8bda9ed6f2b175b4aa0d1dc6e8&units=metric&lang=fr"
      );
      const weatherData = await response.json();
      console.log(weatherData);
      this.ville = weatherData.city.name;
      this.tableaumeteo = weatherData.list;
      this.temperature = weatherData.list[0].main.temp;

      if (this.temperature >= 25) {
        this.card = "cardHot";
      } else if (this.temperature >= 10 && this.temperature < 25) {
        this.card = "card";
      } else {
        this.card = "cardCold";
      }
    },
  },
};
</script>

<style>
body {
  background-image: url("./assets/Beautiful-sky-clouds-sunset_m.jpeg");
  background-size: cover;
  color: white;
  font-family: "Barlow", sans-serif;
  font-weight: bold;
  justify-content: center;
}
.titre {
  font-size: 17px;
  margin-left: 20px;
}

.choix {
  display: flex;
  justify-content: center;
  font-size: 17px;
  align-items: center;
}

.saisie {
  padding: 5px;
  margin-left: 5px;
  margin-right: 5px;
  border-radius: 5px;

  width: 150px;
  border: 2px solid rgb(189, 235, 243);
  background-color: rgb(189, 235, 243);
  color: #809bce;
}

.boubou {
  display: flex;
  flex-wrap: wrap;
}
.bouton {
  height: 30px;
  margin-left: 5px;
  margin-right: 5px;
  border-radius: 10px;
  background-color: #809bce;
  border: 1px solid #809bce;
  color: white;
  width: 100px;
  font-size: 17px;
  box-shadow: 3px 6px 8px #444444;
  cursor: pointer;
}
.bouton:hover {
  background-color: rgb(189, 235, 243);
  color: #809bce;
}

.partie-geoloc {
  display: flex;
}
.boutonloc {
  height: 30px;
  padding: 5px;
  margin-top: 12px;
  margin-left: 5px;
  margin-right: 5px;
  border-radius: 10px;
  background-color: #809bce;
  border: 1px solid #809bce;
  color: white;
  width: 200px;
  font-size: 15px;
  box-shadow: 3px 6px 8px #444444;
  cursor: pointer;
}
.boutonloc:hover {
  background-color: rgb(189, 235, 243);
  color: #809bce;
}
.ville {
  text-align: center;
  color: white;
  font-size: 40px;
  font-weight: bold;
}

.card {
  height: 483px;
  width: 720px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 15px;

  display: flex;
  flex-direction: row;
  overflow-x: scroll;
  border-radius: 5px;
  box-shadow: 4px 5px 8px #444444;
  background-color: rgb(13, 110, 155);
}

.cardHot {
  height: 483px;
  width: 720px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 15px;

  display: flex;
  flex-direction: row;
  overflow-x: scroll;
  border-radius: 5px;
  box-shadow: 4px 5px 8px #444444;
  background-image: url("./assets/12-127551_fire-burning.jpeg");
  background-size: cover;
}
.cardCold {
  height: 483px;
  width: 720px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 15px;

  display: flex;
  flex-direction: row;
  overflow-x: scroll;
  border-radius: 5px;
  box-shadow: 4px 5px 8px #444444;
  background-image: url("./assets/fond-ecran-wallpaper-image-fonds-ecran-abstrait-glace-18-660x330.jpeg");
  background-size: cover;
}

.meteo {
  margin: 20px 20px 20px 20px;
  background-color: rgba(176, 176, 235, 0.5);
  box-shadow: 4px 5px 8px #444444;
  border-radius: 5px;
  padding: 20px;
  color: white;
  width: 150px;
  height: 400px;
  font-size: 12px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

/* @media only screen and (min-device-width: 375px) and (max-device-width: 812px) and (-webkit-min-device-pixel-ratio: 3) and (orientation: portrait) {
  .titre {
    font-size: 17px;
    margin-left: 100px;
  }

  .choix {
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 17px;
    align-items: center;
    justify-content: space-evenly;
  }

  .card {
    height: 483px;
    width: 230px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 15px;
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    border-radius: 5px;
    box-shadow: 4px 5px 8px #444444;
    background-color: rgb(13, 110, 155);
  }*/

@media only screen and (min-device-width: 375px) and (max-device-width: 667px) and (-webkit-min-device-pixel-ratio: 2) and (orientation: portrait) {
  .titre {
    font-size: 17px;
    margin-left: 100px;
  }

  .choix {
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 17px;
    align-items: center;
    justify-content: space-evenly;
  }

  .card {
    height: 483px;
    width: 230px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 15px;
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    border-radius: 5px;
    box-shadow: 4px 5px 8px #444444;
    background-color: rgb(13, 110, 155);
  }

  .cardHot {
    height: 483px;
    width: 230px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 15px;

    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    border-radius: 5px;
    box-shadow: 4px 5px 8px #444444;
    background-image: url("./assets/12-127551_fire-burning.jpeg");
    background-size: cover;
  }

  .cardCold {
    height: 483px;
    width: 230px;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 15px;

    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    border-radius: 5px;
    box-shadow: 4px 5px 8px #444444;
    background-image: url("./assets/fond-ecran-wallpaper-image-fonds-ecran-abstrait-glace-18-660x330.jpeg");
    background-size: cover;
  }

  .partie-geoloc {
    display: none;
  }
}
</style>
