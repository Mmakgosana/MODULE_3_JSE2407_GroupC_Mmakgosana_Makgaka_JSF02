<script>
    import ProductCard from "./ProductCard.svelte";
    import CardSkeleton from "./CardSkeleton.svelte";
    import { useProductStore } from "../../store/productStore";
    import Error from "../Error.svelte";
  
    let products = [];
    let loading = true;
    let error = null;
  
    const unsubscribe = useProductStore.subscribe(state => {
      products = state.products;
      loading = state.loading;
      error = state.error;
    });
  
    // Clean up the subscription when the component is destroyed
    import { onDestroy } from 'svelte';
    onDestroy(unsubscribe);
  </script>
  
  {#if loading && !error}
    <div class="grid justify-center">
      <div class="lg:max-h-[130rem] max-w-xl mx-auto grid gap-4 grid-cols-1 lg:grid-cols-4 md:grid-cols-2 items-center lg:max-w-none my-4">
        {#each Array(20).fill(null) as _, index}
          <CardSkeleton />
        {/each}
      </div>
    </div>
  {:else if error}
    <div class="grid justify-center">
      <Error {...error} />
    </div>
  {:else}
    <div class="grid justify-center">
      <div class="lg:max-h-[130rem] max-w-xl mx-auto grid gap-4 grid-cols-1 lg:grid-cols-4 md:grid-cols-2 items-center lg:max-w-none my-4">
        {#each products as product (product.id)}
          <ProductCard {...product} />
        {/each}
      </div>
    </div>
  {/if}