<script>
  let { text: propText, loading: propLoading } = $props();
  let text = $derived(propText);
  let loading = $derived(propLoading);

  function copyToClipboard() {
    navigator.clipboard.writeText(text);
  }

  function downloadtxt() {
    const trimmedText = text.trim();
    const element = document.createElement("a");
    const file = new Blob([trimmedText], { type: "text/plain" });
    element.href = URL.createObjectURL(file);
    element.download = "transcription.txt";
    document.body.appendChild(element); // required for this to work in FireFox
    element.click();
  }
</script>

<div
  class="outline-2 outline-dashed outline-gray-300 rounded-md p-4 flex flex-col gap-4 font-sans text-sm font-medium overflow-y-auto mb-6 max-h-full min-h-4"
>
  <div id="header" class="flex justify-between">
    <div class="bg-gray-100 text-xs font-medium p-2 w-fit rounded-md">
      {#if loading}
        <svg
          class="animate-spin h-4 w-4 text-blue-800"
          xmlns="http://www.w3.org/2000/svg"
          fill="#EEF2FF"
          viewBox="0 0 24 24"
        >
          <circle
            class="opacity-25"
            cx="12"
            cy="12"
            r="10"
            stroke="currentColor"
            stroke-width="4"
          ></circle>
          <path
            class="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
          ></path>
        </svg>
      {:else}
        <p>Result:</p>
      {/if}
    </div>
    <div
      class="bg-gray-100 text-xs font-medium p-2 w-fit rounded-md flex gap-2"
    >
      <!-- svelte-ignore a11y_click_events_have_key_events -->
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <!-- svelte-ignore a11y_missing_attribute -->
      <a class="cursor-pointer" onclick={downloadtxt}>Download .txt</a>
      <p>|</p>
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <!-- svelte-ignore a11y_click_events_have_key_events -->
      <!-- svelte-ignore a11y_missing_attribute -->
      <a class="cursor-pointer" onclick={copyToClipboard}>Copy</a>
    </div>
  </div>
  <p>
    {text}
  </p>
</div>
