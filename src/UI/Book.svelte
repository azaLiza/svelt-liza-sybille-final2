<script>
    import {
        Row,
        Col,
        Card,
        CardBody,
        CardHeader,
        CardSubtitle,
        CardText,
        CardTitle,
        Button
    } from 'sveltestrap';

    export let book;
    export let context = "list";
    export let handleAddToCart = () => { console.error("unexpected call of handleAddToCart for book", book); };
</script>

{#if context === "detail"}
    <Row class="justify-content-center">
        <Col xs="12" sm="6" lg="4" class="pb-4 pb-sm-0">
            <img class="w-100" src="data:image/jpeg;base64, {book.img.data}" alt="{book.title}" title="{book.title}" onerror="this.onerror=null; this.src='image/not-found.png'" />
        </Col>

        <Col xs="12" sm="6" lg="4">
            <h1>{book.title}</h1>
            <p class="h3 text-muted">by {book.author}</p>

            <p class="h5"><sup class="mr-1">$</sup>{book.price}</p>

            {#if book.description}
                <p>{book.description}</p>
            {:else}
                <p class="font-italic">No description available.</p>
            {/if}

            <Button color="primary" on:click={() => handleAddToCart(book)}>Add to cart</Button>
        </Col>
    </Row>
{:else}
    <Card class="mb-3">
        <a href="/#/detail/{book._id}">
            <CardHeader>
                <img class="w-100" src="data:image/jpeg;base64, {book.img.data}" alt="{book.title}" title="{book.title}" onerror="this.src='image/not-found.png'" />
                <CardTitle class="h6 mt-2">{book.title}</CardTitle>
                <CardSubtitle class="text-muted">by {book.author}</CardSubtitle>
            </CardHeader>
        </a>
        <CardBody>
            <CardText class="h6">
                <sup class="mr-1">$</sup>{book.price}
            </CardText>
            <!-- <Button on:click={() => addToCart(book)}>Add to cart</Button> -->
        </CardBody>
    </Card>
{/if}