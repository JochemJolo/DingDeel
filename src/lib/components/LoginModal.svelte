<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
  
    function closeModal() {
      dispatch('close');
    }
  </script>
<div class="z-20 fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
    <div class="bg-white p-8 rounded-lg shadow-lg w-96">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-bold text-gray-800">Inloggen</h2>
            <button on:click={closeModal} class="text-gray-500 hover:text-gray-700 text-xl">&times;</button>
        </div>
        <form class="space-y-6">
            <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">E-mailadres</label>
                <input
                    type="email"
                    class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
                    placeholder="Voer uw e-mailadres in"
                />
            </div>
            <div>
                <label class="block text-sm font-medium text-gray-700 mb-1">Wachtwoord</label>
                <input
                    type="password"
                    class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
                    placeholder="Voer uw wachtwoord in"
                />
            </div>
            <div class="flex items-center justify-between text-sm">
                <a href="#" class="text-blue-600 hover:underline">Wachtwoord vergeten?</a>
            </div>
            <button
                type="submit"
                class="w-full py-2 px-4 text-white bg-blue-600 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition duration-200"
            >
                Inloggen
            </button>
        </form>
        <div class="mt-6 text-center">
            <p class="text-sm text-gray-600">
                Nog geen account? 
                <a href="#" class="text-blue-600 hover:underline">Maak er een aan</a>
            </p>
        </div>
    </div>
</div>
