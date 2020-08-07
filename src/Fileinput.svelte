<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  const onFile = getFilesFunction => event => {
    const files = getFilesFunction(event);
    if (files.length) {
      dispatch("input", { file: files[0] });
    }
  };

  function getFilesFromInputEvent({ target }) {
    const files = target.files ? [...target.files] : [];
    target.value = "";
    return files;
  }
</script>

<input type="file" accept=".csv" on:input={onFile(getFilesFromInputEvent)} />
