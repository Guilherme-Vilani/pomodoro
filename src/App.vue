<template>
  <div id="app">
    <div class="timer">
      <h1>Pomodoro</h1>
      <h1 v-if="classicPomodoro" class="time">{{ minutos }}:{{ segundos }}</h1>
      <h1 v-if="intervalo" class="time">
        {{ minutosIntervalo }}:{{ segundosIntervalo }}
      </h1>

      <div class="controls">
        <i
          v-on:click="ajustarTempo('+')"
          class="fa fa-plus"
          aria-hidden="true"
        ></i>
        <i
          v-if="tempoCorrendo === false"
          v-on:click="iniciarTempo()"
          class="fa fa-play"
          aria-hidden="true"
        ></i>
        <i
          v-else
          v-on:click="pararTempo()"
          class="fa fa-pause"
          aria-hidden="true"
        ></i>
        <i
          v-on:click="ajustarTempo('-')"
          class="fa fa-minus"
          aria-hidden="true"
        ></i>
      </div>
      <div class="controls">
        <i
          v-on:click="resetaTempo()"
          class="fa fa-refresh"
          aria-hidden="true"
        ></i>
        <button v-on:click="startIntervalo">Intervalo</button>
        <i
          v-if="tempoCorrendoIntervalo"
          v-on:click="pararTempo()"
          class="fa fa-pause"
          aria-hidden="true"
        ></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",

  data() {
    return {
      idTempo: "",
      tempo: 1500,
      duracao: 1500,
      minutos: 25,
      segundos: 0,
      minutosIntervalo: 5,
      segundosIntervalo: 0,
      tempoIntervalo: 300,
      duracaoIntervalo: 300,
      tempoCorrendo: false,
      tempoCorrendoIntervalo: false,
      intervalo: false,
      classicPomodoro: true,
    };
  },

  created() {
    this.minutos = this.minutos < 10 ? "0" + this.minutos : this.minutos;
    this.segundos = this.segundos < 10 ? "0" + this.segundos : this.segundos;
  },

  methods: {
    ajustarTempo(acao) {
      if (this.tempoCorrendo === false) {
        if (acao == "+") {
          this.duracao += 60;
          this.tempo += 60;

          this.minutos = parseInt(this.tempo / 60, 10);
          this.segundos = parseInt(this.tempo % 60, 10);

          this.minutos = this.minutos < 10 ? "0" + this.minutos : this.minutos;
          this.segundos =
            this.segundos < 10 ? "0" + this.segundos : this.segundos;
        } else {
          this.duracao -= 60;
          this.tempo -= 60;

          this.minutos = parseInt(this.tempo / 60, 10);
          this.segundos = parseInt(this.tempo % 60, 10);

          this.minutos = this.minutos < 10 ? "0" + this.minutos : this.minutos;
          this.segundos =
            this.segundos < 10 ? "0" + this.segundos : this.segundos;
        }
      }
    },

    timerObj(duracao) {
      this.tempo = duracao;
      this.duracao = duracao;
      var _this = this;
      this.idTempo = setInterval(function () {
        _this.minutos = parseInt(_this.tempo / 60, 10);
        _this.segundos = parseInt(_this.tempo % 60, 10);

        _this.minutos =
          _this.minutos < 10 ? "0" + _this.minutos : _this.minutos;
        _this.segundos =
          _this.segundos < 10 ? "0" + _this.segundos : _this.segundos;

        if (--_this.tempo < 0) {
          _this.tempo = _this.duracao;
          _this.resetaTempo();
        }
      }, 1000);
    },

    timerIntervaloObj(duracao) {
      this.tempoIntervalo = duracao;
      this.duracaoIntervalo = duracao;
      var _this = this;
      this.idTempo = setInterval(function () {
        _this.minutosIntervalo = parseInt(_this.tempoIntervalo / 60, 10);
        _this.segundosIntervalo = parseInt(_this.tempoIntervalo % 60, 10);

        _this.minutosIntervalo =
          _this.minutosIntervalo < 10
            ? "0" + _this.minutosIntervalo
            : _this.minutosIntervalo;
        _this.segundosIntervalo =
          _this.segundosIntervalo < 10
            ? "0" + _this.segundosIntervalo
            : _this.segundosIntervalo;

        if (--_this.tempoIntervalo < 0) {
          _this.tempoIntervalo = _this.duracaoIntervalo;
          _this.resetaTempo();
        }
      }, 1000);
    },

    iniciarTempo() {
      this.tempoCorrendo = true;
      this.timerObj(this.tempo);
      this.classicPomodoro = true;
      this.intervalo = false;
    },

    pararTempo() {
      this.tempoCorrendo = false;
      return window.clearInterval(this.idTempo);
    },

    pararTempoIntervalo() {
      this.tempoCorrendoIntervalo = false;
      return window.clearInterval(this.idTempo);
    },

    resetaTempo() {
      this.tempoCorrendo = false;

      this.tempo = 1500; // 25min default
      this.duracao = 1500;

      this.minutos = parseInt(this.tempo / 60, 10);
      this.segundos = parseInt(this.tempo % 60, 10);

      this.minutos = this.minutos < 10 ? "0" + this.minutos : this.minutos;
      this.segundos = this.segundos < 10 ? "0" + this.segundos : this.segundos;

      return window.clearInterval(this.idTempo);
    },

    startIntervalo() {
      this.tempoCorrendoIntervalo = true;
      this.intervalo = true;
      this.classicPomodoro = false;
      this.timerIntervaloObj(this.tempoIntervalo);
    },
  },
};
</script>

<style>
html {
  height: 100%;
}

body {
  height: 100%;
}

#app {
  height: 100%;
}

.timer {
  height: 80%;
}

.credits {
  height: 20%;
}

#app {
  font-family: "Open Sans", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: rgba(127, 140, 141, 1);
  margin-top: 0;
}

h1,
h2 {
  font-weight: normal;
}

.time {
  font-size: 5rem;
  font-weight: 300;
  padding-bottom: 10px;
  margin-bottom: 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

i {
  padding: 10px;
  margin: 0 5px;
  color: rgba(52, 73, 94, 1);
  cursor: pointer;
}

i:hover {
  color: rgba(52, 73, 94, 1);
}

.fa-play,
.fa-plus,
.fa-minus,
.fa-refresh {
  color: rgba(189, 195, 199, 1);
}
</style>
le>
