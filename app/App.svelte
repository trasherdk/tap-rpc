<script lang="ts">
  import TapRpc from "./renderer/components/TapRpc.svelte";
  import { MaterialApp } from "svelte-materialify";
  import { onDestroy, onMount } from "svelte";
  import { ProtoFilesDiskStore } from "./disk_storage/protos";
  import { ProtoUtil } from "./commons/utils";
  import { protoImportPathsStore } from "./stores";
  import AppSnackBar from "./renderer/components/microComponents/AppSnackBar.svelte";
  import { AppDataDiskStore } from "./disk_storage/appDataDiskStorage";
  import { appConfigStore } from "./stores/appConfigStore";

  onMount(async () => {
    const protoFilePaths = ProtoFilesDiskStore.fetchProtoFiles();
    await ProtoUtil.loadProtoFilesAndStartServer(
      protoFilePaths,
      $protoImportPathsStore
    );
    appConfigStore.setValue(AppDataDiskStore.fetchAppData());
  });
</script>

<MaterialApp>
  <TapRpc />
  <AppSnackBar />
</MaterialApp>

<style global>
  * {
    box-sizing: border-box;
  }
  html,
  body {
    position: relative;
    width: 100%;
    height: 100%;
  }

  label {
    display: block;
    user-select: none;
  }

  .d-flex {
    display: flex;
  }
  .row {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
  }
  .col {
    display: flex;
    flex-direction: column;
  }
  .center {
    text-align: center;
  }
  .border {
    border: 1px solid black;
  }

  .flex-expand {
    flex: 1 1 auto;
  }
</style>
