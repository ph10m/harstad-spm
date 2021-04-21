<script>
import { Questions } from './consts/questions';

  import RotatingWheel from './RotatingWheel.svelte';
  import SpinningPiece from './SpinningPiece.svelte'

  const lowPrimes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97];

  const animationTimer = .5;
  let rotationDegrees = 0;
  let prevRot = 0;

  const random = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

  let spinning = false;
  let landingQuestion = 0;
  const onSpin = () => {
    spinning = true;

    const pieceDeg = 360 / Questions.length;
    rotationDegrees = Math.floor(random(5*360, 10*360));
    console.log('rot', rotationDegrees % 360 )

    prevRot = rotationDegrees;
    const approxAngle = 8 - Math.ceil(((prevRot + rotationDegrees) % 360) / pieceDeg);
    console.log(approxAngle % 8)
    // console.log('question idx', landingQuestion)
    setTimeout(() => {
      spinning = false;
    }, animationTimer * 1000)
  }  
</script>

<main>
  <link href="//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css" rel="stylesheet">
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
</style>