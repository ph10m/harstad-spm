<script>
import SpinningPiece from "./SpinningPiece.svelte";
import { Colors } from './consts/colors';
import { Questions } from './consts/questions';
import { elasticIn, elasticInOut, elasticOut } from 'svelte/easing';
import { fade } from 'svelte/transition';



// exports
export let rotation;
export let active;
export let timer;

// animations
const spin = (node, { duration }) => {
  setTimeout(() => console.log('done'), 2000)
  return {
    duration,
    css: t => `transform: rotate(${elasticInOut(t) * rotation}deg);`
  }
}

// local vars
const triangleHeight = Math.min(window.innerWidth, window.innerHeight) / 4
const trianglePx = `${triangleHeight}px`;

const triangleDegree = 360 / Questions.length;
const degrees = (i) => `${(triangleDegree) * i}deg`;
const triangleWidth = Math.round(Math.tan(triangleDegree / 2 * Math.PI / 180) * triangleHeight, 2);
</script>

{#if active}
  <div
    id="wheel"
    in:spin={{duration: (timer + 0.5) * 1000}}
    out:fade={{duration: 200}}
    style='--width: {trianglePx};'
  >
    {#each Questions as _, i}
      <SpinningPiece
        idx={i}
        rot={degrees(i)}
        color={Colors[i]}
        width={`${triangleWidth}px`}
        height={`${triangleHeight}px`}
      />
    {/each} 
  </div>
{:else}
  <div id='wheel'
    style='
    --width: {trianglePx};
    --rot: {rotation}deg;
    '>
    {#each Questions as _, i}
      <SpinningPiece
        idx={i}
        rot={degrees(i)}
        color={Colors[i]}
        width={`${triangleWidth}px`}
        height={`${triangleHeight}px`}
      />
    {/each} 
  </div>
{/if}

<style>
#wheel {
  transition: all 1s ease;
  transform: rotate(0deg);
  position: fixed;
  bottom: 90%;
  left: 50%;
  margin-left: calc(-0.5 * var(--width));
  transform-origin: calc(var(--width)/2) var(--width) 0px;
}
</style>