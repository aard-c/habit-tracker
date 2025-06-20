<script>
    import { onMount } from 'svelte';
    import { Plus, Calendar, TrendingUp } from 'lucide-svelte';
    import HabitCard from '../lib/components/HabitCard.svelte';
    import AddHabitModal from '../lib/components/AddHabitModal.svelte';
    import HabitStats from '../lib/components/HabitStats.svelte';
    
    let habits = [];
    let showAddModal = false;
    let selectedDate = new Date().toISOString().split('T')[0];
  
    // Load habits from localStorage
    onMount(() => {
      const saved = localStorage.getItem('habits');
      if (saved) {
        habits = JSON.parse(saved);
      }
    });
  
    // Save habits to localStorage
    function saveHabits() {
      localStorage.setItem('habits', JSON.stringify(habits));
      habits = habits; // Trigger reactivity
    }
  
    function addHabit(habitData) {
      const newHabit = {
        id: Date.now(),
        ...habitData,
        completions: [],
        createdAt: new Date().toISOString()
      };
      habits = [...habits, newHabit];
      saveHabits();
      showAddModal = false;
    }
  
    function toggleHabit(habitId, date = selectedDate) {
      habits = habits.map(habit => {
        if (habit.id === habitId) {
          const completionIndex = habit.completions.indexOf(date);
          if (completionIndex > -1) {
            habit.completions.splice(completionIndex, 1);
          } else {
            habit.completions.push(date);
          }
        }
        return habit;
      });
      saveHabits();
    }
  
    function deleteHabit(habitId) {
      habits = habits.filter(habit => habit.id !== habitId);
      saveHabits();
    }
  
    // Calculate stats
    $: totalHabits = habits.length;
    $: todayCompletions = habits.filter(habit => 
      habit.completions.includes(selectedDate)
    ).length;
    $: completionRate = totalHabits > 0 ? 
      Math.round((todayCompletions / totalHabits) * 100) : 0;
  </script>
  
  <svelte:head>
    <title>Habit Tracker</title>
  </svelte:head>
  
  <main class="min-h-screen bg-gray-50 dark:bg-gray-900">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
      <!-- Header -->
      <header class="text-center mb-8">
        <h1 class="text-4xl font-bold text-gray-800 dark:text-white mb-2">
          Habit Tracker
        </h1>
        <p class="text-gray-600 dark:text-gray-300">
          Build better habits, one day at a time
        </p>
      </header>
  
      <!-- Date Selector -->
      <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-6">
        <div class="flex items-center justify-between mb-4">
          <div class="flex items-center gap-2">
            <Calendar class="text-blue-500" size={20} />
            <h2 class="text-lg font-semibold text-gray-800 dark:text-white">
              Track Your Day
            </h2>
          </div>
          <input 
            type="date" 
            bind:value={selectedDate}
            class="px-3 py-2 border border-gray-300 rounded-md dark:bg-gray-700 dark:border-gray-600 dark:text-white"
          />
        </div>
        
        <!-- Quick Stats -->
        <HabitStats 
          {totalHabits} 
          {todayCompletions} 
          {completionRate} 
        />
      </div>
  
      <!-- Habits List -->
      <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-6">
        <div class="flex items-center justify-between mb-6">
          <h2 class="text-xl font-semibold text-gray-800 dark:text-white">
            Your Habits
          </h2>
          <button 
            on:click={() => showAddModal = true}
            class="flex items-center gap-2 bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg transition-colors"
          >
            <Plus size={16} />
            Add Habit
          </button>
        </div>
  
        {#if habits.length === 0}
          <div class="text-center py-12">
            <TrendingUp class="mx-auto text-gray-400 mb-4" size={48} />
            <h3 class="text-lg font-medium text-gray-500 dark:text-gray-400 mb-2">
              No habits yet
            </h3>
            <p class="text-gray-400 dark:text-gray-500 mb-4">
              Start building better habits by adding your first one!
            </p>
            <button 
              on:click={() => showAddModal = true}
              class="bg-blue-500 hover:bg-blue-600 text-white px-6 py-2 rounded-lg transition-colors"
            >
              Add Your First Habit
            </button>
          </div>
        {:else}
          <div class="grid gap-4">
            {#each habits as habit (habit.id)}
              <HabitCard 
                {habit} 
                {selectedDate}
                on:toggle={() => toggleHabit(habit.id)}
                on:delete={() => deleteHabit(habit.id)}
              />
            {/each}
          </div>
        {/if}
      </div>
    </div>
  
    <!-- Add Habit Modal -->
    {#if showAddModal}
      <AddHabitModal 
        on:add={(e) => addHabit(e.detail)}
        on:close={() => showAddModal = false}
      />
    {/if}
  </main>
  
  <style>
    :global(body) {
      margin: 0;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }
  </style>