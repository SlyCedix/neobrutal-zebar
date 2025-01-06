<script lang="ts">
  import type { GlazeWmOutput } from "zebar";

  let { glazewm, className }: { glazewm: GlazeWmOutput; className: string } =
    $props();
</script>

{#if glazewm?.allWorkspaces
  .flatMap((ws) => ws.children)
  .filter((ch) => ch.type == "window" && ch.processName.toLowerCase() == "spotify").length}
  <div class="flex items-center gap-1 {className}">
    {#each glazewm.allWorkspaces as workspace}
      {#each workspace.children as child}
        {#if child.type == "window" && child.processName.toLowerCase() === "spotify"}
          {#if child.title.toLowerCase() === "spotify premium"}
            <i class="ti ti-music-off text-zb-spotify-paused"></i>
            nothing is playing
          {:else}
            <i class="ti ti-music text-zb-spotify-playing"></i>
            <span class="max-w-md truncate">
              {child.title}
            </span>
          {/if}
        {/if}
      {/each}
    {/each}
  </div>
{/if}
