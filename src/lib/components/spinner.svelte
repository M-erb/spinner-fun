<script lang=ts>
  // @ts-ignore
  import { Wheel } from 'spin-wheel'
  import { browser } from '$app/environment'
  import { onMount } from 'svelte'

  export let items: Array<spinnerItemType> = []

  let wheelEl: HTMLDivElement
  // @ts-ignore
  let wheel
  let selected: string = ''
  let config = {
    borderColor: '#fff',
    borderWidth: 8,
    lineColor: '#fff',
    lineWidth: 8,
    itemLabelFont: 'Fira Sans',
    itemLabelStrokeColor: '#fff',
    itemLabelStrokeWidth: 3,
    itemLabelRadiusMax: .25,
    itemLabelRadius: .75,
    rotationSpeedMax: 500,
    pointerAngle: 0,
    radius: 1,
    items,
    onRest(event: { currentIndex: number, rotation: number, type: string }) {
      const result = config.items[event.currentIndex]
      selected = result.label
    }
  }

  let direction = true
  function spinWheel () {
    const min = 250
    const max = 500
    const rando = Math.floor(Math.random() * (max - min) + min)

    // @ts-ignore
    if (wheel) {
      wheel.pointerAngle = 0
      direction ? wheel.spin(rando) : wheel.spin(-1 * rando)
    }

    direction = !direction
  }

  $: {
    config.items = items
    initWheel()
  }

  onMount(() => initWheel())

  function initWheel () {
    // @ts-ignore
    // if (wheel) wheel.init(config)
    if (wheel) wheel.remove()
    if (browser && items.length) wheel = new Wheel(wheelEl, config)
  }
</script>

<div class="wheel_area">
  <div class="wheel_wrap">
    <button class="spin_btn" on:click={spinWheel}>SPIN</button>
    <div class="wheel_el" bind:this={wheelEl}></div>
  </div>

  <div class="selected_box">
    <p>{ selected ? selected : 'Spin the wheel!' }</p>
    <button class="btn" on:click={spinWheel}>SPIN</button>
  </div>
</div>

<style lang=postcss>
  @import '@styles/mediaQueries.pcss';

  .wheel_area {
    @media (--md) {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: var(--size-4);
  
      .wheel_wrap {
        flex: 1 1 auto;
        width: 50%;
        min-height: 500px;
  
        .wheel_el {
          min-height: 500px;
        }
      }
  
      .selected_box {
        flex: 1 1 auto;
        width: 50%;
        text-align: center;
      }
    }
  }

  .wheel_wrap {
    position: relative;

    .spin_btn {
      position: absolute;
      z-index: 50;
      background-color: var(--color-grey-200);
      border: 4px solid var(--color-white);
      border-radius: var(--radius-full);
      width: 60px;
      height: 60px;
      text-align: center;
      cursor: pointer;
      font-weight: bold;

      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);

      @media (--md) {
        border-width: 8px;
        width: 80px;
        height: 80px;
      }
    }

    .wheel_el {
      position: relative;
      min-height: 343px;

      &::after {
        content: '';
        position: absolute;
        top: -9px;
        left: 50%;
        transform: translateX(-50%) rotate(45deg);

        background-color: var(--color-grey-200);
        border: 4px solid var(--color-white);
        width: 14px;
        height: 14px;

        @media (--md) {
          bottom: -16px;
          border-width: 8px;
          width: 20px;
          height: 20px;
        }
      }
    }
  }

  .selected_box {
    margin-top: var(--size-4);
    text-align: center;

    p {
      font-size: var(--scale-6);
    }
  }
</style>
