<script>
  import Button from "./ui/button/button.svelte";
  import { Input } from "./ui/input";
  import { onMount } from "svelte";
  import { marked } from "marked";

  export let className;

  let messages = [];
  onMount(async () => {
    const response = await fetch("http://localhost:5000/start_chat");
    const data = await response.json();
    messages = [
      {
        id: Date.now(),
        user: "Bot",
        text: data.message,
        sentAt: new Date().toLocaleTimeString(),
      },
    ];
    console.log(messages);
  });

  let newMessage = "";

  const sendMessage = async () => {
    // Add the new userMessage to the chat
    if (newMessage.trim()) {
      messages = [
        ...messages,
        {
          id: Date.now(),
          user: "You",
          text: newMessage,
          sentAt: new Date().toLocaleTimeString(),
        },
      ];

      // Send the message to the backend
      try {
        const response = await fetch("http://localhost:5000/send_message", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ message: newMessage }),
        });

        const data = await response.json();

        // Add the bot's response to the chat
        console.log(marked(data.reply));
        messages = [
          ...messages,
          {
            id: Date.now(),
            user: "Bot",
            text: marked(data.reply),
            sentAt: new Date().toLocaleTimeString(),
          },
        ];
      } catch (error) {
        console.error("Error sending message:", error);
      }

      newMessage = "";
    }
  };
</script>

<div
  class="flex flex-col bg-card border-border border rounded-lg ${className} h-[80%] min-w-[480px]"
>
  <!-- Chat Messages -->
  <div
    class="flex-1 p-4 overflow-y-auto space-y-4"
    style="scrollbar-width: none;"
  >
    {#each messages as message (message.id)}
      <div
        class="flex {message.user === 'You' ? 'justify-end' : 'justify-start'}"
      >
        <div
          class="max-w-xs px-3 py-2 rounded-lg shadow {message.user === 'You'
            ? 'bg-primary'
            : 'bg-secondary'}"
        >
          <p
            class="text-sm prose prose-invert"
            contenteditable="true"
            bind:innerHTML={message.text}
          ></p>
          <small class="block text-gray-400 text-right text-xs"
            >{message.sentAt}</small
          >
        </div>
      </div>
    {/each}
  </div>

  <!-- Chat Input -->
  <form
    class="p-4 flex items-center space-x-4"
    on:submit|preventDefault={sendMessage}
  >
    <Input
      type="text"
      class="flex-1 p-2 border border-border bg-background rounded-lg shadow focus:outline-none focus:ring focus:ring-ring"
      bind:value={newMessage}
      placeholder="Type your message..."
    />
    <Button
      type="submit"
      class="bg-primary text-primary-foreground px-4 py-2 rounded-lg shadow"
    >
      Send
    </Button>
  </form>
</div>
