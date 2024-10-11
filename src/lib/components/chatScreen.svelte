<script>
  import Button from "./ui/button/button.svelte";
  import { Input } from "./ui/input";

  export let className;
  let messages = [
    { id: 1, user: "Alice", text: "Hey there!", sentAt: "10:00 AM" },
    { id: 2, user: "You", text: "Hi! What's up?", sentAt: "10:01 AM" },
  ];
  let newMessage = "";

  const sendMessage = () => {
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
      newMessage = "";
    }
  };
</script>

<div
  class="flex flex-col bg-card border-border border rounded-lg ${className} h-full"
>
  <!-- Chat Messages -->
  <div class="flex-1 p-4 overflow-y-auto space-y-4">
    {#each messages as message (message.id)}
      <div
        class="flex {message.user === 'You' ? 'justify-end' : 'justify-start'}"
      >
        <div
          class="max-w-xs px-3 py-2 rounded-lg shadow {message.user === 'You'
            ? 'bg-primary'
            : 'bg-secondary'}"
        >
          <p class="text-sm">{message.text}</p>
          <small class="block text-gray-400 text-right text-xs"
            >{message.sentAt}</small
          >
        </div>
      </div>
    {/each}
  </div>

  <!-- Chat Input -->
  <form class="p-4 flex items-center space-x-4" on:submit={sendMessage}>
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
