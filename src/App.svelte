<script>
  import Navbar from "./Navbar.svelte";
  import Card from "./Card.svelte";
  import Cart from "./Cart.svelte";
  import {onMount} from 'svelte';

  let displayCart = false;
  let cartItems = [];
  let products = [];

  const API = "https://computers-api.herokuapp.com/products"

  onMount( async () => {
    const response = await fetch(API)
    products = await response.json()
    products = await products.body
  })

  function cartClicked(event) {
    displayCart = event.detail;
  }

  function homeClicked(event) {
    displayCart = event.detail;
  }

  function addToCart(event) {
    const selectedProdId = event.detail;
    const isAlreadyInCart = cartItems.findIndex(
      (prod) => prod.id === selectedProdId
    );
    if (isAlreadyInCart > -1) {
      cartItems[isAlreadyInCart].quantity += 1;
    } else {
      cartItems = [
        ...cartItems,
        { ...products.find((prod) => prod.id === selectedProdId), quantity: 1 },
      ];
    }
  }
</script>

<style>
  section {
    width: 80rem;
    margin: auto;
  }
  p.cart-items-loading{
    padding-top: 10rem;;
    text-align: center;
  }
</style>

<Navbar
  on:cartclicked={cartClicked}
  on:homeclicked={homeClicked}
  bind:cartItems />

{#if displayCart}
  <section>
    <Cart bind:cartItems />
  </section>
{:else}
  <section>
    {#each products as product}
      <Card
        on:addcart={addToCart}
        productId={product.id}
        productTitle={product.title}
        productPrice={product.price}
        productImage={product.image}>
        <p slot="description">{product.description}</p>
      </Card>
      {:else}
      <p class="cart-items-loading">Loading...</p>
    {/each}
  </section>
{/if}
