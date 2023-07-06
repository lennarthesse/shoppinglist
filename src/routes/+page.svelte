<script>
    import { onMount } from "svelte";
    import Cross from "$lib/cross.svelte";

    let items = ["Item 1", "Item 2", "Item 3", "Item 4"]
    let categories = ["Gemüse", "Category 2", "Category 3", "Category 4", "Category 5", "Category 6"]
    let products = ["Product 1", "Product 2", "Product 3", "Product 4", "Product 5", "Product 6", "Product 7", "Product 8", "Product 9"];
    let modal;

	onMount(() => {
		modal = document.getElementById('modal');
	})

    function openModal() {
        modal.showModal();
    }

    function closeModal() {
        modal.close();
        let form = document.getElementById("add-product-form");
        form.reset();
    }
    
    function addProduct() {
        alert("gagagugu");
        let form = document.getElementById("add-product-form");
        form.reset();
    }

    function removeItemFromList() {
        alert("Removed item from shopping list!");
    }

    function filterCategory() {
        alert("Selected category!");
    }

    function addProductToList() {
        alert("Added product to shopping list!");
    }

    function editProduct() {
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
                    <button class="remove-btn" on:click={removeItemFromList}>
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
                        <button class="product-body" on:click={addProductToList}>
                            {product}
                        </button>
                        <button class="product-info" on:click={editProduct}>
                            ?
                        </button>
                    </div>
                </li>
            {/each}
        </ul>
    </div>
</section>

<button class="add-product" on:click={openModal}>
    <Cross />
</button>

<dialog id="modal">
    <h1>Produkt hinzufügen</h1>
    <form method="dialog" id="add-product-form" on:submit={addProduct}>
        <div class="input-text">
            <label for="product-name">Produktname</label>
            <input type="text" name="product-name" id="product-name" required>
        </div>
        
        <div class="input-text">
            <label for="product-brand">Marke</label>
            <input type="text" name="product-brand" id="product-brand">
        </div>

        <div class="buttons">
            <button class="button btn-primary-light" on:click={closeModal}>Abbrechen</button>
            <button class="button btn-primary">Hinzufügen</button>
        </div>
    </form>
</dialog>