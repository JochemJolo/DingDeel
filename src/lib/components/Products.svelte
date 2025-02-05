<script>
    import { productData } from "$lib/store/datastore.store";
    import { onMount } from "svelte";
    import Zoekfilter from "./Zoekfilter.svelte";
    import { filterStore } from "$lib/store/Zoekfilter.store";

    let products = [];
    let filteredProducts = [];
    let currentPage = 1;
    let itemsPerPage = 6; // 2 columns * 3 rows

    // Wait for store to be populated
    $: if ($productData && $productData.length > 0) {
        products = shuffleArray($productData);
        applyFilters();
    }

    $: totalPages = Math.ceil(filteredProducts.length / itemsPerPage);
    $: paginatedProducts = getPaginatedProducts(
        filteredProducts,
        currentPage,
        itemsPerPage,
    );

    filterStore.subscribe(() => {
        applyFilters();
    });

    function applyFilters() {
        filteredProducts = products.filter((product) => {
            const { categories, types } = $filterStore;

            const categoryMatch = Object.keys(categories).some(
                (category) =>
                    categories[category] &&
                    product.productType.toLowerCase() ===
                        category.toLowerCase(),
            );
            const typeMatch = Object.keys(types).some(
                (type) => types[type] && product.type === type,
            );

            const noCategoriesSelected = Object.values(categories).every(
                (v) => !v,
            );
            const noTypesSelected = Object.values(types).every((v) => !v);

            return (
                (noCategoriesSelected || categoryMatch) &&
                (noTypesSelected || typeMatch)
            );
        });

        currentPage = 1; // Reset to first page after filtering
    }

    function getPaginatedProducts(allProducts, page, perPage) {
        const start = (page - 1) * perPage;
        const end = start + perPage;
        return allProducts.slice(start, end);
    }

    function nextPage() {
        if (currentPage < totalPages) currentPage++;
    }

    function prevPage() {
        if (currentPage > 1) currentPage--;
    }

    function shuffleArray(arr) {
        let shuffled = [...arr];
        for (let i = shuffled.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
        }
        return shuffled;
    }
</script>

<main class="p-4 flex">
    <aside class="w-1/4 p-4 rounded-2xl mr-4">
        <Zoekfilter />
    </aside>

    <section class="w-full max-w-7xl mx-auto px-4">
        <h1 class="text-2xl font-bold mb-8 text-center">Beschikbare Items</h1>

        <!-- Pagination controls -->
        <div class="my-8 flex justify-center items-center space-x-2">
            <button
                on:click={prevPage}
                disabled={currentPage === 1}
                class="flex items-center px-3 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 disabled:opacity-50 disabled:pointer-events-none transition-all"
            >
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M15 19l-7-7 7-7"
                    />
                </svg>
            </button>

            <span class="text-lg font-medium px-4">
                Pagina {currentPage} van {totalPages}
            </span>

            <button
                on:click={nextPage}
                disabled={currentPage === totalPages}
                class="flex items-center px-3 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 disabled:opacity-50 disabled:pointer-events-none transition-all"
            >
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M9 5l7 7-7 7"
                    />
                </svg>
            </button>
        </div>

        <div class="grid grid-cols-3 gap-8">
            {#each paginatedProducts as product}
                <div
                    class="flex flex-col justify-between bg-white rounded-xl shadow-md overflow-hidden transition-transform relative"
                >
                    <div>
                        <div class="relative">
                            <img
                                src={product.image}
                                alt={product.name}
                                class="w-full cover h-52 hover:scale-110 transition-transform object-top object-cover hover:object-scale-down"
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

        <!-- Pagination controls -->
        <div class="my-8 flex justify-center items-center space-x-2">
            <button
                on:click={prevPage}
                disabled={currentPage === 1}
                class="flex items-center px-3 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 disabled:opacity-50 disabled:pointer-events-none transition-all"
            >
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M15 19l-7-7 7-7"
                    />
                </svg>
            </button>

            <span class="text-lg font-medium px-4">
                Pagina {currentPage} van {totalPages}
            </span>

            <button
                on:click={nextPage}
                disabled={currentPage === totalPages}
                class="flex items-center px-3 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 disabled:opacity-50 disabled:pointer-events-none transition-all"
            >
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M9 5l7 7-7 7"
                    />
                </svg>
            </button>
        </div>
    </section>
</main>
