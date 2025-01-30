<script lang="ts">
  import type { BatteryOutput, CpuOutput, MemoryOutput } from "zebar";

  import Button from "./Button.svelte";
  import Meter from "./Meter.svelte";

  type LeftGroupProps = {
    battery: BatteryOutput;
    cpu: CpuOutput;
    memory: MemoryOutput;
  };

  let { battery, cpu, memory }: LeftGroupProps = $props();
</script>

<div class="flex flex-row items-center divide-x-[1px] divide-dashed">
  <Button class="text-zb-icon mx-1.5">
    <i class="ti ti-cat"></i>
  </Button>
  <div class="flex gap-1 items-center px-1.5">
    <i class="ti ti-ruler-2"></i>
    <Meter class="bg-zb-memory" percent={Math.round(memory?.usage ?? 0)} />
  </div>
  <div class="flex gap-1 items-center px-1.5">
    <i class="ti ti-cpu"></i>
    <Meter class="bg-zb-cpu" percent={Math.round(cpu?.usage ?? 0)} />
  </div>
  {#if battery}
    <div class="flex gap-1 items-center px-1.5">
      <i class="ti ti-bolt"></i>
      <Meter
        class="bg-zb-battery-good"
        percent={Math.round(battery?.chargePercent ?? 100)}
      />
    </div>
  {/if}
</div>
