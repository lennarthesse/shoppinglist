<script>
    import { onMount } from "svelte";
    import Cross from "$lib/cross.svelte";

    let items = []
    let categories = ["Obst", "Gemüse", "Süßigkeiten", "Aufschnitt"]
    let products = [{name: "Nudeln - Fussili", brand: "Barilla", price: null, category: null}, {name: "Tomaten", brand: null, price: null, category: "Gemüse"}];

    let modalEditProduct;
    let currentProduct;

    let modalAddProduct;
    let modalAddProductMore = false;

	onMount(() => {
        modalEditProduct = document.getElementById('edit-product-modal');
		modalAddProduct = document.getElementById('add-product-modal');
	})

    function openModalAddProduct() {
        let form = document.getElementById("add-product-form");
        form.reset();

        modalAddProductMore = false;
        modalAddProduct.showModal();
    }
    
    function addProduct() {
        let productName = document.getElementById("product-name").value;
        let productBrand = document.getElementById("product-brand") ? document.getElementById("product-brand").value == "" ? null : document.getElementById("product-brand").value : null;
        let productPrice = document.getElementById("product-price") ? document.getElementById("product-price").value == "" ? null : document.getElementById("product-price").value : null;
        let productCategory = document.getElementById("product-category") ? document.getElementById("product-category").value == "" ? null : document.getElementById("product-category").value : null;

        let product = {
            name: productName,
            brand: productBrand,
            price: productPrice,
            category: productCategory
        }
        
        products = [...products, product];
    }
    
    function editProduct(product) {
        currentProduct = product;
        modalEditProduct.showModal();
    }

    function addProductToList(product) {
        let item = {
            name: product.name,
            checked: false

        }
        items = [...items, item];
    }
    
    function removeItemFromList(item) {
        for (let i = 0; i < items.length; i++) {
            if (items[i] == item) {
                items.splice(i, 1);
                items = [...items];
                break;
            }
        }
    }

    function filterCategory() {
        alert("Selected category!");
    }
</script>

<svelte:head>
    <title>ShoppingList</title>
</svelte:head>

<section>
    <h1>ShoppingList</h1>
    <div class="items">
        <h2>Einkaufszettel</h2>
        <ul>
            {#each items as item}
                <li class="item">
                    <label>
                        <input type="checkbox" bind:checked={item.checked}>
                        <span>{item.name}</span>
                    </label>
                    <button class="remove-btn" on:click={removeItemFromList(item)}>
                        <Cross />
                    </button>
                </li>
            {/each}
        </ul>
    </div>
</section>

<div class="categories" tabindex="-1">
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
                        <button class="product-body" on:click={addProductToList(product)}>
                            {product.name}
                        </button>
                        <button class="product-info" on:click={editProduct(product)}>
                            ?
                        </button>
                    </div>
                </li>
            {/each}
        </ul>
        <dialog id="edit-product-modal" class="modal">
            <h1>Product Info</h1>
            {#if currentProduct}
                <p>{currentProduct.name}</p>
                <p>{currentProduct.brand}</p>
                <p>{currentProduct.price}</p>
                <p>{currentProduct.category}</p>
            {/if}
        </dialog>
    </div>
</section>

<button class="add-product" on:click={openModalAddProduct}>
    <Cross />
</button>

<dialog id="add-product-modal" class="modal">
    <h1>Produkt hinzufügen</h1>
    <form method="dialog" id="add-product-form" on:submit={addProduct}>
        <div class="input-text">
            <label for="product-name">Produktname</label>
            <input type="text" id="product-name" required>
        </div>

        {#if !modalAddProductMore}
            <button class="button btn-primary-light" on:click={() => {modalAddProductMore = true;}}>Mehr Optionen...</button>
        {/if}
        
        {#if modalAddProductMore}
            <div class="input-text">
                <label for="product-brand">Marke</label>
                <input type="text" id="product-brand">
            </div>

            <div class="input-text">
                <label for="product-price">Preis (in Euro)</label>
                <input type="number" step="0.01" id="product-price">
            </div>

            <div class="input-text">
                <label for="product-category">Kategorie</label>
                <select id="product-category">
                    <option value="">Auswählen...</option>
                    {#each categories as category}
                        <option>{category}</option>
                    {/each}
                </select>
            </div>
            <button class="button btn-primary-light" on:click={() => {modalAddProductMore = false;}}>Weniger Optionen...</button>
        {/if}

        <div class="buttons">
            <button class="button btn-primary">Hinzufügen</button>
            <button class="button btn-primary-light" on:click={() => {modalAddProduct.close();}}>Abbrechen</button>
        </div>
    </form>
</dialog>