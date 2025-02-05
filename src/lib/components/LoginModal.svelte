<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
  
    function closeModal() {
      dispatch('close');
    }
  </script>
<div class="z-20 fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 ">
    <div class="bg-white p-6 rounded-lg shadow-lg w-96">
        <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl font-semibold">Login</h2>
        <button on:click={closeModal} class="text-gray-500">&times;</button>
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
