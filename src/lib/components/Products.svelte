<script>
    import { productData } from "$lib/store/datastore.store";
    import { onMount } from "svelte";
    import Zoekfilter from "./Zoekfilter.svelte";
    $: products = shuffleArray($productData);

    function shuffleArray(arr) {
        for (let i = arr.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1)); // Random index
            [arr[i], arr[j]] = [arr[j], arr[i]]; // Swap elements
        }
        return arr;
    }
</script>

<main class="p-4 flex">
    <!-- Empty Box -->
    <aside class=" w-1/4 p-4 rounded-2xl mr-4">
        <Zoekfilter />
    </aside>

    <section class="w-full max-w-7xl mx-auto px-4">
        <h1 class="text-2xl font-bold mb-8 text-center">Beschikbare Items</h1>
        <div class="flex flex-wrap justify-center gap-8">
            {#each products as product}
                <div
                    class="flex flex-col justify-between w-64 bg-white rounded-xl shadow-md overflow-hidden transition-transform relative"
                >
                    <div>
                        <div class="relative">
                            <img
                                src={product.image}
                                alt={product.name}
                                class="w-full h-64 hover:scale-110 transition-transform object-cover"
                            />
                            <div class="absolute top-4 right-4 z-10">
                                <span
                                    class="inline-flex items-center px-3 py-1 rounded-full text-sm font-bold
                            {product.type === 'Kopen'
                                        ? 'bg-red-100 text-red-900'
                                        : product.type === 'Ruilen'
                                          ? 'bg-blue-100 text-blue-900'
                                          : product.type === 'Lenen'
                                            ? 'bg-green-100 text-green-900'
                                            : 'bg-gray-100 text-gray-800'}"
                                >
                                    {product.type}
                                </span>
                            </div>
                        </div>
                        <div class="p-4">
                            <h2 class="text-xl font-semibold mb-2 truncate">
                                {product.name}
                            </h2>
                            <p class="text-sm mb-2 line-clamp-3">
                                {product.description}
                            </p>
                            <p class="text-sm text-gray-600">
                                Category: {product.productType}
                            </p>
                        </div>
                    </div>
                    <div class="p-4 mt-auto">
                        <div class="flex flex-col space-y-2">
                            {#if product.type === "Kopen"}
                                <button
                                    class="text-white font-semibold w-full py-2 rounded-md bg-red-500 hover:bg-red-600"
                                >
                                    Bieden
                                </button>
                                <button
                                    class="text-white font-semibold w-full py-2 rounded-md bg-green-500 hover:bg-green-600"
                                >
                                    Stuur Bericht
                                </button>
                            {:else if product.type === "Ruilen"}
                                <button
                                    class="text-white font-semibold w-full py-2 rounded-md bg-blue-500 hover:bg-blue-600"
                                >
                                    Ruil Voorstel
                                </button>
                                <button
                                    class="text-white font-semibold w-full py-2 rounded-md bg-green-500 hover:bg-green-600"
                                >
                                    Stuur Bericht
                                </button>
                            {:else if product.type === "Lenen"}
                                <button
                                    class="text-white font-semibold w-full py-2 rounded-md bg-green-500 hover:bg-green-600"
                                >
                                    Stuur Bericht
                                </button>
                            {:else}
                                <button
                                    class="text-white font-semibold w-full py-2 rounded-md bg-gray-500 hover:bg-gray-600"
                                >
                                    Stuur Bericht
                                </button>
                            {/if}
                        </div>
                    </div>
                </div>
            {/each}
        </div>
    </section>
</main>
