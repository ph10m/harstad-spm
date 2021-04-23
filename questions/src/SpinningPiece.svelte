<script>
  export let idx;
  export let color;
  export let rot;
  export let width;
  export let height;

  const position = idx * 10 + "px";
</script>


<div
  class='piece'
  style='
  --slice-position: {position};
  --bg: {color};
  --rot: {rot};
  --w: {width};
  --h: {height};
  '
>
  <div class={`piece-content`}>
    <!-- { idx + 1} -->
  </div>
</div>

<style>
  .piece {
    position: fixed;
    transform-origin: bottom;
    transform: rotate(var(--rot));
    margin-left: calc(var(--width) / 10);
    width: 0;
    height: 0;
    border-style: solid;
    border-width: var(--h) var(--w) 0 var(--w);
    border-color: var(--bg) transparent transparent transparent;
    /* border-radius: var(--h)/calc(var(--h)/3); */
    border-radius: 50% 50% 50% 50% / 20% 20% 80% 80%;
    /* transition: all 1s cubic-bezier(0, 0.99, 0.44, 0.99); */
  }
  .piece-content {
    font-size: 1.5em;
    cursor: pointer;
    max-width: calc(var(--w)  * 0.9);
    text-align: center;
    word-wrap: break-word;
    position: absolute;
    margin-top: calc(-1 * var(--h)); 
    margin-left: calc(-0.5 * var(--w)); /* center conetent on the piece */
    padding: 50px 20px;
    transition: font-size .2s linear;
  }
  .dot {
    position: absolute;
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: black;
    margin-top: calc(-0.98 * var(--h)); 
  }
</style>