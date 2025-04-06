<template>
  <div id="app">
    <!-- Texto original -->
    <div class="text-container">
      <div
        @click="onCakeClick"
        style="cursor: pointer"
        v-if="isUnlocked && !showBalloons && !showBirthdayModal"
      >
        <div class="cake">
          <div class="candle">
            <div class="fire"></div>
            <div class="fire"></div>
            <div class="fire"></div>
            <div class="fire"></div>
            <div class="fire"></div>
          </div>
          <div class="coverage"></div>
          <div class="mid-cake"></div>
          <h1>Feliz aniversário!</h1>
          <p>Pedro Henrique</p>
        </div>
      </div>
    </div>

    <!-- Camada de bloqueio -->
    <div class="overlay" v-if="!isUnlocked">
      <input
        type="password"
        ref="passwordInput"
        v-model="password"
        placeholder="Digite a senha"
        @keyup.enter="checkPassword"
        :class="{ shake: isShaking, error: isErrored }"
      />
      <button @click="checkPassword" class="btn">Desbloquear</button>
      <!-- Mensagem de dica após 3 tentativas -->
      <p v-if="attempts >= 3" class="hint">
        Dica: Agora que provei meu ponto, vou ali comer...
      </p>
    </div>

    <!-- Animação de Balões -->
    <div v-if="showBalloons" class="box-canvas">
      <div class="balloon-wrapper red">
        <div class="string"></div>
        <div class="balloon"></div>
      </div>
      <div class="balloon-wrapper green">
        <div class="string"></div>
        <div class="balloon"></div>
      </div>
      <div class="balloon-wrapper orange">
        <div class="string"></div>
        <div class="balloon"></div>
      </div>
      <div class="balloon-wrapper blue">
        <div class="string"></div>
        <div class="balloon"></div>
      </div>
      <div class="balloon-wrapper yellow">
        <div class="string"></div>
        <div class="balloon"></div>
      </div>
    </div>

    <!-- Modal de Aniversário -->
    <div v-if="showBirthdayModal" class="modal-overlay">
      <div class="modal-content">
        <h2>te amo pdrinho</h2>
        <p>
          <strong>
            É meio estranho vir aqui depois de gravar o vídeo e mesmo aqui, com infinitas linhas de texto, eu ainda não consigo pôr em palavras o quanto você é foda e importante pra mim, eu amo muito você e eu te admiro demais por quem você é e tudo que você tem feito pelas pessoas, mesmo que ninguém ou poucas pessoas saibam disso. Quero deixar claro a gratidão que eu tenho por você, mesmo que você não saiba disso, você me ajudou de diversas formas; Seja apenas conversando, ou só zoando como a gente sempre fez. Volto a falar que espero ansiosamente pelo dia em que você e a Rafa irão vir para São Paulo, ou eu ir para Selbach para que possamos nos ver pessoalmente. Amo muito você e agradeço por ter uma pessoa como você na minha vida. <br>
            ❤️
          </strong>
        </p>
        <button @click="closeBirthdayModal">Fechar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      password: "",
      correctPassword: "bosta",
      isUnlocked: false,
      attempts: 0,
      isShaking: false,
      isErrored: false,
      showBirthdayModal: false, // Estado para controlar o modal de aniversário
      showBalloons: false, // Estado para controlar a animação de balões
    };
  },
  methods: {
    checkPassword() {
      this.password = this.password.toLowerCase();
      if (this.password === this.correctPassword) {
        this.isUnlocked = true;
        this.attempts = 0;
      } else {
        this.attempts++;
        this.triggerError();
        this.password = null;
      }
    },
    triggerError() {
      this.isErrored = true;
      this.isShaking = true;

      this.$refs.passwordInput.blur();

      setTimeout(() => {
        this.isShaking = false;
        this.isErrored = false;
      }, 500);
    },
    onCakeClick() {
      this.showBalloons = true; // Ativa a animação de balões

      // Após 2 segundos (duração da animação), exibe o modal
      setTimeout(() => {
        this.showBirthdayModal = true;
        this.showBalloons = false; // Desativa a animação de balões
      }, 2000);
    },
    closeBirthdayModal() {
      this.showBirthdayModal = false; // Fecha o modal de aniversário
    },
  },
};
</script>

