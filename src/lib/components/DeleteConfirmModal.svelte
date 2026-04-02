<script>
  import { createEventDispatcher } from "svelte";
  import { AlertTriangle, X } from "lucide-svelte";

  const dispatch = createEventDispatcher();

  /** @type {string} */
  export let habitName = "";

  function handleConfirm() {
    dispatch("confirm");
  }

  function handleCancel() {
    dispatch("cancel");
  }

  /**
   * @param {KeyboardEvent} event
   */
  function handleKeydown(event) {
    if (event.key === "Escape") {
      handleCancel();
    }
  }
</script>

<svelte:window on:keydown={handleKeydown} />

<div
  class="fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center p-4 z-[100]"
  on:click={handleCancel}
  on:keydown={handleKeydown}
  role="presentation"
>
  <div
    class="bg-white dark:bg-gray-800 rounded-2xl shadow-2xl p-6 w-full max-w-sm border border-gray-100 dark:border-gray-700"
    on:click|stopPropagation
    role="dialog"
    aria-modal="true"
  >
    <div class="flex flex-col items-center text-center">
      <div
        class="mb-4 p-3 bg-red-50 dark:bg-red-900/30 rounded-full text-red-500 dark:text-red-400"
      >
        <AlertTriangle size={32} />
      </div>

      <h2 class="text-xl font-bold text-gray-900 dark:text-white mb-2">
        Delete Habit?
      </h2>

      <p class="text-gray-600 dark:text-gray-400 mb-8">
        Are you sure you want to delete <span
          class="font-semibold text-gray-900 dark:text-gray-200"
          >"{habitName}"</span
        >? This action cannot be undone.
      </p>

      <div class="flex gap-3 w-full">
        <button
          on:click={handleCancel}
          class="flex-1 px-4 py-2.5 rounded-xl bg-gray-100 hover:bg-gray-200 dark:bg-gray-700 dark:hover:bg-gray-600 text-gray-700 dark:text-gray-200 font-medium transition-colors outline-none focus:ring-2 focus:ring-gray-300 dark:focus:ring-gray-500"
        >
          Cancel
        </button>
        <button
          on:click={handleConfirm}
          class="flex-1 px-4 py-2.5 rounded-xl bg-red-500 hover:bg-red-600 text-white font-medium transition-colors shadow-lg shadow-red-200 dark:shadow-none outline-none focus:ring-2 focus:ring-red-400"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</div>
