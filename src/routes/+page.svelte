<!-- Home.svelte -->

<script>
  import { onMount } from "svelte";

  onMount(() => {
    window.addEventListener("keydown", handleKeyDown);
    window.addEventListener("keyup", handleKeyUp);

    // Cleanup the event listener when the component is destroyed
    return () => {
      window.removeEventListener("keydown", handleKeyDown);
      window.removeEventListener("keyup", handleKeyUp);
    };
  });

  // Define an array to hold the square data
  let squares = [
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
    { color: "primary", text: "" },
  ];
  let keyStates = {};
  const columns = 5;
  const rows = 6;
  const max = columns * rows;
  let rowLimit = columns - 1;
  let lowerLimit = 0;
  let index = 0;

  /**
   * @param {{ key: string; }} event
   */
  function handleKeyDown(event) {
    const pressedKey = event.key.toLowerCase();
    // @ts-ignore
    if (keyStates[pressedKey] == null) keyStates[pressedKey] = true;
    // @ts-ignore
    if (keyStates[pressedKey]) {
      let element = handleKey(event);
      if (element) {
        pressAnim(element);
        if (pressedKey == "enter") submitRow();
        else if (pressedKey == "backspace") removeSquareText();
        else setSquareText(pressedKey);
      }
      // @ts-ignore
      keyStates[pressedKey] = false;
    }
  }
  /**
   * @param {any} event
   */
  function handleKeyUp(event) {
    const pressedKey = event.key.toLowerCase();
    let element = handleKey(event);
    if (element) {
      releaseAnim(element);
    }
    // @ts-ignore
    keyStates[pressedKey] = true;
  }
  /**
   * @param {{ key: any; }} event
   */
  function handleKey(event) {
    const pressedKey = event.key.toLowerCase();
    const keyboard = document.getElementById("onsKyb");
    let keyElements = keyboard?.querySelectorAll(`.kbd`);
    let element = null;
    // console.log("pressed key:", pressedKey);
    if (pressedKey == "enter" || pressedKey == "backspace") {
      element = document.getElementById(`kbd-${pressedKey}`);
    } else if (pressedKey.length == 1) {
      keyElements?.forEach((elem) => {
        if (elem.innerHTML == pressedKey) {
          element = elem;
        }
      });
    }
    return element;
  }
  /**
   * @param {{ style: { color: string; }; }} elem
   */
  function pressAnim(elem) {
    elem.style.color = "red";
  }
  /**
   * @param {{ style: { color: string; }; }} elem
   */
  function releaseAnim(elem) {
    elem.style.color = "";
  }
  /**
   * @param {string} letter
   */
  function setSquareText(letter) {
    if (index <= rowLimit) {
      console.log("index:", index);
      console.log("rowLimit:", rowLimit);
      if(squares[index].text == ""){
        squares[index].text = letter;
        squares[index].color = "secondary";
        if (index < rowLimit) index++;  
      }
    }
  }
  function removeSquareText() {
    console.log("index:", index);
    if (index >= lowerLimit) {
      squares[index].text = "";
      squares[index].color = "primary";
      if (index - 1 >= lowerLimit) index--;
    }
  }
  function submitRow() {
    if (index == rowLimit) {
      // send information to server get results etc

      // update index
      lowerLimit += columns;
      if(rowLimit < max) rowLimit += columns;
      index++;
      console.log("rowLimit:", rowLimit);
    }
  }
</script>

<div class="hidden bg-primary bg-secondary" />
<div class="min-h-screen flex flex-col justify-center items-center">
  <div class="grid grid-cols-5 gap-4">
    {#each squares as square (square)}
      <div class="bg-{square.color} square p-4 text-center border-2">
        <span>{square.text}</span>
      </div>
    {/each}
  </div>

  <div id="onsKyb" class="mt-4">
    <div class="flex justify-center gap-1 my-1 w-full">
      <kbd class="kbd">q</kbd>
      <kbd class="kbd">w</kbd>
      <kbd class="kbd">e</kbd>
      <kbd class="kbd">r</kbd>
      <kbd class="kbd">t</kbd>
      <kbd class="kbd">y</kbd>
      <kbd class="kbd">u</kbd>
      <kbd class="kbd">i</kbd>
      <kbd class="kbd">o</kbd>
      <kbd class="kbd">p</kbd>
      <kbd class="kbd" id="kbd-backspace">⌫</kbd>
    </div>
    <div class="flex justify-center gap-1 my-1 w-full">
      <kbd class="kbd">a</kbd>
      <kbd class="kbd">s</kbd>
      <kbd class="kbd">d</kbd>
      <kbd class="kbd">f</kbd>
      <kbd class="kbd">g</kbd>
      <kbd class="kbd">h</kbd>
      <kbd class="kbd">j</kbd>
      <kbd class="kbd">k</kbd>
      <kbd class="kbd">l</kbd>
    </div>
    <div class="flex justify-center gap-1 my-1 w-full">
      <kbd class="kbd">z</kbd>
      <kbd class="kbd">x</kbd>
      <kbd class="kbd">c</kbd>
      <kbd class="kbd">v</kbd>
      <kbd class="kbd">b</kbd>
      <kbd class="kbd">n</kbd>
      <kbd class="kbd">m</kbd>
      <kbd class="kbd" id="kbd-enter">↵</kbd>
    </div>
  </div>
</div>

<style lang="postcss">
  /* :global(html) {
    background-color: theme(colors.gray.100);
  } */
  .square {
    width: 75px;
    height: 75px;
  }
  .kbd {
    font-size: large;
  }
</style>
