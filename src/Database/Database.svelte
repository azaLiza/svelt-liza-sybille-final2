<script>
    import { onMount } from 'svelte';
    import PouchDB from 'pouchdb-browser';

    export let db;
    export let name;
    export let initDocsSrc = null;

    onMount(
        async () => {
            const database = new PouchDB(name);
            const res = await database.info(); // wait until database is up

            if (res.update_seq === 0 && initDocsSrc !== null) { // if there is no revision (aka nothing) in the DB and a JSON to initialize the DB
                const res = await fetch(initDocsSrc);
                const docs = await res.json();
                database.bulkDocs(docs); // populate the database with the initial docs
            }

            db = database;
        }
    );
</script>
