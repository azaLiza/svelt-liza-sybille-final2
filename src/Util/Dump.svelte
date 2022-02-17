<script>
    import { inspect } from 'util';

    export let value;
    export let error = false;

    if (error) console.error(value);
    else console.log(value);
</script>

<pre><code>{inspect(value, $$props)}</code></pre>
