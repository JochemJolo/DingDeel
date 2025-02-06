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
                                class="w-full cover h-52 transition-transform object-top object-cover hover:object-scale-down"
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
                                    class="flex items-center justify-center gap-2 text-white font-semibold w-full py-3 px-4 transition-all duration-300 hover:scale-105 active:scale-95 rounded-full bg-purple-500 hover:bg-purple-600 shadow-md"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="h-5 w-5"
                                        viewBox="0 0 20 20"
                                        fill="currentColor"
                                    >
                                        <path
                                            d="M10 12a2 2 0 100-4 2 2 0 000 4z"
                                        />
                                        <path
                                            fill-rule="evenodd"
                                            d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zm-2 6a3 3 0 116 0 3 3 0 01-6 0z"
                                            clip-rule="evenodd"
                                        />
                                    </svg>
                                    Bieden
                                </button>
                                <button
                                    class="flex items-center justify-center gap-2 text-white font-semibold w-full py-3 px-4 transition-all duration-300 hover:scale-105 active:scale-95 rounded-full bg-green-500 hover:bg-green-600"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="h-5 w-5"
                                        viewBox="0 0 20 20"
                                        fill="currentColor"
                                    >
                                        <path
                                            fill-rule="evenodd"
                                            d="M18 10c0 3.866-3.582 7-8 7a8.841 8.841 0 01-4.083-.98L2 17l1.338-3.123C2.493 12.767 2 11.434 2 10c0-3.866 3.582-7 8-7s8 3.134 8 7zM7 9H5v2h2V9zm8 0h-2v2h2V9zM9 9h2v2H9V9z"
                                            clip-rule="evenodd"
                                        />
                                    </svg>
                                    Stuur Bericht
                                </button>
                            {:else if product.type === "Ruilen"}
                                <button
                                    class="flex items-center justify-center gap-2 text-white font-semibold w-full py-3 px-4 transition-all duration-300 hover:scale-105 active:scale-95 rounded-full bg-blue-500 hover:bg-blue-600 shadow-md"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="h-5 w-5"
                                        viewBox="0 0 20 20"
                                        fill="currentColor"
                                    >
                                        <path
                                            d="M8 5a1 1 0 100 2h5.586l-1.293 1.293a1 1 0 001.414 1.414l3-3a1 1 0 000-1.414l-3-3a1 1 0 10-1.414 1.414L13.586 5H8zM12 15a1 1 0 100-2H6.414l1.293-1.293a1 1 0 10-1.414-1.414l-3 3a1 1 0 000 1.414l3 3a1 1 0 001.414-1.414L6.414 15H12z"
                                        />
                                    </svg>
                                    Ruil voorstel
                                </button>
                                <button
                                    class="flex items-center justify-center gap-2 text-white font-semibold w-full py-3 px-4 transition-all duration-300 hover:scale-105 active:scale-95 rounded-full bg-green-500 hover:bg-green-600"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="h-5 w-5"
                                        viewBox="0 0 20 20"
                                        fill="currentColor"
                                    >
                                        <path
                                            fill-rule="evenodd"
                                            d="M18 10c0 3.866-3.582 7-8 7a8.841 8.841 0 01-4.083-.98L2 17l1.338-3.123C2.493 12.767 2 11.434 2 10c0-3.866 3.582-7 8-7s8 3.134 8 7zM7 9H5v2h2V9zm8 0h-2v2h2V9zM9 9h2v2H9V9z"
                                            clip-rule="evenodd"
                                        />
                                    </svg>
                                    Stuur Bericht
                                </button>
                            {:else if product.type === "Lenen"}
                                <button
                                    class="flex items-center justify-center gap-2 text-white font-semibold w-full py-3 px-4 transition-all duration-300 hover:scale-105 active:scale-95 rounded-full bg-green-500 hover:bg-green-600"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="h-5 w-5"
                                        viewBox="0 0 20 20"
                                        fill="currentColor"
                                    >
                                        <path
                                            fill-rule="evenodd"
                                            d="M18 10c0 3.866-3.582 7-8 7a8.841 8.841 0 01-4.083-.98L2 17l1.338-3.123C2.493 12.767 2 11.434 2 10c0-3.866 3.582-7 8-7s8 3.134 8 7zM7 9H5v2h2V9zm8 0h-2v2h2V9zM9 9h2v2H9V9z"
                                            clip-rule="evenodd"
                                        />
                                    </svg>
                                    Stuur Bericht
                                </button>
                            {:else}
                                <button
                                    class="flex items-center justify-center gap-2 text-white font-semibold w-full py-3 px-4 transition-all duration-300 hover:scale-105 active:scale-95 rounded-full bg-green-500 hover:bg-green-600"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="h-5 w-5"
                                        viewBox="0 0 20 20"
                                        fill="currentColor"
                                    >
                                        <path
                                            fill-rule="evenodd"
                                            d="M18 10c0 3.866-3.582 7-8 7a8.841 8.841 0 01-4.083-.98L2 17l1.338-3.123C2.493 12.767 2 11.434 2 10c0-3.866 3.582-7 8-7s8 3.134 8 7zM7 9H5v2h2V9zm8 0h-2v2h2V9zM9 9h2v2H9V9z"
                                            clip-rule="evenodd"
                                        />
                                    </svg>
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
