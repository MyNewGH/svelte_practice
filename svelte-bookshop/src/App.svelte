<script>
	/* npx degit sveltejs/template svelte-typescript-app
cd svelte-typescript-app
node scripts/setupTypeScript.js */
  import BookCard from "./BookCard.svelte";
  let title = "";
  let price = 0;
  let description = "";
  let books = [];
  let bookCard = [];
	$: carBookNum = bookCard.length;
	$: totalPrice = bookCard.reduce((prev,next)=>prev+next.price,0)
  const handleTitleChange = (e) => {
    title = e.target.value;
  };
  const handleAddBook = () => {
    books = books.concat({ title, price, description });
    title = "";
    price = "";
    description = "";
  };
  const handleAddBookToCars = (title) => {
    let carBook = books.find((book) => book.title === title);
    bookCard = bookCard.concat(carBook);
  };
</script>

<main>
  <h1>Welcome to my online bookstore!</h1>
  <section>
    <h2>Add new book</h2>
    <label for="title">Title</label>
    <input type="text" id="title" bind:value={title} />
    <label for="price">Price</label>
    <input type="number" id="price" bind:value={price} />
    <label for="description">Description</label>
    <textarea rows="3" id="description" bind:value={description} />
    <div>
      <button on:click={handleAddBook}>添加书籍</button>
    </div>
  </section>

  <section>
    <h2>购物车</h2>
    {#if !bookCard.length}
      <p>购物车为空哦~</p>
    {:else}
      {#each bookCard as { title, price }}
        <div>名称: {title}, 价钱: {price}</div>
      {/each}
			<h2>购物车</h2>
			<div>书本总数：{carBookNum}</div>
			<div>订单金额：{totalPrice}</div>
    {/if}
  </section>

  <section>
    <h2>书单</h2>
    {#each books as { title, price, description }}
      <BookCard {title} {price} {description} {handleAddBookToCars} />
    {/each}
  </section>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
  button {
    background-color: #ff3e00;
    color: white;
    border-radius: 5px;
    cursor: pointer;
    border: none;
  }
  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
