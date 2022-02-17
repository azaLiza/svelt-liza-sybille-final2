<script>
    import { Alert } from 'sveltestrap';
    import Dump from "../Util/Dump.svelte";

    export let title;
    export let error;

    let type;
    let color;

    $: if (error) {
        type = "error";
        color = "danger";
    } else {
        type = "info";
        color = "info";
    }
</script>

<Alert color={color}>
    <p class="h5 alert-heading text-capitalize">{title}</p>
    <slot></slot>

    {#if type === "error"}
        <p>Please contact an administrator and provide them the following message:</p>
        <Dump value={error} error=true />
    {/if}
</Alert>