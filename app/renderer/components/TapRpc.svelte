<script lang="ts">
  import TapRpcTab from '../pages/tabPage/MainTabPage.svelte'

  import {
    //@ts-ignore
    Tabs,
    //@ts-ignore
    TabContent,
    //@ts-ignore,
    Tab,
    Button,
    Icon,
  } from 'svelte-materialify/src'
  import { onMount} from 'svelte'
  import TabCloseButton from '../pages/tabPage/components/TabCloseButton.svelte'
  import type { TabConfigModel } from './types/types';
  import { appConfigStore } from '../../stores/appConfigStore';

  $: tabs = $appConfigStore.tabs

  const onTabChange = (e: any) => {
    const newIndex = e.detail
    if (newIndex >= 0) {
      console.log('New tab index = ', newIndex)
      appConfigStore.setActiveTab(newIndex)
    }
  }

  const onNewTabClick = () => {
    appConfigStore.addNewTab()
  }

  const closeTab = (tabIndex: number, tab: TabConfigModel) => {
    appConfigStore.removeTab(tabIndex)
  }
  $: activeTabIndex = $appConfigStore.activeTabIndex

  onMount(()=>{
    if($appConfigStore.tabs.length==0){
      appConfigStore.addNewTab()
    }
  })
</script>

<Tabs
  centerActive
  slider={false}
  class="primary-text elevation-1"
  value={activeTabIndex}
  on:change={onTabChange}
>
  <div slot="tabs">
    {#each tabs as tab, i (i)}
      <Tab value={i}>
        <div
          style="position:relative;text-transform: capitalize;
          {activeTabIndex === i ? 'font-weight:bold;' : ''}"
        >
          {#if tab.selectedRpc}{tab.selectedRpc.methodName}{:else}{"RPC"}{/if}
          <span>
            <TabCloseButton on:click={e => closeTab(i, tab)} />
          </span>
        </div>
      </Tab>
    {/each}
    <span class="new-tab-button">
      <Button fab size="x-small" on:click={onNewTabClick}>
        <Icon small class="mdi mdi-plus" />
      </Button>
    </span>
  </div>

  <TapRpcTab />
</Tabs>

<style>
  .new-tab-button {
    transform: translate(10px, 10px);
  }
</style>
