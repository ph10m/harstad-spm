<script>
import { Questions } from './consts/questions';

  import RotatingWheel from './RotatingWheel.svelte';
  import SpinningPiece from './SpinningPiece.svelte'

  const lowPrimes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97];

  const animationTimer = 2;
  let rotationDegrees = 0;
  let prevRot = 0;

  const random = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

  let spinning = false;
  let landingQuestion = 0;
  const onSpin = () => {
    spinning = true;

    const pieceDeg = 360 / Questions.length;
    rotationDegrees = Math.floor(random(5*360, 10*360));
    console.log('rot', rotationDegrees % 360)
    prevRot = rotationDegrees;


    const finalRotation = (prevRot + rotationDegrees) % 360;
    console.log('normed rot', finalRotation)
    const normalizedFinal = 360 - finalRotation;
    // if the normalized final is in batches of 
    console.log("final", normalizedFinal);
    console.log("bucket:",  (Math.ceil(normalizedFinal / pieceDeg) * pieceDeg) / pieceDeg);

    // const approxAngle = 8 - Math.ceil(((prevRot + rotationDegrees) % 360) / pieceDeg);
    // console.log(approxAngle % 8)
    // console.log('question idx', landingQuestion)
    setTimeout(() => {
      spinning = false;
    }, animationTimer * 1000)
  }  
</script>

<main>
  <link href="//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css" rel="stylesheet">
  <div
    class={`ticker ${spinning && "spinning-ticker"}`}
    style='--timer: {animationTimer}'
  />
  <RotatingWheel
    active={spinning}
    previousRot={prevRot}
    rotation={rotationDegrees}
    timer={animationTimer}
  />
  {#if !spinning}
    <button on:click={onSpin}>spin!</button>
  {/if}
</main>

<style>
:root {
  background: #222;
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}
.ticker {
  position: fixed;
  top: 10%;
  left: 37%;
  width: 0;
  height: 0;
  border-left: 30px solid transparent;
  border-right: 30px solid transparent;
  border-top: 60px solid black;
  transform: rotate(-25deg);
  z-index: 1000;
}
.ticker::after {
  background: pink;
}
.spinning-ticker {
  animation: tick 2s cubic-bezier(0, 0.99, 0.44, 0.99);
}
@keyframes tick {
  0% { transform: rotate(-50deg) }
  3% { transform: rotate(-40deg) }
  5% { transform: rotate(-50deg) }
  7% { transform: rotate(-40deg) }
  10% { transform: rotate(-50deg) }
  13% { transform: rotate(-40deg) }
  15% { transform: rotate(-50deg) }
  17% { transform: rotate(-40deg) }
  20% { transform: rotate(-50deg) }
  23% { transform: rotate(-40deg) }
  25% { transform: rotate(-50deg) }
  27% { transform: rotate(-40deg) }
  35% { transform: rotate(-50deg) }
  50% { transform: rotate(-40deg) }
  70% { transform: rotate(-50deg) }
  100% { transform: rotate(-40deg) }
}
</style>