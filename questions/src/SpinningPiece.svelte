<script>
  export let question;
  export let idx;
  export let color;
  export let rot;
  export let width;
  export let height;
  let hovering = false;
  const position = idx * 10 + "px";
  console.log(width, height);
</script>


<div
  class={`piece ${hovering && "hover"}`}
  style='
  --slice-position: {position};
  --bg: {color};
  --rot: {rot};
  --w: {width};
  --h: {height};
  '
>
  <div
    class={`piece-content`}
    on:mouseleave={() => hovering = false}
    on:mouseover={() => hovering = true}
  >
    Kategori {idx + 1}
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
    /* border-radius: var(--h)/calc(var(--h)/3.3); */
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
    margin-left: calc(-0.5 * var(--w) * 0.9 - 20px); /* center conetent on the piece */
    padding: 50px 20px;
    transition: font-size .2s linear;
  }
  .hover {
    /* z-index: 100; */
    border-color: lightgray transparent transparent transparent;
  }
</style>