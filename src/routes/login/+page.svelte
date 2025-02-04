<script>
    import { onMount } from "svelte";
    let isOpen = false;
  </script>
  
  <div class="flex items-center justify-center min-h-screen bg-gray-100">
    <button
      class="px-4 py-2 text-white bg-blue-500 rounded-lg"
      on:click={() => isOpen = true}
    >
      Open Login Modal
    </button>
  
    {#if isOpen}
      <div class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
        <div class="bg-white p-6 rounded-lg shadow-lg w-96">
          <div class="flex justify-between items-center mb-4">
            <h2 class="text-xl font-semibold">Login</h2>
            <button on:click={() => isOpen = false} class="text-gray-500">&times;</button>
          </div>
          <form class="space-y-4">
            <div>
              <label class="block text-sm font-medium text-gray-700">Email</label>
              <input
                type="email"
                class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Enter your email"
              />
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700">Password</label>
              <input
                type="password"
                class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                placeholder="Enter your password"
              />
            </div>
            <button
              type="submit"
              class="w-full py-2 text-white bg-blue-500 rounded-lg hover:bg-blue-600"
            >
              Login
            </button>
          </form>
        </div>
      </div>
    {/if}
  </div>
  