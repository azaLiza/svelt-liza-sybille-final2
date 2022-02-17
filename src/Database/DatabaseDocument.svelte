<script>
    import PouchDB from 'pouchdb-browser';

    export let docPromise = null;
    export let doc = null;
    export let db = null;
    export let docId;

    $: if (db !== null) { // if database is ready
        docPromise = pullDoc(db);
    }

    $: if (docPromise !== null) {
        (async () => {
            doc = await docPromise;
        })();
    }

    async function pullDoc(database) {
        if (!database) throw new Error('database must be up')
        const doc = await database.get(docId);
        return doc;
    }
</script>
