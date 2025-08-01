          <!-- src/lib/components/AddHabitModal.svelte -->
          <script>
            import { createEventDispatcher } from 'svelte';
            import { X } from 'lucide-svelte';
            
            const dispatch = createEventDispatcher();
            
            let name = '';
            let category = 'Health';
            let color = '#3b82f6';
            
            const categories = [
              'Health', 'Fitness', 'Learning', 'Productivity', 
              'Mindfulness', 'Social', 'Creative', 'Other'
            ];
            
            const colors = [
              '#3b82f6', '#ef4444', '#10b981', '#f59e0b',
              '#8b5cf6', '#ec4899', '#06b6d4', '#84cc16'
            ];
            
            function handleSubmit() {
              if (name.trim()) {
                dispatch('add', {
                  name: name.trim(),
                  category,
                  color
                });
                // Reset form
                name = '';
                category = 'Health';
                color = '#3b82f6';
              }
            }
            
            function handleClose() {
              dispatch('close');
            }
            
            /**
             * @param {KeyboardEvent} event
             */
            function handleKeydown(event) {
              if (event.key === 'Escape') {
                handleClose();
              }
            }
          </script>
          
          <svelte:window on:keydown={handleKeydown} />
          
          <!-- Modal Backdrop -->
          <div 
            class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50" 
            on:click={handleClose}
            on:keydown={handleKeydown}
            role="presentation"
          >
            <!-- Modal Content -->
            <div 
              class="bg-white dark:bg-gray-800 rounded-lg shadow-xl p-6 w-full max-w-md max-h-[90vh] overflow-y-auto" 
              on:click|stopPropagation
              role="dialog"
              aria-modal="true"
            >
              <!-- Header -->
              <div class="flex items-center justify-between mb-6">
                <h2 class="text-xl font-semibold text-gray-800 dark:text-white">
                  Add New Habit
                </h2>
                <button
                  on:click={handleClose}
                  class="p-1 text-gray-400 hover:text-gray-600 dark:hover:text-gray-200 rounded-md transition-colors"
                  aria-label="Close modal"
                >
                  <X size={20} />
                </button>
              </div>
              
              <!-- Form -->
              <form on:submit|preventDefault={handleSubmit}>
                <!-- Habit Name -->
                <div class="mb-4">
                  <label for="habit-name" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
                    Habit Name
                  </label>
                  <input
                    id="habit-name"
                    type="text"
                    bind:value={name}
                    placeholder="e.g., Drink 8 glasses of water"
                    class="w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md bg-white dark:bg-gray-700 text-gray-900 dark:text-white focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                    required
                  />
                </div>
                
                <!-- Category -->
                <div class="mb-4">
                  <label for="category" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
                    Category
                  </label>
                  <select
                    id="category"
                    bind:value={category}
                    class="w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md bg-white dark:bg-gray-700 text-gray-900 dark:text-white focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none"
                  >
                    {#each categories as cat}
                      <option value={cat}>{cat}</option>
                    {/each}
                  </select>
                </div>
                
                <!-- Color -->
                <div class="mb-6">
                  <fieldset>
                    <legend class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
                      Color
                    </legend>
                  <div class="flex gap-2 flex-wrap">
                    {#each colors as colorOption}
                      <button
                        type="button"
                        class="w-8 h-8 rounded-full border-2 border-white shadow-md relative hover:scale-110 transition-transform cursor-pointer {color === colorOption ? 'ring-2 ring-gray-400 dark:ring-gray-300' : ''}"
                        style="background-color: {colorOption}"
                        on:click={() => color = colorOption}
                        aria-label="Select color {colorOption}"
                      >
                        {#if color === colorOption}
                          <div class="absolute inset-0 rounded-full flex items-center justify-center text-white font-bold text-sm" style="background-color: rgba(255, 255, 255, 0.3);">
                            ✓
                          </div>
                        {/if}
                      </button>
                    {/each}
                  </fieldset>
                </div>
                
                <!-- Actions -->
                <div class="flex gap-3 justify-end">
                  <button
                    type="button"
                    on:click={handleClose}
                    class="bg-gray-200 hover:bg-gray-300 dark:bg-gray-600 dark:hover:bg-gray-500 text-gray-800 dark:text-white px-4 py-2 rounded-md transition-colors"
                  >
                    Cancel
                  </button>
                  <button
                    type="submit"
                    class="bg-blue-500 hover:bg-blue-600 disabled:bg-gray-300 disabled:cursor-not-allowed text-white px-4 py-2 rounded-md transition-colors"
                    disabled={!name.trim()}
                  >
                    Add Habit
                  </button>
                </div>
              </form>
              </div>
          </div>
          
          