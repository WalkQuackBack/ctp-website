<script lang="ts">
  import type { PortWithIcons } from "@data/ports";
  import PortCard from "./PortCard.svelte";

  interface Props {
    hydrated: boolean;
    portGrid: Array<PortWithIcons> | undefined;
    searchTerm: string;
  }

  let { hydrated, portGrid, searchTerm }: Props = $props();
</script>

{#if !hydrated}
  <div class="loadbar" aria-label="Loading..."></div>
{/if}
<div class="port-grid" class:hydrated>
  {#key portGrid}
    {#if searchTerm && portGrid && portGrid.length === 0}
      <div>
        <p>Sorry, we couldn't find any ports matching your search :(</p>
        <p>
          You can request a port to be themed by raising a
          <a href="https://github.com/catppuccin/catppuccin/discussions/new?category=port-requests"
            >port request in catppuccin/catppuccin</a
          >.
        </p>
      </div>
    {:else if portGrid && portGrid.length > 0}
      {#each portGrid as port (port.key)}
        <PortCard {port} />
      {/each}
    {/if}
  {/key}
</div>

<noscript>
  <style lang="scss">
    .loadbar {
      display: none;
    }
    .port-grid:not(.hydrated):not(.hydrated) {
      opacity: 1;
    }
  </style>
</noscript>

<style lang="scss">
  @use "../../../styles/utils";
  .loadbar {
    width: 100%;
    height: 4px;
    position: relative;
    overflow: hidden;
    margin-top: -10px;
    margin-bottom: 6px;
    clip-path: inset(0 0 round 2px);
    &::after {
      content: "";
      position: absolute;
      inset: 0;
      border-radius: inherit;
      background-color: var(--mauve);
      transform: translateX(-100%);

      animation: loadbar 1s ease infinite;
      animation-delay: 300ms;
    }
  }
  @keyframes loadbar {
    from {
      transform: translateX(-100%);
    }
    to {
      transform: translateX(100%);
    }
  }
  .port-grid {
    @include utils.grid($column: 300px, $gap: var(--space-sm));
    transition: opacity 400ms ease;
  }
  .port-grid:not(.hydrated) {
    opacity: 0.5;
  }
</style>
