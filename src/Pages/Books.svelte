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
        Spinner,
        Icon
    } from 'sveltestrap';

    import Db from "../Database/Database.svelte";
    import DbDocs from "../Database/DatabaseDocuments.svelte";

    import Book from "../UI/Book.svelte";
    import Alert from "../UI/Alert.svelte";

    let booksDb = null;
    let booksPromise = null;
    let books = null;

    let isNavbarOpen = false;
    function handleNavbarUpdate(event) {
        isNavbarOpen = event.detail.isOpen;
    }

    // function addBook() {
    //     const date = (new Date()).toISOString();
    //     let myBook = {
    //         "author": date,
    //         "_id": "00" + date,
    //         "price": 9.99,
    //         "title": date,
    //         "img": {
    //             "data": "",
    //         }
    //     };
    //     books.unshift(myBook);
    //     books = books;
    // }

    // function deleteBook(theBook) {
    //     theBook._deleted = true;
    //     books = books;
    // }
</script>

<Db bind:db={booksDb} name="books" initDocsSrc="./books.json" />
<DbDocs bind:docsPromise={booksPromise} bind:docs={books} db={booksDb} />

<main>
    <Navbar color="primary" dark expand="sm" style="position: sticky; top: 0; z-index: 10;">
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
        {#if booksPromise !== null} <!-- if there is an actual promise -->
            <Row class="justify-content-center">
                {#await booksPromise}
                    <Spinner color="primary" />
                {:then}
                    {#each books as book}
                        <Col xs="6" sm="6" md="4" lg="3" xl="2" class="px-2">
                            <Book book={book} />
                        </Col>
                    {:else}
                        <Alert title="No books found">
                            <p>There is currently no books in the database.</p>
                        </Alert>
                    {/each}
                {:catch error}
                    <Alert title="Unable to fetch books" {error}>
                        <p>The connection to the database failed, retry in a few minutes.</p>
                    </Alert>
                {/await}
            </Row>
        {/if}
    </Container>
</main>
