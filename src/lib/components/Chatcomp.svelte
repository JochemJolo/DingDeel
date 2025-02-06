<script>
    import { createEventDispatcher } from "svelte";

    let messages = [];
    let newMessage = "";
    const dispatch = createEventDispatcher();
    let isBotTyping = false;
    let chatContainer;

    const botResponses = [
        "Vertel me daar eens meer over.",
        "Hoe voelt dat voor jou?",
        "Dat klinkt boeiend! Wat denk je daar verder over?",
        "Bedankt dat je dit deelt. Kun je dat wat meer uitleggen?",
        "Wat wil je hierna bespreken?",
        "Dat is een goed punt. Heb je andere opties overwogen?",
        "Interessante gedachte. Hoe kwam je daarbij?",
        "Ik hoor je. Wat houdt je nog meer bezig?",
        "Dat zet aan tot nadenken. Hoe past dat in jouw ervaring?",
        "Ik ben benieuwd naar wat je verder denkt.",
        "Wat inspireert je het meest aan dit onderwerp?",
        "Welke uitdagingen zie je hier nog?",
        "Hoe zou je dit aanpakken als alles mogelijk was?",
        "Wat zou je nog meer willen leren hierover?",
        "Wat zou je doen als je geen beperkingen had?",
        "Dat klinkt als een interessant pad! Waarom spreekt dat je aan?",
        "Zijn er specifieke voorbeelden die je hierbij helpen?",
        "Hoe zou je dit aan iemand uitleggen die er niets van weet?",
        "Hoe kijk je er nu anders naar dan eerst?",
        "Wat motiveert je om hierover na te denken?",
    ];
    function closeModal() {
        dispatch("close");
    }

    function sendMessage() {
        if (newMessage.trim()) {
            messages = [...messages, { text: newMessage, sender: "user" }];
            newMessage = "";

            // Show typing animation
            isBotTyping = true;

            // Scroll to the latest message
            scrollToBottom();

            // Simulate a delay before the bot responds
            setTimeout(() => {
                const randomResponse =
                    botResponses[
                        Math.floor(Math.random() * botResponses.length)
                    ];
                messages = [
                    ...messages,
                    { text: randomResponse, sender: "bot" },
                ];
                isBotTyping = false;

                // Scroll to the latest message
                scrollToBottom();
            }, 500);
        }
    }

    function scrollToBottom() {
        setTimeout(() => {
            if (chatContainer) {
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }
        }, 0);
    }
</script>

<div
    class="fixed bottom-4 z-20 right-4 w-96 h-[65%] bg-white rounded-2xl shadow-2xl flex flex-col overflow-hidden"
>
    <!-- Header with person details -->
    <div
        class="bg-green-500 text-white p-4 flex justify-between items-center rounded-t-2xl"
    >
        <div class="flex items-center space-x-3">
            <img
                src="/products/profilepicture.png"
                alt="Profile picture"
                class="w-20 h-20 rounded-full object-cover"
            />
            <div>
                <h3 class="font-semibold">Alex Jones</h3>
                <div class="flex items-center text-yellow-400">
                    <svg
                        class="w-4 h-4"
                        fill="currentColor"
                        viewBox="0 0 24 24"
                    >
                        <path
                            d="M12 .587l3.668 7.425 8.167 1.188-5.917 5.77 1.396 8.13L12 18.897l-7.314 3.85 1.396-8.13L.165 9.2l8.167-1.188z"
                        />
                    </svg>
                    <svg
                        class="w-4 h-4"
                        fill="currentColor"
                        viewBox="0 0 24 24"
                    >
                        <path
                            d="M12 .587l3.668 7.425 8.167 1.188-5.917 5.77 1.396 8.13L12 18.897l-7.314 3.85 1.396-8.13L.165 9.2l8.167-1.188z"
                        />
                    </svg>
                    <svg
                        class="w-4 h-4"
                        fill="currentColor"
                        viewBox="0 0 24 24"
                    >
                        <path
                            d="M12 .587l3.668 7.425 8.167 1.188-5.917 5.77 1.396 8.13L12 18.897l-7.314 3.85 1.396-8.13L.165 9.2l8.167-1.188z"
                        />
                    </svg>
                    <svg
                        class="w-4 h-4"
                        fill="currentColor"
                        viewBox="0 0 24 24"
                    >
                        <path
                            d="M12 .587l3.668 7.425 8.167 1.188-5.917 5.77 1.396 8.13L12 18.897l-7.314 3.85 1.396-8.13L.165 9.2l8.167-1.188z"
                        />
                    </svg>
                    <svg
                        class="w-4 h-4 text-gray-300"
                        fill="currentColor"
                        viewBox="0 0 24 24"
                    >
                        <path
                            d="M12 .587l3.668 7.425 8.167 1.188-5.917 5.77 1.396 8.13L12 18.897l-7.314 3.85 1.396-8.13L.165 9.2l8.167-1.188z"
                        />
                    </svg>
                    <span class="ml-2 text-sm text-gray-200">(4.8)</span>
                </div>
            </div>
        </div>
        <button on:click={closeModal} class="text-white hover:text-gray-200">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
            >
                <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M6 18L18 6M6 6l12 12"
                />
            </svg>
        </button>
    </div>

    <!-- Messages section -->
    <div class="flex-1 p-4 overflow-y-auto space-y-3 bg-gray-50">
        {#each messages as message}
            <div class={message.sender === "user" ? "text-right" : "text-left"}>
                <span
                    class="inline-block p-3 rounded-xl max-w-[75%] {message.sender ===
                    'user'
                        ? 'bg-green-100'
                        : 'bg-white border'} shadow"
                >
                    {message.text}
                </span>
            </div>
        {/each}

        {#if isBotTyping}
            <div class="flex items-center space-x-2 mb-2">
                <div
                    class="w-2 h-2 bg-gray-500 rounded-full animate-bounce"
                ></div>
                <div
                    class="w-2 h-2 bg-gray-500 rounded-full animate-bounce"
                    style="animation-delay: 0.2s"
                ></div>
                <div
                    class="w-2 h-2 bg-gray-500 rounded-full animate-bounce"
                    style="animation-delay: 0.4s"
                ></div>
            </div>
        {/if}
    </div>

    <!-- Input Section -->
    <div class="p-4 border-t bg-white">
        <form on:submit|preventDefault={sendMessage} class="flex">
            <input
                bind:value={newMessage}
                type="text"
                placeholder="Type een bericht..."
                class="flex-1 p-3 border rounded-l-xl focus:outline-none focus:ring-2 focus:ring-green-500"
            />
            <button
                type="submit"
                class="bg-green-500 text-white p-3 rounded-r-xl hover:bg-green-600 transition-colors"
            >
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"
                    />
                </svg>
            </button>
        </form>
    </div>
</div>
