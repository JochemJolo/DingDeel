<script>
    import { productData } from "$lib/store/datastore.store";
    import Zoekfilter from "./Zoekfilter.svelte";
    let products = $productData;
</script>

<main class="p-4 flex">
    <!-- Empty Box -->
    <aside class="w-1/4 bg-gray-100 p-4 rounded-2xl mr-4">
        <Zoekfilter />
    </aside>

    <section class="w-full max-w-7xl mx-auto px-4">
        <h1 class="text-3xl font-bold mb-8 text-center">Artikelen</h1>
        <div class="flex flex-wrap justify-center gap-8">
            {#each products as product}
                <div
                    class="flex-shrink-0 w-64 pb-4 bg-white rounded-xl shadow-md overflow-hidden transition-transform hover:scale-105 relative"
                >
                    <img
                        src={product.image}
                        alt={product.name}
                        class="w-full h-64 object-cover"
                    />
                    <div
                        class="absolute top-6 -right-8 transform rotate-45
                {product.type === 'Kopen'
                            ? 'bg-red-500'
                            : product.type === 'Ruilen'
                              ? 'bg-blue-500'
                              : product.type === 'Lenen'
                                ? 'bg-green-500'
                                : 'bg-gray-500'}
                 text-white py-1 px-10 text-sm font-semibold shadow-md"
                    >
                        {product.type}
                    </div>
                    <div class="p-4">
                        <h2 class="text-xl font-semibold mb-2 truncate">
                            {product.name}
                        </h2>
                        <p class="text-sm text-gray-600">
                            Category: {product.productType}
                        </p>
                    </div>
                    <button
                        class="text-white font-semibold w-1/2 py-2 rounded-md
    {product.type === 'Kopen'
                            ? 'bg-red-500 hover:bg-red-600'
                            : product.type === 'Ruilen'
                              ? 'bg-blue-500 hover:bg-blue-600'
                              : product.type === 'Lenen'
                                ? 'bg-green-500 hover:bg-green-600'
                                : 'bg-gray-500 hover:bg-gray-600'}"
                    >
                        {#if product.type === "Kopen"}
                            Bieden
                        {:else if product.type === "Ruilen"}
                            Ruil Voorstel
                        {:else if product.type === "Lenen"}
                            Stuur Bericht
                        {:else}
                            Stuur Bericht
                        {/if}
                    </button>
                </div>
            {/each}
        </div>
    </section>
</main>
