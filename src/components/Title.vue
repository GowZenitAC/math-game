<template>
  <div>

    <h1 class="title"><span class="text-1">Math </span><span class="text">HangMan</span></h1>
    <h3>Tercer Concurso de Matemáticas</h3>
  </div>
</template>
<script>
export default {

}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Anton&family=Raleway:wght@300&family=Space+Grotesk:wght@500&display=swap');

h3 {
  font-family: 'Anton', sans-serif;
  letter-spacing: 1px;
  font-size: 2.5rem;
  animation: bounce-in-left 1.1s 4s both;
  text-align: center;
  background: linear-gradient(90deg, rgb(192, 189, 20), rgb(245, 63, 7));
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.title {
  font-size: 5rem;
  margin-top: 3rem;
  text-align: center;
  letter-spacing: 2px;
  font-family: 'Anton', sans-serif;
  color: #f7f7f7;
  /* -webkit-box-reflect: below 1px linear-gradient(transparent, #0004); */
}

.text {
  background:  rgb(38, 14, 0);
  background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-left: 6px;
  display: inline-block;
  animation: roll-in-right 0.9s ease-in 2s both;
}

.text-1 {
  background:  rgb(243, 143, 4);
  background-clip: text; 
  -webkit-text-fill-color: transparent;
  display: inline-block;
  animation: swing-in-left-fwd 0.8s cubic-bezier(0.175, 0.885, 0.320, 1.275) 1s both;
}

@keyframes swing-in-left-fwd {
  0% {
    transform: rotateY(100deg);
    transform-origin: left;
    opacity: 0;
  }

  100% {
    transform: rotateY(0);
    transform-origin: left;
    opacity: 1;
  }
}

@keyframes roll-in-right {
  0% {
    transform: translateX(-1000px) rotate(-540deg);
    opacity: 0;
  }

  100% {
    transform: translateX(0) rotate(0deg);
    opacity: 1;
  }
}

@keyframes bounce-in-left {
  0% {
    transform: translateX(-600px);
    animation-timing-function: ease-in;
    opacity: 0;
  }

  38% {
    transform: translateX(0);
    animation-timing-function: ease-out;
    opacity: 1;
  }

  55% {
    transform: translateX(-68px);
    animation-timing-function: ease-in;
  }

  72% {
    transform: translateX(0);
    animation-timing-function: ease-out;
  }

  81% {
    transform: translateX(-28px);
    animation-timing-function: ease-in;
  }

  90% {
    transform: translateX(0);
    animation-timing-function: ease-out;
  }

  95% {
    transform: translateX(-8px);
    animation-timing-function: ease-in;
  }

  100% {
    transform: translateX(0);
    animation-timing-function: ease-out;
  }
}
</style>