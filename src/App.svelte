<script>
  import ChatScreen from "$lib/components/chatScreen.svelte";
</script>

<main class="h-screen w-screen text-foreground p-8 flex flex-col items-center">
  <header class="w-full text-center flex-col items-center py-8">
    <h1 class="scroll-m-20 text-4xl font-extrabold tracking-tight lg:text-5xl">
      G-BOT
    </h1>
    <p class="text-muted-foreground text-xl">Chat it out</p>
  </header>

  <ChatScreen className="h-full w-[30%]"></ChatScreen>
</main>
