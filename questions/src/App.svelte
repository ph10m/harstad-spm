<script>
  import { Questions } from './consts/questions';
  import RotatingWheel from './RotatingWheel.svelte';
  import SpinningPiece from './SpinningPiece.svelte'
  import { fly } from 'svelte/transition';
  import { fade } from 'svelte/transition';
  import { cubicInOut } from 'svelte/easing';
import fadeScale from './animations/fadeScale';
import { Colors } from './consts/colors';


  let questionBoxOpen = true;

  let animationTimer = 2;
  let rotationDegrees = 0;
  let prevRot = 0;

  const random = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

  let spinning = false;
  let landingQuestion = 0;
  const onSpin = () => {
    // animationTimer = random(2, 6);
    spinning = true;

    const pieceDeg = 360 / Questions.length;
    rotationDegrees = Math.floor(random(5*360, 10*360));
    prevRot = rotationDegrees;

    const normRot = 360 - (prevRot + rotationDegrees) % 360;
    landingQuestion = (Math.ceil(normRot / pieceDeg) * pieceDeg) / pieceDeg - 1;
    console.log('question idx', landingQuestion)

    setTimeout(() => {
      spinning = false;
      questionBoxOpen = true;
    }, animationTimer * 1000)
  }  
  const triangleHeight = Math.min(window.innerWidth, window.innerHeight) / 3;
</script>

<div class='bg-image'/>
<main style='
--timer: {animationTimer + .5}s;
--triangleHeight: {`${triangleHeight}px`};
'>
  <link href="//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css" rel="stylesheet">
  {#if (questionBoxOpen)}
    {console.log(Colors[landingQuestion])}
    <div
      class='question-box'
      style="background-color: {Colors[landingQuestion]}"
      transition:fadeScale={{
        delay: 0,
        duration: 500,
        easing: cubicInOut,
        baseScale: 0.5
      }}
    >
      <div class='question-box-closer' on:click={() => questionBoxOpen = false}>
        &#x2716;
      </div>
      <p>{Questions[landingQuestion].text}</p>
    </div>
  {:else}
    <div in:fade={{duration: 200}} out:fade={{duration: 200}}>
      <div class={`ticker ${spinning && 'spinning-ticker'}`} />
      <RotatingWheel
        active={spinning}
        previousRot={prevRot}
        rotation={rotationDegrees}
        timer={animationTimer}
      />
      <div
        on:click={() => !spinning && onSpin()}
        class={`spin-button ${spinning && "disabled"}`}
      >
        {spinning ? "Snurre..." : "Still mæ et spørsmål!"}
      </div>
    </div>
  {/if}
</main>

<style>
:root {
  overflow: hidden;
  overflow-y: hidden;
}
.bg-image {
  height: 100%;
  background-image: url('./assets/generalhagen.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  filter: blur(3px);
}
.question-box {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2em;
  color: black;
}
.question-box p {
  text-shadow: 0px 3px 20px white;
  word-break: break-word;
  margin: 0 10%;
}
.question-box-closer {
  position: fixed;
  bottom: 15%;
  left: 50%;
  width: 50px;
  height: 50px;
  margin-left: -25px;
  justify-content: center;
  align-items: center;
  display: flex;
  border: 2px solid black;
  border-radius: 50%;
  cursor: pointer;
}
.spin-button {
  position: fixed;
  text-align: center;
  z-index: 1000;
  display: flex;
  padding: 15px;
  width: 150px;
  height: 50px;
  bottom: 0%;
  left: 50%;
  margin-left: -90px;
  margin-bottom: 15%;
  align-items: center;
  justify-content: center;
  background-image: linear-gradient(to left bottom, #5daeef, #00b8e2, #00bcb4, #12b871, #82ad27);
  /* border-radius: 28% 72% 47% 53% / 54% 47% 53% 46% ; */
  border: 1px solid black;
  font-weight: 500;
  font-size: 20px;
  cursor: pointer;
}
.disabled {
  background-image: linear-gradient(to left bottom, #1a3267, #004574, #005264, #005a3c, #225e02);
  cursor: auto;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
.ticker {
  position: fixed;
  top: 10%;
  left: 50%;
  width: 0;
  height: 0;
  margin-left: calc(-0.45 * var(--triangleHeight)); /* a small offset from the center (0.5) */
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
  border-top: 40px solid black;
  transform: rotate(-25deg);
  z-index: 1000;
}
.spinning-ticker {
  animation: tick var(--timer) cubic-bezier(0, 0.99, 0.44, 0.99);
  transition: all .5s ease;
}
@keyframes tick {
  0% { transform: rotate(-22deg) }
  12% { transform: rotate(-30deg) }
  17% { transform: rotate(-30deg) }
  20% { transform: rotate(-50deg) }
  23% { transform: rotate(-40deg) }
  25% { transform: rotate(-32deg) }
  27% { transform: rotate(-45deg) }
  30% { transform: rotate(-30deg) }
  33% { transform: rotate(-40deg) }
  35% { transform: rotate(-32deg) }
  37% { transform: rotate(-40deg) }
  40% { transform: rotate(-30deg) }
  45% { transform: rotate(-32deg) }
  50% { transform: rotate(-20deg) }
  80% { transform: rotate(-28deg) }
  100% { transform: rotate(-25deg) }
}
</style>