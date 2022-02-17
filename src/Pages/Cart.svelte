<svelte:head>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">

    <style>
        body {
            margin: 0;
            padding: 0;
        }

        .table td {
            vertical-align: middle;
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
        Icon,
        Table
    } from 'sveltestrap';

    import Db from "../Database/Database.svelte";
    import DbDocs from "../Database/DatabaseDocuments.svelte";
    import DbDoc from "../Database/DatabaseDocument.svelte";

    import Alert from "../UI/Alert.svelte";
    import Dump from "../Util/Dump.svelte";

    let cartDb = null;
    let cartPromise = null;
    let cart = null;

    let isNavbarOpen = false;
    function handleNavbarUpdate(event) {
        isNavbarOpen = event.detail.isOpen;
    }

    function deleteItem(theItem) {
        theItem._deleted = true;
        cart = cart;
    }

    let booksDb = null;
    let books = [];
    let total = 0;
</script>

<Db bind:db={cartDb} name="cart" />
<DbDocs bind:docsPromise={cartPromise} bind:docs={cart} db={cartDb} />

<Db bind:db={booksDb} name="books" />

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
        <h1>Cart</h1>

        <Row class="justify-content-center">
            {#if cartPromise !== null} <!-- if there is an actual promise -->
                {#await cartPromise}
                    <Spinner color="primary" />
                {:then}
                    <Table>
                        {#if cart.length > 0}
                            <tr>
                                <th></th>
                                <th>Title</th>
                                <th style="width: 1px; white-space: nowrap;">Unit price</th>
                                <th style="width: 1px; white-space: nowrap;">Quantity</th>
                                <th style="width: 1px; white-space: nowrap;">Total</th>
                                <th></th>
                            </tr>
                        {/if}

                        {#each cart as item}
                            <DbDoc bind:doc={books[item.bookId]} db={booksDb} docId={item.bookId} />
                            {#if books[item.bookId]}
                                <tr>
                                    <td class="d-none d-sm-table-cell" style="width: 4rem"><img class="w-100" src="data:image/jpeg;base64, {books[item.bookId].img.data}" alt="{books[item.bookId].title}" title="{books[item.bookId].title}" onerror="this.src='image/not-found.png'" /></td>
                                    <td>{books[item.bookId].title}</td>
                                    <td style="width: 1px; white-space: nowrap;"><sup class="mr-1">$</sup>{books[item.bookId].price}</td>
                                    <td style="width: 1px; white-space: nowrap;"><Input type="number" min="1" bind:value={item.amount} style="width: 4rem" /></td>
                                    <td style="width: 1px; white-space: nowrap;">{books[item.bookId].price * item.amount}</td>

                                    <td style="width: 1px; white-space: nowrap;"><Button color="danger" on:click={() => deleteItem(item)}><Icon name="trash"/></Button></td>
                                </tr>
                               
                            {/if}
                             

                        {:else}
                            <Alert title="Empty cart">
                                <p>There is currently no books in your cart.</p>
                            </Alert>
                        {/each}

                        {#if cart.length > 0}
                            <tr>
                                <th></th>
                                <th>Total</th>
                            {#each cart as item}
                            <DbDoc bind:doc={books[item.bookId]} db={booksDb} docId={item.bookId} />
                            {#if books[item.bookId]}
                                <th style="width: 1px; white-space: nowrap;">{total = total + books[item.bookId].price * item.amount}</th>

                            {/if}
                             

                        {/each}

                                
                                     <th style="width: 1px; white-space: nowrap;">{total}</th>
                                
                            </tr>
                        {/if}

                    </Table>
                {:catch error}
                    <Alert title="Unable to fetch cart" {error}>
                        <p>The connection to the database failed, retry in a few minutes.</p>
                    </Alert>
                {/await}
            {/if}
        </Row>
    </Container>
</main>
