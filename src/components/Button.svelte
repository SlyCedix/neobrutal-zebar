<script lang="ts">
  type ButtonProps = {
    label?: string;
    children?: CallableFunction;
    callback?: () => void;
    class: string;
  };
  let { label, children, callback, ...rest }: ButtonProps = $props();
</script>

<button
  class="flex bg-blend-30 border border-zb-border rounded-base shadow-button hover:translate-x-boxShadowX hover:translate-y-boxShadowY hover:shadow-none p-1 {rest.class}"
  onclick={callback}
  aria-label={label}
>
  {@render children?.()}
</button>
