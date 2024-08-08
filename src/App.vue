<template>
  <div id="app">
    <div id="contenedor">
      <div id="caja1">
        <h1 id="temperatura-valor">{{ temperaturaValor }}</h1>
        <h1 id="temperatura-descripcion">{{ temperaturaDescripcion }}</h1>
      </div>
      <div id="caja2">
        <h2 id="ubicacion">{{ ubicacion }}</h2>
        <img id="icono-animado" :src="iconoAnimado" alt="" height="128" width="128">
      </div>
      <div id="caja3">
        <h3>Veloc. del Viento</h3>
        <h1 id="viento-velocidad">{{ vientoVelocidad }}</h1>
      </div>
    </div>
  </div>
</template>

<script>
import '@/assets/styles.css';

export default {
  data() {
    return {
      temperaturaValor: '',
      temperaturaDescripcion: '',
      ubicacion: '',
      iconoAnimado: '',
      vientoVelocidad: ''
    };
  },
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(posicion => {
        let lon = posicion.coords.longitude;
        let lat = posicion.coords.latitude;

        const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&APPID=adfdefbb82d8a2e392b7d1e15cf9e1f1&units=metric&lang=es`;

        fetch(url)
          .then(response => {
            if (!response.ok) {
              throw new Error('Network response was not ok ' + response.statusText);
            }
            return response.json();
          })
          .then(data => {
            this.temperaturaValor = `${Math.round(data.main.temp)} °C`;
            this.temperaturaDescripcion = data.weather[0].description.toUpperCase();
            this.ubicacion = data.name;
            this.vientoVelocidad = `${data.wind.speed} m/s`;

            switch (data.weather[0].main) {
              case 'Thunderstorm':
                this.iconoAnimado = require('@/assets/animated/thunder.svg');
                break;
              case 'Drizzle':
                this.iconoAnimado = require('@/assets/animated/rainy-2.svg');
                break;
              case 'Rain':
                this.iconoAnimado = require('@/assets/animated/rainy-7.svg');
                break;
              case 'Snow':
                this.iconoAnimado = require('@/assets/animated/snowy-6.svg');
                break;
              case 'Clear':
                this.iconoAnimado = require('@/assets/animated/day.svg');
                break;
              case 'Atmosphere':
                this.iconoAnimado = require('@/assets/animated/weather.svg');
                break;
              case 'Clouds':
                this.iconoAnimado = require('@/assets/animated/cloudy-day-1.svg');
                break;
              default:
                this.iconoAnimado = require('@/assets/animated/cloudy-day-1.svg');
            }
          })
          .catch(error => {
            console.log(error);
          });
      });
    }
  }
};
</script>
