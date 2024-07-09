<script lang="ts">
  import { open } from "@tauri-apps/api/dialog";
  import { convertFileSrc } from "@tauri-apps/api/tauri";

  let src: string;
  let alt: string = "hogehoge";

  const openFile = async (): Promise<string> => {
    const fallbackPath = "";

    const filePath = await open({
      multiple: false,
      filters: [{ name: "Image", extensions: ["jpg", "jpeg", "png", "gif"] }],
    });

    if (filePath === null) return fallbackPath;
    if (Array.isArray(filePath)) return filePath[0];

    return filePath;
  };

  const handleClick = async () => {
    const rowFilePath = await openFile();

    const filePath = convertFileSrc(rowFilePath);

    src = filePath;
    alt = filePath;
  };
</script>

<div class="container">
  <div class="left-container">
    <img {src} {alt} />
  </div>
  <div class="right-container">
    <button on:click={handleClick}>Open</button>
  </div>
</div>

<style>
  img {
    object-fit: contain;
  }

  .container {
    display: flex;
    flex-direction: row;

    object-fit: contain;
  }

  .left-container {
    flex-grow: 1;
  }

  .right-container {
    width: auto;
  }
</style>
