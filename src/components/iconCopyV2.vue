<style scoped>
/* nongol settings 👇 */

.copy {
  /* button */
  --button-bg: #353434;
  --button-hover-bg: #464646;
  --button-text-color: #cccccc;
  --button-hover-text-color: #8bb9fe;
  --button-border-radius: 10px;
  --button-diameter: 36px;
  --button-outline-width: 1px;
  --button-outline-color: rgb(141, 141, 141);
  /* nongol */
  --nongol-bg: #f4f3f3;
  --toolptip-border-radius: 4px;
  --nongol-font-family: Menlo, Roboto Mono, monospace;
  /* 👆 this field should not be empty */
  --nongol-font-size: 12px;
  /* 👆 this field should not be empty */
  --tootip-text-color: rgb(50, 50, 50);
  --nongol-padding-x: 7px;
  --nongol-padding-y: 7px;
  --nongol-offset: 8px;
  /* --nongol-transition-duration: 0.3s; */
  /* 👆 if you need a transition, 
    just remove the comment,
    but I didn't like the transition :| */
}

.copy {
  box-sizing: border-box;
  width: var(--button-diameter);
  height: var(--button-diameter);
  border-radius: var(--button-border-radius);
  background-color: var(--button-bg);
  color: var(--button-text-color);
  border: none;
  cursor: pointer;
  position: relative;
  outline: none;
}

.nongol {
  position: absolute;
  opacity: 0;
  visibility: 0;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
  font: var(--nongol-font-size) var(--nongol-font-family);
  color: var(--tootip-text-color);
  background: var(--nongol-bg);
  padding: var(--nongol-padding-y) var(--nongol-padding-x);
  border-radius: var(--toolptip-border-radius);
  pointer-events: none;
  transition: all var(--nongol-transition-duration)
    cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.nongol::before {
  content: attr(data-text-initial);
}

.nongol::after {
  content: "";
  position: absolute;
  bottom: calc(var(--nongol-padding-y) / 2 * -1);
  width: var(--nongol-padding-y);
  height: var(--nongol-padding-y);
  background: inherit;
  left: 50%;
  transform: translateX(-50%) rotate(45deg);
  z-index: -999;
  pointer-events: none;
}

.copy svg {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.checkmark {
  display: none;
}

/* actions */

.copy:hover .nongol,
.copy:focus:not(:focus-visible) .nongol {
  opacity: 1;
  visibility: visible;
  top: calc((100% + var(--nongol-offset)) * -1);
}

.copy:focus:not(:focus-visible) .nongol::before {
  content: attr(data-text-end);
}

.copy:focus:not(:focus-visible) .clipboard {
  display: none;
}

.copy:focus:not(:focus-visible) .checkmark {
  display: block;
}

.copy:hover,
.copy:focus {
  background-color: var(--button-hover-bg);
}

.copy:active {
  outline: var(--button-outline-width) solid var(--button-outline-color);
}

.copy:hover svg {
  color: var(--button-hover-text-color);
}
</style>

<template>
  <button class="copy">
    <span
      data-text-end="Copied!"
      data-text-initial="Copy to clipboard"
      class="nongol"
    ></span>
    <span>
      <svg
        xml:space="preserve"
        style="enable-background: new 0 0 512 512"
        viewBox="0 0 6.35 6.35"
        y="0"
        x="0"
        height="20"
        width="20"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        class="clipboard"
      >
        <g>
          <path
            fill="currentColor"
            d="M2.43.265c-.3 0-.548.236-.573.53h-.328a.74.74 0 0 0-.735.734v3.822a.74.74 0 0 0 .735.734H4.82a.74.74 0 0 0 .735-.734V1.529a.74.74 0 0 0-.735-.735h-.328a.58.58 0 0 0-.573-.53zm0 .529h1.49c.032 0 .049.017.049.049v.431c0 .032-.017.049-.049.049H2.43c-.032 0-.05-.017-.05-.049V.843c0-.032.018-.05.05-.05zm-.901.53h.328c.026.292.274.528.573.528h1.49a.58.58 0 0 0 .573-.529h.328a.2.2 0 0 1 .206.206v3.822a.2.2 0 0 1-.206.205H1.53a.2.2 0 0 1-.206-.205V1.529a.2.2 0 0 1 .206-.206z"
          ></path>
        </g>
      </svg>
      <svg
        xml:space="preserve"
        style="enable-background: new 0 0 512 512"
        viewBox="0 0 24 24"
        y="0"
        x="0"
        height="18"
        width="18"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        class="checkmark"
      >
        <g>
          <path
            data-original="#000000"
            fill="currentColor"
            d="M9.707 19.121a.997.997 0 0 1-1.414 0l-5.646-5.647a1.5 1.5 0 0 1 0-2.121l.707-.707a1.5 1.5 0 0 1 2.121 0L9 14.171l9.525-9.525a1.5 1.5 0 0 1 2.121 0l.707.707a1.5 1.5 0 0 1 0 2.121z"
          ></path>
        </g>
      </svg>
    </span>
  </button>
</template>
