<script>
    import { onMount } from "svelte";
    import Cross from "$lib/cross.svelte";

    let items = ["Item 1", "Item 2", "Item 3", "Item 4"]
    let categories = ["Gemüse", "Category 2", "Category 3", "Category 4", "Category 5", "Category 6"]
    let products = [{ name: "Nudeln - Fussili", brand: "Barilla", price: null }, { name: "Tomaten", brand: null, price: null }];
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
        let productName = document.getElementById("product-name").value;
        let productBrand = document.getElementById("product-brand").value == "" ? null : document.getElementById("product-brand").value;
        let productPrice = document.getElementById("product-price").value == "" ? null : document.getElementById("product-price").value;

        let product = {
            name: productName,
            brand: productBrand,
            price: productPrice
        }

        console.log(product);

        products = [...products, product];

        let form = document.getElementById("add-product-form");
        form.reset();
    }
    
    function editProduct() {
        alert("Showing product info!");
    }

    function addProductToList(product) {
        items = [...items, product.name];
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

<section>
    <h1>ShoppingList</h1>
    <div class="items">
        <h2>Einkaufszettel</h2>
        <ul>
            {#each items as item}
                <li class="item">
                    <label>
                        <input type="checkbox">
                        <span>{item}</span>
                    </label>
                    <button class="remove-btn" on:click={removeItemFromList(item)}>
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
                        <button class="product-body" on:click={addProductToList(product)}>
                            {product.name}
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

        <div class="input-text">
            <label for="product-price">Preis (in Euro)</label>
            <input type="number" step="0.01" name="product-price" id="product-price">
        </div>

        <div class="buttons">
            <button class="button btn-primary">Hinzufügen</button>
            <button class="button btn-primary-light" on:click={closeModal}>Abbrechen</button>
        </div>
    </form>
</dialog>