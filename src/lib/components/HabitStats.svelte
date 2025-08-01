<script>
  export let totalHabits = 0;
  export let todayCompletions = 0;
  export let completionRate = 0;
</script>

<div class="grid gap-4 sm:grid-cols-3 text-center">
  <!-- Total Habits -->
  <div class="stat-card">
    <p class="stat-title">Total Habits</p>
    <p class="stat-value">{totalHabits}</p>
  </div>

  <!-- Completions Today -->
  <div class="stat-card">
    <p class="stat-title">Today's Completions</p>
    <p class="stat-value">{todayCompletions}</p>
  </div>

  <!-- Completion Rate -->
  <div class="stat-card">
    <p class="stat-title">Completion Rate</p>
    <p class="stat-value">{completionRate}%</p>

    <!-- Improved Progress Bar -->
    <div class="progress-container">
      <div
        class="progress-bar"
        style="width: {completionRate}%"
      ></div>
    </div>
  </div>
</div>

<style>
  .stat-card {
    background-color: var(--card-bg, #e0f2fe);
    padding: 1rem;
    border-radius: 1rem;
    box-shadow: 0 2px 6px rgba(0,0,0,0.04);
  }

  .stat-title {
    font-size: 0.875rem;
    color: #6b7280;
    margin-bottom: 0.25rem;
  }

  .stat-value {
    font-size: 1.5rem;
    font-weight: bold;
    color: #111827;
  }

  :global(.dark) .stat-card {
    background-color: #1f2937;
  }

  .progress-container {
    margin-top: 0.5rem;
    width: 100%;
    height: 12px;
    background-color: #e0e7ff; /* pastel base */
    border-radius: 9999px;
    overflow: hidden;
  }

  .progress-bar {
    height: 100%;
    background-color: #93c5fd; /* pastel blue */
    border-radius: 9999px 0 0 9999px;
    transition: width 0.3s ease;
  }

  :global(.dark) .progress-container {
    background-color: #374151;
  }

  :global(.dark) .progress-bar {
    background-color: #60a5fa;
  }
</style>
