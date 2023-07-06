<script>
    import { onMount } from "svelte";
    import Cross from "$lib/cross.svelte";

    let items = ["Item 1", "Item 2", "Item 3", "Item 4"]
    let categories = ["Gemüse", "Category 2", "Category 3", "Category 4", "Category 5", "Category 6"]
    let products = ["Product 1", "Product 2", "Product 3", "Product 4", "Product 5", "Product 6", "Product 7", "Product 8", "Product 9"];
    let dialog;

	onMount(() => {
		dialog = document.getElementById('modal');
	})

    function openDialog() {
        dialog.showModal();
    }

    function closeModal() {
        dialog.close();
    }

    function removeItem() {
        alert("Removed item from shopping list!");
    }

    function filterCategory() {
        alert("Selected category!");
    }

    function addProduct() {
        alert("Added product to shopping list!");
    }

    function showInfo() {
        alert("Showing product info!");
    }
</script>

<section>
    <h1>ShoppingList</h1>
    <div class="items">
        <h2>Einkaufszettel</h2>
        <ul>
            {#each items as item}
                <li class="item">
                    <label>
                        <input type="checkbox">
                        {item}
                    </label>
                    <button class="remove-btn" on:click={removeItem}>
                        <Cross />
                    </button>
                </li>
            {/each}
        </ul>
    </div>
</section>

<div class="categories">
    {#each categories as category}
        <button class="category" on:click={filterCategory}>
            {category}
        </button>
    {/each}
</div>

<section>
    <div class="products">
        <ul>
            {#each products as product}
                <li class="product-wrapper">
                    <div class="product">
                        <button class="product-body" on:click={addProduct}>
                            {product}
                        </button>
                        <button class="product-info" on:click={showInfo}>
                            ?
                        </button>
                    </div>
                </li>
            {/each}
        </ul>
    </div>
</section>

<button class="add-product" on:click={openDialog}>
    <Cross />
</button>

<dialog id="modal">
    <h1>Produkt hinzufügen</h1>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Corrupti nostrum natus minus! Perferendis nemo ut repellendus unde deserunt ipsam earum amet a velit, blanditiis porro ipsa labore eos doloribus sequi?</p>
    <div class="buttons">
        <button class="button btn-primary-light" on:click={closeModal}>Abbrechen</button>
        <button class="button btn-primary" on:click={closeModal}>Hinzufügen</button>
    </div>
</dialog>