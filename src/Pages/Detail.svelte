<svelte:head>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">

    <style>
        body {
            margin: 0;
            padding: 0;
        }
    </style>
</svelte:head>

<script>
import { type } from 'os';

    import { fade, fly } from 'svelte/transition';

    export let params={};
    import {
        Collapse,
        Navbar,
        NavbarToggler,
        NavbarBrand,
        Nav,
        NavItem,
        Form,
        Input,
        Button,
        UncontrolledDropdown,
        DropdownToggle,
        DropdownMenu,
        DropdownItem,
        Container,
        Row,
        Col,
        Card,
        CardBody,
        CardHeader,
        CardSubtitle,
        CardText,
        CardTitle,
        Spinner,
        Pagination,
        PaginationItem,
        PaginationLink,
        Icon
    } from 'sveltestrap';

    import Db from "../Database/Database.svelte";
    import DbDoc from "../Database/DatabaseDocument.svelte";
    import DbDocs from "../Database/DatabaseDocuments.svelte";

    import Book from "../UI/Book.svelte";
    import Alert from "../UI/Alert.svelte";

    let booksDb = null;
    let bookPromise = null;
    let book = null;

    let cartDb = null;
    let cartPromise = null;
    let cart = null;

    let isNavbarOpen = false;
    function handleNavbarUpdate(event) {
        isNavbarOpen = event.detail.isOpen;
    }

    function addToCart(theBook) {
        let theItem = null;
        cart.forEach(item => {
            if (item.bookId == theBook._id)
                theItem = item;
        });

        if (theItem === null) {
            theItem = {
                "_id": new Date().toJSON(),
                "bookId": theBook._id,
                "amount": 0
            };
            cart.push(theItem);
        }

        theItem.amount++;
        cart = cart;

        alert('Added to cart');
    }

    $: console.warn("cart", cart);
</script>

<Db bind:db={booksDb} name="books" initDocsSrc="./books.json" />
<DbDoc bind:docPromise={bookPromise} bind:doc={book} db={booksDb} docId={params.id} />

<Db bind:db={cartDb} name="cart" />
<DbDocs bind:docsPromise={cartPromise} bind:docs={cart} db={cartDb} />

<main>
    <Navbar color="primary" dark expand="sm">
        <Container>
            <NavbarBrand href="/">Book fugue</NavbarBrand>
            <NavbarToggler on:click={() => (isNavbarOpen = !isNavbarOpen)} />

            <Collapse isOpen={isNavbarOpen} navbar expand="sm" on:update={handleNavbarUpdate}>
                <Nav class="ml-auto" navbar>
                    <!--NavItem>
                        <Button on:click={addBook}>Create book</Button>
                    </NavItem>
                    <NavItem>
                        <Form class="form-inline">
                            <Input type="search" class="mr-sm-2" />
                            <Button color="success" class="my-2 my-sm-0">Search</Button>
                        </Form>
                    </NavItem-->
                    <NavItem>
                        <Button color="success" href="/#/cart"><Icon name="cart" /></Button>
                    </NavItem>
                </Nav>
            </Collapse>
        </Container>
    </Navbar>

    <Container class="py-3">
        {#if bookPromise !== null} <!-- if there is an actual promise -->
            {#await bookPromise}
                <Spinner color="primary" />
            {:then}
                <Book book={book} context="detail" handleAddToCart={addToCart}/>
            {:catch error}
                <Alert title="Unable to fetch book" {error}>
                    <p>The connection to the database failed, retry in a few minutes.</p>
                </Alert>
            {/await}
        {/if}
    </Container>
</main>