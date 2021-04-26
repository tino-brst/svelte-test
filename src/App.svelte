<script>
  import { onMount } from 'svelte'
  import { VctrApi } from './libs/viewer-api'

  const Shape = {
    box: 'box',
    sphere: 'sphere',
  }

  const Color = {
    blue: 'blue0',
    crimson: 'crimson0',
  }

  const shapeOptions = [
    { label: 'Box', value: Shape.box },
    { label: 'Sphere', value: Shape.sphere },
  ]

  const colorOptions = [
    { label: 'Blue', value: Color.blue },
    { label: 'Crimson', value: Color.crimson },
  ]

  let selectedShape = Shape.box
  let selectedColor = Color.blue

  let viewerApi
  let isViewerApiReady = false

  onMount(async () => {
    viewerApi = new VctrApi('vectary-viewer')
    await viewerApi.init()
    isViewerApiReady = true
  })

  function handleShapeChange(value) {
    viewerApi.setMaterial(value, selectedColor)
    viewerApi.setVisibility(value, true, true)
  }

  function handleColorChange(value) {
    viewerApi.setMaterial(selectedShape, value)
  }

  $: isViewerApiReady && handleShapeChange(selectedShape)
  $: isViewerApiReady && handleColorChange(selectedColor)
</script>

<div class="configurator">
  <vctr-viewer
    class="configurator__viewer"
    id="vectary-viewer"
    model="60872dd8-5cb0-4c79-9a9c-91baf9944a17"
    mouseFollow="0.5"
  />
  <div class="configurator__controls">
    <div class="group">
      <h2 class="group-title">Shape</h2>
      {#each shapeOptions as option}
        <label class="option" class:option_selected={selectedShape === option.value}>
          <input
            class="option__input"
            type="radio"
            bind:group={selectedShape}
            value={option.value}
            checked
          />{option.label}</label
        >
      {/each}
    </div>
    <div class="group">
      <h2 class="group-title">Color</h2>
      {#each colorOptions as option}
        <label class="option" class:option_selected={selectedColor === option.value}>
          <input
            class="option__input"
            type="radio"
            bind:group={selectedColor}
            value={option.value}
            checked
          />{option.label}</label
        >
      {/each}
    </div>
  </div>
</div>

<style>
  .configurator {
    background: whitesmoke;
    height: 100%;
  }

  .configurator__viewer {
    height: 100%;
  }

  .configurator__controls {
    background-color: white;
    border-radius: 8px;
    position: fixed;
    padding: 14px;
    bottom: 10px;
    left: 10px;
    right: 10px;
    box-shadow: 0px 10px 20px 0px rgba(0, 0, 0, 0.1);
  }

  .group:not(:last-child) {
    margin-bottom: 16px;
  }

  .group-title {
    font-size: 18px;
    margin-top: 0;
    margin-bottom: 8px;
  }

  .option {
    display: inline-block;
    border-radius: 8px;
    padding: 4px 8px;
    user-select: none;
    color: rgb(110, 110, 110);
    cursor: pointer;
    transition: all 0.15s;
  }

  .option:not(:last-child) {
    margin-right: 4px;
  }

  .option:hover {
    color: black;
    background-color: whitesmoke;
  }

  .option.option_selected {
    color: black;
    background-color: rgb(230, 230, 230);
    font-weight: 500;
  }

  .option__input {
    appearance: none;
    margin: 0;
  }
</style>
