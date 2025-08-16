<script lang="ts">
  import Layout from "$lib/components/ui/layout.svelte";

  // Icon SVGs (replace Lucide icons with SVGs or use a Svelte icon library)
  // For brevity, using emoji placeholders
  const icons = {
    book: "📚",
    users: "👥",
    arrow: "🔄",
    trending: "📈",
    calendar: "📅",
    clock: "⏰",
    alert: "⚠️"
  };

  const recentActivity = [
    { id: 1, action: "Book borrowed", book: "Introduction to Computer Science", member: "John Doe", time: "2 hours ago" },
    { id: 2, action: "Book returned", book: "Calculus and Analytic Geometry", member: "Jane Smith", time: "3 hours ago" },
    { id: 3, action: "New member registered", book: "", member: "Michael Johnson", time: "5 hours ago" },
    { id: 4, action: "Book reserved", book: "Physics Principles", member: "Sarah Wilson", time: "1 day ago" },
  ];

  const overdueBooks = [
    { id: 1, book: "Advanced Mathematics", member: "Tom Brown", dueDate: "2024-01-10", daysOverdue: 5 },
    { id: 2, book: "Chemistry Fundamentals", member: "Lisa Garcia", dueDate: "2024-01-12", daysOverdue: 3 },
    { id: 3, book: "World History", member: "David Lee", dueDate: "2024-01-13", daysOverdue: 2 },
  ];
</script>

<Layout>
  <div class="space-y-6">
    <!-- Stats Cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <div class="bg-card text-card-foreground p-6 rounded-xl shadow border flex items-center justify-between">
        <div>
          <p class="text-sm font-medium text-muted-foreground">Total Books</p>
          <p class="text-3xl font-bold">12,847</p>
          <p class="text-sm text-green-600 mt-1">+125 this month</p>
        </div>
        <div class="p-3 bg-blue-100 rounded-full text-2xl">{icons.book}</div>
      </div>
      <div class="bg-card text-card-foreground p-6 rounded-xl shadow border flex items-center justify-between">
        <div>
          <p class="text-sm font-medium text-muted-foreground">Active Members</p>
          <p class="text-3xl font-bold">2,456</p>
          <p class="text-sm text-green-600 mt-1">+48 this month</p>
        </div>
        <div class="p-3 bg-green-100 rounded-full text-2xl">{icons.users}</div>
      </div>
      <div class="bg-card text-card-foreground p-6 rounded-xl shadow border flex items-center justify-between">
        <div>
          <p class="text-sm font-medium text-muted-foreground">Books Borrowed</p>
          <p class="text-3xl font-bold">1,247</p>
          <p class="text-sm text-blue-600 mt-1">This month</p>
        </div>
        <div class="p-3 bg-yellow-100 rounded-full text-2xl">{icons.arrow}</div>
      </div>
      <div class="bg-card text-card-foreground p-6 rounded-xl shadow border flex items-center justify-between">
        <div>
          <p class="text-sm font-medium text-muted-foreground">Overdue Books</p>
          <p class="text-3xl font-bold text-red-600">23</p>
          <p class="text-sm text-red-600 mt-1">Needs attention</p>
        </div>
        <div class="p-3 bg-red-100 rounded-full text-2xl">{icons.alert}</div>
      </div>
    </div>

    <!-- Activity and Overdue Books -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <div class="bg-card text-card-foreground rounded-xl shadow border">
        <div class="p-6 border-b flex items-center justify-between">
          <h3 class="text-lg font-semibold">Recent Activity</h3>
          <span class="text-muted-foreground text-xl">{icons.clock}</span>
        </div>
        <div class="p-0">
          <div>
            {#each recentActivity as activity}
              <div class="p-4 border-b last:border-b-0 hover:bg-muted transition-colors duration-150">
                <div class="flex items-start space-x-3">
                  <div class="w-2 h-2 bg-blue-500 rounded-full mt-2"></div>
                  <div class="flex-1">
                    <p class="text-sm font-medium">{activity.action}</p>
                    {#if activity.book}
                      <p class="text-sm text-muted-foreground">{activity.book}</p>
                    {/if}
                    <div class="flex items-center justify-between mt-1">
                      <span class="text-sm text-blue-600">{activity.member}</span>
                      <span class="text-xs text-muted-foreground">{activity.time}</span>
                    </div>
                  </div>
                </div>
              </div>
            {/each}
          </div>
        </div>
      </div>
      <div class="bg-card text-card-foreground rounded-xl shadow border">
        <div class="p-6 border-b flex items-center justify-between">
          <h3 class="text-lg font-semibold">Overdue Books</h3>
          <span class="text-red-500 text-xl">{icons.alert}</span>
        </div>
        <div class="p-0">
          <div>
            {#each overdueBooks as book}
              <div class="p-4 border-b last:border-b-0 hover:bg-muted transition-colors duration-150">
                <div class="flex items-start space-x-3">
                  <div class="w-2 h-2 bg-red-500 rounded-full mt-2"></div>
                  <div class="flex-1">
                    <p class="text-sm font-medium">Overdue: {book.book}</p>
                    <p class="text-sm text-muted-foreground">{book.member}</p>
                    <div class="flex items-center justify-between mt-1">
                      <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-red-100 text-red-800">
                        {book.daysOverdue} days overdue
                      </span>
                      <span class="text-xs text-muted-foreground">{book.dueDate}</span>
                    </div>
                  </div>
                </div>
              </div>
            {/each}
          </div>
        </div>
      </div>
    </div>
  </div>
</Layout>

<style>
  /* Shadcn-inspired colors and rounded corners */
  .bg-card { background-color: var(--card-bg, #fff); }
  .text-card-foreground { color: var(--card-fg, #1a1a1a); }
  .text-muted-foreground { color: var(--muted-fg, #6b7280); }
  .bg-blue-100 { background-color: #dbeafe; }
  .bg-green-100 { background-color: #d1fae5; }
  .bg-yellow-100 { background-color: #fef3c7; }
  .bg-red-100 { background-color: #fee2e2; }
  .bg-muted { background-color: var(--muted-bg, #f3f4f6); }
  .rounded-xl { border-radius: 1rem; }
  .shadow { box-shadow: 0 2px 8px rgba(0,0,0,0.06); }
  .border {
    border-width: 1px;
    border-color: #e5e7eb;
  }
</style>