<script>
    import Router from 'svelte-spa-router';
    import Books from './Pages/Books.svelte';
    import Detail from './Pages/Detail.svelte';
    import Cart from './Pages/Cart.svelte';
</script>

<Router routes={{
    '/': Books,
    '/detail/:id': Detail,
    '/cart': Cart
}} />