<style>
/* Estilo global para tema escuro */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  background-color: #1e1e1e;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.btn {
  background-color: #c77dff !important;
}

.text-container {
  position: relative;
  display: inline-block;
}

.text {
  font-size: 50px;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  text-align: center;
  color: #ffffff;
  transition: opacity 0.5s ease;
}

.text:not(.unlocked) {
  filter: blur(15px);
  opacity: 0.3;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  z-index: 9999;
}

.overlay input {
  font-size: 20px;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ffffff;
  border-radius: 5px;
  width: 200px;
  background-color: #333333;
  color: #ffffff;
  transition: border-color 0.3s ease, transform 0.3s ease;
}

.overlay input.error {
  border-color: #ff4d4d;
}

.overlay input.shake {
  animation: shake 0.5s;
}

.overlay button {
  font-size: 16px;
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.overlay button:hover {
  background-color: #3aa876;
}

.hint {
  margin-top: 20px;
  font-size: 18px;
  color: #ffcc00;
  font-weight: bold;
}

@keyframes shake {
  0% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-10px);
  }
  50% {
    transform: translateX(10px);
  }
  75% {
    transform: translateX(-10px);
  }
  100% {
    transform: translateX(0);
  }
}

/* Estilos do Modal de Aniversário */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10000;
}

.modal-content {
  width: 600px;
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  text-align: center;
  color: rgb(54, 52, 52);
}
.modal-content > p {
  color: rgb(54, 52, 52);
}

.modal-content h2 {
  margin-bottom: 10px;
}

.modal-content p {
  margin-bottom: 20px;
}

