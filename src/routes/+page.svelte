<script lang=ts>
  import { onMount } from 'svelte'
  import Spinner from '$lib/components/spinner.svelte'

  let items: Array<spinnerItemType> = []
  let colorOptions = [
    '#e5e7eb',
    '#334155',
    '#f87171',
    '#f9a8d4',
    '#fb923c',
    '#fbbf24',
    '#a3e635',
    '#34d399',
    '#22d3ee',
    '#60a5fa',
    '#818cf8',
    '#a78bfa',
    '#e879f9',
    '#fb7185'
  ]

  onMount(() => {
    items = [
      {
        label: 'Dad 🦖',
        backgroundColor: '#fb923c'
      },
      {
        label: 'Mom 🐯',
        backgroundColor: '#5eead4'
      },
      {
        label: 'Brooke 🦄',
        backgroundColor: '#f9a8d4'
      }
    ]
  })

  let newItem: spinnerItemType = {
    label: '',
    backgroundColor: ''
  }

  function addItem () {
    console.log('addItem')
    items.push(newItem)
    items = items

    newItem = {
      label: '',
      backgroundColor: ''
    }
  }

  function removeItem (index: number) {
    items.splice(index, 1)
    items = items
  }
</script>

<section class="container">
  <h1>Spinner Fun</h1>
</section>

<section class="container">
  <Spinner bind:items={items} />
</section>

<section class="edit_sec">
  <div class="container">
    <div class="edit_area">
      <form class="form" on:submit|preventDefault={addItem}>
        <h2 class="h4">Add new Item</h2>
        <label>
          <span class="label">Name</span>
          <input type="text" bind:value={newItem.label}>
        </label>

        <div class="color_wrap">
          <label>
            <span class="label">Color</span>
            <input type="color" bind:value={newItem.backgroundColor}>
          </label>
    
          <div class="options_wrap">
            <p class="label">Color Options</p>
            <div class="color_options">
              {#each colorOptions as color}
                <label class="color_option">
                  <div class="color_box" style:background-color={color}></div>
                  <input type="radio" bind:group={newItem.backgroundColor} value={color} />
                </label>
              {/each}
            </div>
          </div>
        </div>

        <div class="btn_wrap">
          <button class="btn" type="submit">Add</button>
        </div>
      </form>

      <div class="edit_items_wrap">
        <h2 class="h4">Edit Items</h2>

        <ul>
          {#each items as item, i}
            <li>
              <div class="color_box" style:background-color={item.backgroundColor}></div>
              <span class="name">{item.label}</span>
              <button class="btn __sm" on:click={() => removeItem(i)}>Remove</button>
            </li>
          {/each}
        </ul>
      </div>
    </div>
  </div>
</section>

<style lang=postcss>
  @import '@styles/mediaQueries.pcss';

  .edit_sec {
    padding-top: var(--size-24);
    padding-bottom: var(--size-24);

    .edit_area {
      @media (--md) {
        display: flex;
        justify-content: center;
        align-items: flex-start;
        gap: var(--size-12);
      }
    }
  }

  h1 {
    text-align: center;
  }

  .color_wrap {
    display: flex;
    justify-content: flex-start;
    align-items: flex-start;
    /* flex-wrap: wrap; */
    gap: var(--size-3);

    .options_wrap {
      flex: 1 1 auto;
    }
  }

  .color_box {
    width: 20px;
    height: 20px;
    border-radius: var(--radius-full);
    border: 1px solid var(--color-white);
  }

  .color_options {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-wrap: wrap;
    gap: var(--size-2);
    margin-bottom: var(--size-4);

    .color_option {
      margin-bottom: 0;
      cursor: pointer;

      input[type=radio] {
        position: absolute;
        appearance: none;
        background-color: transparent;
        border: none;
        width: 0;
        min-width: auto;
        height: 0;
        padding: 0;
        margin: 0;
      }
    }
  }

  .edit_items_wrap {
    ul {
      li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: var(--size-2);

        margin-bottom: var(--size-3);
      }
    }
  }

  .form {
    max-width: 300px;

    p.label {
      display: block;
      font-family: var(--font-body);
      font-size: var(--scale-00);
      font-weight: bold;
      text-transform: uppercase;
      margin-bottom: var(--size-1);
    }
  }
</style>
