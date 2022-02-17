<script>
    import PouchDB from 'pouchdb-browser';
    import hash from 'object-hash';
    import { inspect } from 'util';

    export let docsPromise = null;
    export let docs = null;
    export let db = null;
    let localDocsHash = "";

    $: if (db !== null) { // if database is ready
        setDocsPromise();
    }

    function setDocsPromise() {
        docsPromise = fetchDocs();
    }

    $: if (docsPromise !== null) {
        (async () => {
            docs = await docsPromise;
            generateLocalDocsHash();
        })();
    }

    function generateLocalDocsHash() {
        localDocsHash = hash(docs);
    }

    $: if (docs !== null) {
        if (hash(docs) !== localDocsHash) {
            db.bulkDocs(docs);
            setDocsPromise();
        }
    }

    $: console.debug("db", db);
    $: console.debug("docsPromise", docsPromise);
    $: console.debug("docs", docs);

    async function fetchDocs() {
        if (!db) throw new Error('database must be up')
        const documents = await db.allDocs({ include_docs: true, update_seq: true });

        // For testing purpose

        // await new Promise(resolve => setTimeout(resolve, 2000 + Math.random() * 3000));
        // const rnd = Math.random();
        // if (rnd < 1/3)
        //     throw new Error('not working');
        // if (rnd < 2/3)
        //     return [];

        return documents.rows.map(row => row.doc);
    }
</script>
