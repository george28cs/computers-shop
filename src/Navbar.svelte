<script>
  import { createEventDispatcher } from "svelte";
  import logo from './assets/images/logo.png';

  export let cartItems;
  $: cartLength = cartItems.length;

  let activeLink = "home";
  const dispatch = createEventDispatcher();

  function cartClicked() {
    dispatch("cartclicked", true);
    activeLink = "cart";
  }

  function homeClicked() {
    dispatch("homeclicked", false);
    activeLink = "home";
  }
</script>

<style>
  ul.topnav {
    list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;
    background-color: #232f3e;
  }

  ul.topnav li {
    float: left;
  }

  ul.topnav li a {
    display: block;
    color: white;
    text-align: center;
    padding: 1.5rem 1rem;
    text-decoration: none;
  }

  ul.topnav li a:hover:not(.active) {
    background-color: #111;
  }

  ul.topnav li a.active {
    background-color: #ff9900;
  }

  ul.topnav li.right {
    float: right;
  }

  .logo {
    padding: 0.6rem 1rem 0rem 1rem;
    height: 3rem;
    width: 6rem;
  }

  .cart {
    position: relative;
    top: 0.3rem;
    padding: 0rem 0rem 0rem 0.2rem;
    height: 1.3rem;
    width: 1.3rem;
  }

  .badge {
    width: 1.5rem;
    background-color: #ff9900;
    border-radius: 20rem;
    float: right;
  }

  @media screen and (max-width: 600px) {
    ul.topnav li.right,
    ul.topnav li {
      float: none;
    }
  }
</style>

<ul class="topnav">
  <li>
    <img class="logo" src={logo} alt="Logo" />
  </li>
  <li>
    <a
      class:active={activeLink === 'home'}
      href
      on:click|preventDefault={homeClicked}>
      Home
    </a>
  </li>
  <li class="right">
    <a
      class:active={activeLink === 'cart'}
      href
      on:click|preventDefault={cartClicked}>
      Cart
      <img class="cart" src="https://image.flaticon.com/icons/png/512/126/126083.png" alt="Cart" />
      {#if cartLength > 0}
        <div class="badge">{cartLength}</div>
      {/if}
    </a>
  </li>
</ul>
