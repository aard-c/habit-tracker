<!-- src/lib/components/HabitCard.svelte -->
<script>
  import { createEventDispatcher } from 'svelte';
  import { Check, Flame, Trash2 } from 'lucide-svelte';
  
  /** @type {any} */
  export let habit;
  /** @type {string} */
  export let selectedDate;
  
  const dispatch = createEventDispatcher();
  
  $: isCompleted = habit.completions.includes(selectedDate);
  $: currentStreak = calculateStreak(habit.completions);
  $: totalCompletions = habit.completions.length;
  
  /**
   * @param {string[]} completions
   * @returns {number}
   */
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
    width: 2rem;
    height: 2rem;
    border-radius: 9999px;
    border-width: 2px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.15s ease-in-out;
    cursor: pointer;
  }
  
  .completion-btn.completed {
    background-color: #10b981;
    border-color: #10b981;
    color: white;
  }
  
  .completion-btn.incomplete {
    border-color: #d1d5db;
    background-color: transparent;
  }
  
  .completion-btn.incomplete:hover {
    border-color: #6ee7b7;
    background-color: #f0fdf4;
  }
  
  :global(.dark) .completion-btn.incomplete {
    border-color: #4b5563;
  }
  
  :global(.dark) .completion-btn.incomplete:hover {
    border-color: #6ee7b7;
    background-color: #064e3b;
  }
  
  .delete-btn {
    padding: 0.5rem;
    color: #9ca3af;
    border-radius: 0.375rem;
    transition: all 0.15s ease-in-out;
  }
  
  .delete-btn:hover {
    color: #ef4444;
    background-color: #fef2f2;
  }
  
  :global(.dark) .delete-btn:hover {
    background-color: #7f1d1d;
  }
  
  .category-badge {
    padding: 0.25rem 0.5rem;
    background-color: #dbeafe;
    color: #1d4ed8;
    border-radius: 9999px;
    font-size: 0.75rem;
  }
  
  :global(.dark) .category-badge {
    background-color: #1e3a8a;
    color: #93c5fd;
  }
  
  .habit-card {
    border-left-width: 4px;
    border-left-color: #3b82f6;
    background-color: #f9fafb;
    padding: 1rem;
    border-radius: 0.5rem;
    transition: all 0.15s ease-in-out;
  }
  
  .habit-card:hover {
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  }
  
  :global(.dark) .habit-card {
    background-color: #374151;
  }
</style>