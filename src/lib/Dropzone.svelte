<script>
  let { handleFiles } = $props();
  let files = [];

  function onDrop(event) {
    event.preventDefault();
    files = event.dataTransfer.files;
    handleFiles(files);
    event.currentTarget.classList.remove("dragover");
  }

  function onSelect(event) {
    files = event.target.files;
    handleFiles(files);
  }

  function openFileDialog() {
    document.getElementById("fileInput").click();
  }

  function onDragOver(event) {
    event.preventDefault();
    event.currentTarget.classList.add("dragover");
  }

  function onDragLeave(event) {
    event.currentTarget.classList.remove("dragover");
  }
</script>

<div
  class="drop-zone"
  role="button"
  tabindex="0"
  on:click={openFileDialog}
  on:drop={onDrop}
  on:dragover={onDragOver}
  on:dragleave={onDragLeave}
  on:keydown={(e) => e.key === "Enter" && openFileDialog()}
>
  <div class="drop-zone__prompt">
    <svg
      class="svg-upload"
      width="30"
      height="30"
      viewBox="0 0 24 24"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <path
        d="M11 14.9861C11 15.5384 11.4477 15.9861 12 15.9861C12.5523 15.9861 13 15.5384 13 14.9861V7.82831L16.2428 11.0711L17.657 9.65685L12.0001 4L6.34326 9.65685L7.75748 11.0711L11 7.82854V14.9861Z"
        fill="#5046E5"
      />
      <path
        d="M4 14H6V18H18V14H20V18C20 19.1046 19.1046 20 18 20H6C4.89543 20 4 19.1046 4 18V14Z"
        fill="#5046E5"
      />
    </svg>
  </div>
  <input
    type="file"
    name="file"
    id="fileInput"
    class="drop-zone__input"
    accept=".mp3,.mp4,.mpeg,.mpga,.m4a,.wav,.webm"
    on:change={onSelect}
  />
  <div class="file-info">
    <div>
      Using:
      <span id="model-identifier" style="font-weight: bold"
        >whisper-large-v3</span
      >
    </div>
    {#if files.length > 0}
      {#each Array.from(files) as file}
        <div>
          <span class="font-bold">Selected File: </span>
          {file.name} ({(file.size / 1024 / 1024).toFixed(2)} MB)
        </div>
      {/each}
    {/if}
    {#if files.length === 0}
      <div class="file-size-message">Max file size: 25MB</div>
    {/if}
    <br />
    <div class="format-chip-container">
      <div class="format-chip">mp3</div>
      <div class="format-chip">mp4</div>
      <div class="format-chip">mpeg</div>
      <div class="format-chip">mpga</div>
      <div class="format-chip">m4a</div>
      <div class="format-chip">wav</div>
      <div class="format-chip">webm</div>
    </div>
  </div>
</div>

<style>
  .drop-zone {
    width: 100%;
    /* max-width: 60%; */
    min-height: 200px;
    padding: 25px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    background-color: white;
    border: 2px dashed #ccc;
    border-radius: 10px;
    position: relative;
    transition: border-color 0.3s ease;
    cursor: pointer;
  }

  .drop-zone.dragover {
    border-color: #2196f3;
    background-color: rgba(33, 150, 243, 0.05);
  }

  .drop-zone:hover {
    border: 2px dashed light;
  }

  .drop-zone__prompt {
    font-size: 1.2em;
    color: #666;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    align-items: center;
    margin-bottom: 1.5rem;
  }

  .drop-zone__input {
    display: none;
  }

  .file-info {
    font-size: 0.9em;
    text-align: center;
    color: #888;
    margin-top: 10px;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .format-chip-container {
    display: flex;
    gap: 0.25rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .format-chip {
    padding: 0.25rem 0.5rem;
    border-radius: 0.25rem;
    background-color: #eee;
    color: #666;
    font-size: 0.8rem;
  }

  .svg-upload {
    outline: 2px dashed #ccc;
    border-radius: 50%;
    padding: 0.35rem;
  }
</style>
