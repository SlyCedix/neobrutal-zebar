<script lang="ts">
  import type { Window } from "glazewm";
  import type { GlazeWmOutput } from "zebar";

  import iconMap from "$lib/icon_map.json";
  import ignoredApps from "$lib/ignored_apps.json";

  import Button from "./Button.svelte";

  const getProcessIcon = (child: Window) => {
    const possibleAppNames = [
      child.title.toLowerCase(),
      child.processName.toLowerCase()
    ];

    if (ignoredApps.find((app) => possibleAppNames.includes(app.name))) return;

    let entry = iconMap.find((entry) =>
      entry.appNames
        .map((name) => name.toLowerCase())
        .some((name) => possibleAppNames.includes(name))
    );

    return entry?.iconName ?? "ti-background";
  };

  let { glazewm }: { glazewm: GlazeWmOutput } = $props();
</script>

{#if glazewm}
  <div class="flex flex-row gap-2 items-center">
    {#each glazewm.currentWorkspaces as workspace, i}
      <Button
        class={workspace.children.length
          ? "text-zb-ws-" + i
          : "text-zb-ws-empty"}
        callback={() =>
          glazewm!.runCommand(`focus --workspace ${workspace.name}`)}
      >
        {@const icons = workspace.children
          .map((c) => c as Window)
          .filter((w) => w !== null)
          .map((w) => [getProcessIcon(w), w.hasFocus])
          .filter((p) => p[0] !== null)}
        {console.log(icons)}
        {#if !icons.length}
          <i class="ti {workspace.hasFocus ? 'ti-point-filled' : 'ti-point'}"
          ></i>
        {/if}
        {#each icons as pairs}
          <span
            class={pairs[1]
              ? "text-zb-focused-process"
              : workspace.hasFocus
                ? "text-zb-process"
                : ""}
          >
            <i class="ti {pairs[0]}"></i>
          </span>
        {/each}
      </Button>
    {/each}
    <button
      aria-label="tiling-direction"
      class="flex items-center justify-center text-zb-tiling-direction"
      onclick={() => glazewm!.runCommand("toggle-tiling-direction")}
    >
      <i class="ti ti-switch-{glazewm?.tilingDirection}"></i>
    </button>
    {#each glazewm.bindingModes as bindingMode, i}
      <div class="flex items-center">
        <button
          class="pb-[0px]"
          onclick={() => {
            glazewm!.runCommand(
              `wm-disable-binding-mode --name ${bindingMode.name.toLowerCase()}`
            );
          }}
        >
          {bindingMode.displayName ?? bindingMode.name}
        </button>
      </div>
    {/each}
  </div>
{/if}
