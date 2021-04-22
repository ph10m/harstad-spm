<script>
import SpinningPiece from "./SpinningPiece.svelte";
import { Colors } from './consts/colors';
import { Questions } from './consts/questions';

// exports
export let previousRot;
export let rotation;
export let active;
export let timer;

// local vars
const triangleHeight = Math.min(window.innerWidth, window.innerHeight) / 3
const trianglePx = `${triangleHeight}px`;

const triangleDegree = 360 / Questions.length;
const degrees = (i) => `${(triangleDegree) * i}deg`;
const triangleWidth = Math.round(Math.tan(triangleDegree / 2 * Math.PI / 180) * triangleHeight, 2);
</script>

<div
  id="wheel" class={active ? "animation" : ""}
 style='
    --width: {trianglePx};
    --timer: {`${timer}s`};
    --deg: {`${previousRot + rotation + 2}deg`};
    --degFallback: {`${previousRot + rotation}deg`};
  '
  >
  {#each Questions as question, i}
    <SpinningPiece
      question={question}
      idx={i}
      rot={degrees(i)}
      color={Colors[i]}
      width={`${triangleWidth}px`}
      height={`${triangleHeight}px`}
    />
  {/each} 
</div>

<style>
#wheel {
  transform: rotate(var(--degFallback));
  position: fixed;
  bottom: 90%;
  left: 50%;
  margin-left: calc(-0.5 * var(--width));
  transform-origin: calc(var(--width)/2) var(--width) 0px;
}

.animation {
  animation: spin var(--timer) cubic-bezier(0, 0.99, 0.44, 0.99);
}

@keyframes spin {
  0% {
    transform: rotate(var(--startDeg));
  }
  90% {
    transform: rotate(var(--deg));
  }
  100% {
    transform: rotate(var(--degFallback));
  }
}
</style>