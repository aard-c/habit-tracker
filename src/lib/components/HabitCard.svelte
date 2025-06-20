<!-- src/lib/components/HabitCard.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';
    import { Check, Flame, Trash2 } from 'lucide-svelte';
    
    export let habit;
    export let selectedDate;
    
    const dispatch = createEventDispatcher();
    
    $: isCompleted = habit.completions.includes(selectedDate);
    $: currentStreak = calculateStreak(habit.completions);
    $: totalCompletions = habit.completions.length;
    
    function calculateStreak(completions) {
      if (completions.length === 0) return 0;
      
      const sortedDates = [...completions].sort().reverse();
      const today = new Date();
      let streak = 0;
      
      for (let i = 0; i < sortedDates.length; i++) {
        const date = new Date(sortedDates[i]);
        const expectedDate = new Date(today);
        expectedDate.setDate(today.getDate() - i);
        
        if (date.toDateString() === expectedDate.toDateString()) {
          streak++;
        } else {
          break;
        }
      }
      
      return streak;
    }
    
    function handleToggle() {
      dispatch('toggle');
    }
    
    function handleDelete() {
      if (confirm(`Are you sure you want to delete "${habit.name}"?`)) {
        dispatch('delete');
      }
    }
  </script>
  
  <div class="habit-card border-l-4 border-l-blue-500 bg-gray-50 dark:bg-gray-700 p-4 rounded-lg transition-all hover:shadow-md">
    <div class="flex items-center justify-between">
      <div class="flex items-center gap-3 flex-1">
        <!-- Completion Toggle -->
        <button
          on:click={handleToggle}
          class="completion-btn {isCompleted ? 'completed' : 'incomplete'}"
          aria-label="Toggle habit completion"
        >
          {#if isCompleted}
            <Check size={16} />
          {/if}
        </button>
        
        <!-- Habit Info -->
        <div class="flex-1">
          <h3 class="font-medium text-gray-800 dark:text-white {isCompleted ? 'line-through text-gray-500' : ''}">
            {habit.name}
          </h3>
          <div class="flex items-center gap-4 text-sm text-gray-500 dark:text-gray-400 mt-1">
            <span class="flex items-center gap-1">
              <Flame size={14} class="text-orange-500" />
              {currentStreak} day streak
            </span>
            <span>
              {totalCompletions} total completions
            </span>
            <span class="category-badge">
              {habit.category}
            </span>
          </div>
        </div>
      </div>
      
      <!-- Actions -->
      <div class="flex items-center gap-2">
        <button
          on:click={handleDelete}
          class="delete-btn"
          aria-label="Delete habit"
        >
          <Trash2 size={16} />
        </button>
      </div>
    </div>
  </div>
  
  <style>
    .completion-btn {
      @apply w-8 h-8 rounded-full border-2 flex items-center justify-center transition-all cursor-pointer;
    }
    
    .completion-btn.completed {
      @apply bg-green-500 border-green-500 text-white;
    }
    
    .completion-btn.incomplete {
      @apply border-gray-300 hover:border-green-400 hover:bg-green-50 dark:border-gray-600 dark:hover:border-green-400 dark:hover:bg-green-900;
    }
    
    .delete-btn {
      @apply p-2 text-gray-400 hover:text-red-500 hover:bg-red-50 dark:hover:bg-red-900 rounded-md transition-colors;
    }
    
    .category-badge {
      @apply px-2 py-1 bg-blue-100 dark:bg-blue-900 text-blue-700 dark:text-blue-300 rounded-full text-xs;
    }
    
    .habit-card {
      border-left-color: var(--habit-color, #3b82f6);
    }
  </style>