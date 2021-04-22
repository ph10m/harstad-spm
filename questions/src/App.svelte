<script>
  import { Questions } from './consts/questions';
  import RotatingWheel from './RotatingWheel.svelte';
  import SpinningPiece from './SpinningPiece.svelte'
  import { fly } from 'svelte/transition';
  import { cubicInOut } from 'svelte/easing';
import fadeScale from './animations/fadeScale';


  let questionBoxOpen = false;

  let animationTimer = 0.5;
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
    landingQuestion = (Math.ceil(normRot / pieceDeg) * pieceDeg) / pieceDeg;
    console.log('question idx', landingQuestion)

    setTimeout(() => {
      spinning = false;
      // questionBoxOpen = true;
    }, animationTimer * 1000)
  }  
  const triangleHeight = Math.min(window.innerWidth, window.innerHeight) / 3;
  console.log("height?!?!?", triangleHeight);
</script>

<div class='bg-image'/>
<main style='
--timer: {animationTimer}s;
--triangleHeight: {`${triangleHeight}px`};
'>
  <link href="//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css" rel="stylesheet">
  {#if (questionBoxOpen)}
    <div
      class='question-box'
      transition:fadeScale={{
        delay: 500,
        duration: 1000,
        easing: cubicInOut,
        baseScale: 0.1
      }}
     >
      ok
      ok
      ok
      ok
      ok
    </div>
  {:else}
    <div
      class={`ticker ${spinning && "spinning-ticker"}`}
      style=''
    />
    <RotatingWheel
      active={spinning}
      previousRot={prevRot}
      rotation={rotationDegrees}
      timer={animationTimer}
      landingQuestion={landingQuestion}
    />
    <div
      on:click={() => !spinning && onSpin()}
      class={`spin-button ${spinning && "disabled"}`}
    >
      {spinning ? "Snurre..." : "Still mæ et spørsmål!"}
    </div>
  {/if}
</main>

<style>
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
  background: black;
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
  margin-bottom: 50px;
  align-items: center;
  justify-content: center;
  background-image: linear-gradient(to left bottom, #5daeef, #00b8e2, #00bcb4, #12b871, #82ad27);
  border-radius: 28% 72% 47% 53% / 54% 47% 53% 46% ;
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
  0% { transform: rotate(-60deg) }
  3% { transform: rotate(-30deg) }
  5% { transform: rotate(-80deg) }
  7% { transform: rotate(-40deg) }
  10% { transform: rotate(-70deg) }
  13% { transform: rotate(-40deg) }
  15% { transform: rotate(-60deg) }
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