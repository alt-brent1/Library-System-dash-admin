<script lang="ts">
  import { onMount } from "svelte";
  import { page } from "$app/stores";
  import { goto } from "$app/navigation";
  import { writable } from "svelte/store";

  export let onLogout: () => void = () => {};
  
  const sidebarOpen = writable(false);

  const navigation = [
    { name: "Dashboard", href: "/dashboard", icon: "📊" },
    { name: "Books", href: "/books", icon: "📚" },
    { name: "Members", href: "/members", icon: "👥" },
    { name: "Transactions", href: "/transactions", icon: "🔄" },
    { name: "Reports", href: "/reports", icon: "📈" },
    { name: "Settings", href: "/settings", icon: "⚙️" },
  ];

  let currentPath = "";
  $: currentPath = $page.url.pathname;
</script>

<div class="flex h-screen bg-muted">
  <!-- Sidebar -->
  <div class="fixed inset-y-0 left-0 z-50 w-64 bg-card shadow-lg transform transition-transform duration-300 ease-in-out lg:translate-x-0 lg:static lg:inset-0"
    class:translate-x-0={$sidebarOpen}
    class:-translate-x-full={!$sidebarOpen}
  >
    <!-- Logo -->
    <div class="flex items-center justify-between h-16 px-6 bg-primary text-primary-foreground">
      <div class="flex items-center space-x-3">
        <span class="text-2xl">📚</span>
        <div>
          <h1 class="text-lg font-bold">Kalibro</h1>
          <p class="text-xs text-green-200">Library System</p>
        </div>
      </div>
      <button class="lg:hidden" on:click={() => sidebarOpen.set(false)}>
        <span class="text-xl">✖️</span>
      </button>
    </div>

    <!-- Navigation -->
    <nav class="mt-6 px-4">
      <ul class="space-y-2">
        {#each navigation as item}
          <li>
            <a
              href={item.href}
              class="flex items-center space-x-3 px-4 py-3 text-sm font-medium rounded-lg transition-colors duration-150
                {currentPath === item.href
                  ? 'bg-primary/10 text-primary border-r-2 border-primary'
                  : 'text-muted-foreground hover:bg-muted hover:text-card-foreground'}"
              on:click|preventDefault={() => { goto(item.href); sidebarOpen.set(false); }}
            >
              <span class="text-xl">{item.icon}</span>
              <span>{item.name}</span>
            </a>
          </li>
        {/each}
      </ul>
    </nav>

    <!-- User info -->
    <div class="absolute bottom-0 left-0 right-0 p-4 border-t">
      <div class="flex items-center space-x-3 mb-3">
        <div class="w-8 h-8 bg-muted rounded-full flex items-center justify-center">
          <span class="text-lg text-muted-foreground">👤</span>
        </div>
        <div>
          <p class="text-sm font-medium text-card-foreground">Admin User</p>
          <p class="text-xs text-muted-foreground">admin@kalibro.com</p>
        </div>
      </div>
      <button
        on:click={onLogout}
        class="flex items-center space-x-2 w-full px-3 py-2 text-sm text-muted-foreground hover:bg-muted rounded-lg transition-colors duration-150"
      >
        <span class="text-lg">🚪</span>
        <span>Sign out</span>
      </button>
    </div>
  </div>

  <!-- Main content -->
  <div class="flex-1 flex flex-col overflow-hidden">
    <!-- Header -->
    <header class="bg-card shadow-sm border-b">
      <div class="flex items-center justify-between px-6 py-4">
        <div class="flex items-center space-x-4">
          <button class="lg:hidden" on:click={() => sidebarOpen.set(true)}>
            <span class="text-xl text-muted-foreground">☰</span>
          </button>
          <h1 class="text-2xl font-semibold text-card-foreground">
            {navigation.find(nav => nav.href === currentPath)?.name || "Dashboard"}
          </h1>
        </div>
        <div class="flex items-center space-x-4">
          <button class="relative p-2 text-muted-foreground hover:text-card-foreground">
            <span class="text-xl">🔔</span>
            <span class="absolute top-1 right-1 h-3 w-3 bg-green-500 rounded-full"></span>
          </button>
        </div>
      </div>
    </header>

    <!-- Page content -->
    <main class="flex-1 overflow-x-hidden overflow-y-auto bg-muted p-6">
      <slot />
    </main>
  </div>

  <!-- Overlay -->
  {#if $sidebarOpen}
    <div
      class="fixed inset-0 z-40 bg-black bg-opacity-25 lg:hidden"
      on:click={() => sidebarOpen.set(false)}
    ></div>
  {/if}
</div>

<style>
  :root {
    --card-bg: #f7fafc;
    --card-foreground: #1a2e22;
    --card-border: #d1fae5;
    --primary-bg: #059669;
    --primary-text: #059669;
    --primary-foreground: #fff;
    --primary-border: #059669;
    --muted-bg: #e6f4ea;
    --muted-foreground: #4b5563;
    --green-200: #bbf7d0;
  }

  :global(.bg-card) { background-color: var(--card-bg); }
  :global(.text-card-foreground) { color: var(--card-foreground); }
  :global(.border-card) { border-color: var(--card-border); }

  :global(.bg-primary) { background-color: var(--primary-bg); }
  :global(.text-primary) { color: var(--primary-text); }
  :global(.text-primary-foreground) { color: var(--primary-foreground); }
  :global(.border-primary) { border-color: var(--primary-border); }

  :global(.bg-muted) { background-color: var(--muted-bg); }
  :global(.text-muted-foreground) { color: var(--muted-foreground); }

  /* Custom scrollbar styles */
  :global(body) {
    scrollbar-width: thin;
    scrollbar-color: var(--primary-bg) transparent;
  }

  :global(body::-webkit-scrollbar) {
    width: 8px;
  }

  :global(body::-webkit-scrollbar-thumb) {
    background-color: var(--primary-bg);
    border-radius: 4px;
  }

  :global(body::-webkit-scrollbar-track) {
    background-color: transparent;
  }

  /* Modern greenish accent for sidebar active state */
  .bg-primary\/10 {
    background-color: rgba(5, 150, 105, 0.10);
  }
  .border-primary {
    border-color: #059669 !important;
  }
  .text-primary {
    color: #059669 !important;
  }
  .bg-green-500 {
    background-color: #22c55e !important;
  }
  .text-green-200 {
    color: #bbf7d0 !important;
  }
</style>