.modal-content button {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.modal-content button:hover {
  background-color: #3aa876;
}

/* Estilos do Bolo */
@import url(https://fonts.googleapis.com/css?family=Lato:300italic);

html,
body {
  height: 100%;
}

body {
  background: rgb(162, 123, 204);
  background: -moz-radial-gradient(
    center,
    ellipse cover,
    rgba(162, 123, 204, 1) 0%,
    rgba(108, 82, 153, 1) 100%
  );
  background: -webkit-gradient(
    radial,
    center center,
    0px,
    center center,
    100%,
    color-stop(0%, rgba(162, 123, 204, 1)),
    color-stop(100%, rgba(108, 82, 153, 1))
  );
  background: -webkit-radial-gradient(
    center,
    ellipse cover,
    rgba(162, 123, 204, 1) 0%,
    rgba(108, 82, 153, 1) 100%
  );
  background: -o-radial-gradient(
    center,
    ellipse cover,
    rgba(162, 123, 204, 1) 0%,
    rgba(108, 82, 153, 1) 100%
  );
  background: -ms-radial-gradient(
    center,
    ellipse cover,
    rgba(162, 123, 204, 1) 0%,
    rgba(108, 82, 153, 1) 100%
  );
  background: radial-gradient(
    ellipse at center,
    rgba(162, 123, 204, 1) 0%,
    rgba(108, 82, 153, 1) 100%
  );
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#a27bcc', endColorstr='#6c5299', GradientType=1);
}

.cake {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -50px;
  margin-top: -50px;
  width: 100px;
  height: 100px;
}

.cake:after {
  background: rgba(235, 227, 225, 1);
  border-radius: 100px;
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100px;
  height: 2px;
}

.candle {
  background: rgba(255, 255, 255, 1);
  border-radius: 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -2.5px;
  margin-top: -8.33px;
  width: 5px;
  height: 16.67px;
}

.candle:after,
.candle:before {
  background: rgba(255, 0, 0, 0.4);
  content: "";
  position: absolute;
  width: 100%;
  height: 2.22px;
}

.candle:after {
  top: 25%;
  left: 0;
}

.candle:before {
  top: 45%;
  left: 0;
}

.fire {
  border-radius: 100%;
  box-shadow: 0 0 40px 10px rgba(248, 233, 209, 0.2);
  position: absolute;
  top: -12px;
  left: 50%;
  margin-left: -3.33px;
  width: 6.67px;
  height: 12.5px;
}

.fire:nth-child(1) {
  animation: fire 2s infinite;
}

.fire:nth-child(2) {
  animation: fire 1.5s infinite;
}

.fire:nth-child(3) {
  animation: fire 1s infinite;
}

.fire:nth-child(4) {
  animation: fire 0.5s infinite;
}

.fire:nth-child(5) {
  animation: fire 0.2s infinite;
}

@keyframes fire {
  0% {
    background: rgba(254, 248, 97, 0.5);
    transform: translateY(0) scale(1);
  }
  50% {
    background: rgba(255, 50, 0, 0.1);
    transform: translateY(-20px) scale(0);
  }
  100% {
    background: rgba(254, 248, 97, 0.5);
    transform: translateY(0) scale(1);
  }
}

.coverage {
  background: #c77dff;
  border-radius: 50px;
  position: absolute;
  top: 60%;
  left: 50%;
  margin-left: -27.78px;
  margin-top: -5px;
  width: 55.56px;
  height: 12.5px;
  z-index: 10;
}

.coverage:after,
.coverage:before {
  background: #c77dff;
  border-radius: 100px;
  content: "";
  position: absolute;
  width: 5px;
  height: 10px;
}

.coverage:after {
  top: 6.67px;
  right: 14.29px;
}

.coverage:before {
  top: 10px;
  right: 9.09px;
}

.mid-cake {
  background: rgba(109, 56, 38, 1);
  position: absolute;
  bottom: 0;
  left: 50%;
  margin-left: -25px;
  width: 50px;
  height: 33.33px;
}

.mid-cake:after,
.mid-cake:before {
  background: rgba(236, 231, 227, 0.6);
  content: "";
  position: absolute;
  width: 100%;
  height: 5px;
}

.mid-cake:after {
  top: 30%;
  left: 0;
}

.mid-cake:before {
  top: 60%;
  left: 0;
}

h1,
p {
  font-family: "Lato", sans-serif;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  width: 100%;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

h1 {
  color: rgba(108, 82, 153, 1);
  font-size: 1em;
  margin-top: 6.8em;
}

p {
  color: rgba(236, 231, 227, 0.6);
  font-size: 0.8em;
  line-height: 2em;
}

:root {
  --red: red;
  --pink: pink;
  --green: #20b1a3;
}

body {
  background: #f4bcd3;
}

.box-canvas {
  position: relative;
  margin: auto;
  display: block;
  margin-bottom: 8%;
  width: 230px;
  height: 600px;
  animation: floatUp 5s infinite linear;
}

@keyframes floatUp {
  0% {
    transform: translateY(100vh);
  }

  100% {
    transform: translateY(-480px);
  }
}

.red {
  --balloon-color: var(--red);
  --highlight-color: #fc9999;
  --top-initial: 40px;
  --string-angle: -20deg;
}

.yellow {
  --balloon-color: #f2f24b;
  --highlight-color: #e5e570;
  --top-initial: 80px;
  --left-initial: 50px;
  --string-angle: -8deg;
}

.green {
  --balloon-color: var(--green);
  --highlight-color: #bad6d3;
  --top-initial: 0;
  --left-initial: 80px;
  --string-angle: 1deg;
}

.blue {
  --balloon-color: dodgerblue;
  --highlight-color: #6ab5fc;
  --left-initial: 100px;
  --top-initial: 110px;
  --string-angle: 10deg;
}

.orange {
  --balloon-color: orange;
  --highlight-color: #f9b94a;
  --left-initial: 140px;
  --top-initial: 50px;
  --string-angle: 18deg;
}

.balloon-wrapper {
  position: absolute;
  left: var(--left-initial);
  top: var(--top-initial);
  width: 85px;
}

.string {
  position: absolute;
  top: 110px;
  left: 42px;
  transform: rotate(var(--string-angle));
  transform-origin: top left;
  width: 2px;
  height: 250px;
  background: #50535e;
}

.balloon {
  position: absolute;
  width: 85px;
  height: 100px;
  background: var(--balloon-color);
  border-radius: 50%;
}

.balloon::before {
  content: "";
  position: absolute;
  right: 20px;
  top: 15px;
  width: 15px;
  height: 35px;
  box-shadow: 5px 0 0 var(--highlight-color);
  border-radius: 50%;
  transform: rotate(-30deg);
}

.balloon::after {
  content: "";
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: -15px;
  width: 25px;
  height: 20px;
  background: var(--balloon-color);
  clip-path: polygon(20% 0%, 80% 0%, 100% 100%, 0% 100%);
}
</style>
