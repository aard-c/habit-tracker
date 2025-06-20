<!-- src/lib/components/HabitStats.svelte -->
<script>
    import { Target, CheckCircle, TrendingUp } from 'lucide-svelte';
    
    export let totalHabits;
    export let todayCompletions;
    export let completionRate;
  </script>
  
  <div class="stats-grid">
    <!-- Total Habits -->
    <div class="stat-card">
      <div class="stat-icon bg-blue-100 dark:bg-blue-900 text-blue-600 dark:text-blue-400">
        <Target size={20} />
      </div>
      <div>
        <p class="stat-value">{totalHabits}</p>
        <p class="stat-label">Total Habits</p>
      </div>
    </div>
    
    <!-- Today's Completions -->
    <div class="stat-card">
      <div class="stat-icon bg-green-100 dark:bg-green-900 text-green-600 dark:text-green-400">
        <CheckCircle size={20} />
      </div>
      <div>
        <p class="stat-value">{todayCompletions}</p>
        <p class="stat-label">Completed Today</p>
      </div>
    </div>
    
    <!-- Completion Rate -->
    <div class="stat-card">
      <div class="stat-icon bg-purple-100 dark:bg-purple-900 text-purple-600 dark:text-purple-400">
        <TrendingUp size={20} />
      </div>
      <div>
        <p class="stat-value">{completionRate}%</p>
        <p class="stat-label">Success Rate</p>
      </div>
    </div>
  </div>
  
  <!-- Progress Bar -->
  {#if totalHabits > 0}
    <div class="mt-4">
      <div class="flex justify-between text-sm text-gray-600 dark:text-gray-400 mb-2">
        <span>Daily Progress</span>
        <span>{todayCompletions}/{totalHabits}</span>
      </div>
      <div class="progress-bar">
        <div 
          class="progress-fill" 
          style="width: {completionRate}%"
        ></div>
      </div>
    </div>
  {/if}
  
  <style>
    .stats-grid {
      @apply grid grid-cols-1 sm:grid-cols-3 gap-4;
    }
    
    .stat-card {
      @apply flex items-center gap-3 p-3 bg-gray-50 dark:bg-gray-700 rounded-lg;
    }
    
    .stat-icon {
      @apply p-2 rounded-lg flex-shrink-0;
    }
    
    .stat-value {
      @apply text-xl font-bold text-gray-800 dark:text-white;
    }
    
    .stat-label {
      @apply text-sm text-gray-600 dark:text-gray-400;
    }
    
    .progress-bar {
      @apply w-full bg-gray-200 dark:bg-gray-600 rounded-full h-2;
    }
    
    .progress-fill {
      @apply bg-gradient-to-r from-blue-500 to-green-500 h-2 rounded-full transition-all duration-300 ease-in-out;
    }
  </style>