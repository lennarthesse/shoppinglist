<script>
    import { onMount } from "svelte";
    import Cross from "$lib/cross.svelte";

    let items = []
    let categories = [
        {
            name: "Obst",
            active: false
        },
        {
            name: "Gemüse",
            active: false
        },
        {
            name: "Hülsenfrüchte",
            active: false
        },
        {
            name: "Backwaren",
            active: false
        },
        {
            name: "Aufschnitt",
            active: false
        },
        {
            name: "Aufstrich",
            active: false
        },
        {
            name: "Vegetarisch / Vegan",
            active: false
        },
        {
            name: "Milchprodukte",
            active: false
        },
        {
            name: "Fisch & Fleisch",
            active: false
        },
        {
            name: "Gewürze",
            active: false
        },
        {
            name: "Trockenlebensmittel",
            active: false
        },
        {
            name: "Konserven",
            active: false
        },
        {
            name: "Tiefkühlwaren",
            active: false
        },
        {
            name: "Süßwaren",
            active: false
        },
        {
            name: "Getränke",
            active: false
        },
        {
            name: "Sonstige",
            active: false
        }
    ]
    let products = [];

    let currentCategory = null;

    let editProductModal;
    let currentProduct;

    let addProductModal;
    let modalAddProductMore = false;

    
	onMount(() => {

        const existingProducts = localStorage.getItem("products");
        products = JSON.parse(existingProducts) || [];

        // Create references to modals
        editProductModal = document.getElementById('edit-product-modal');
		addProductModal = document.getElementById('add-product-modal');
	})

    function openAddProductModal() {
        // Clear the form since submit doesn't clear it
        let form = document.getElementById("add-product-form");
        form.reset();
        // Initialize optional values as hidden
        modalAddProductMore = false;
        // Open the modal
        addProductModal.showModal();
    }
    
    function addProduct() {
        //  Fetch the data from the inputs using IDs
        let productName = document.getElementById("product-name").value;
        let productBrand = document.getElementById("product-brand") ? document.getElementById("product-brand").value == "" ? null : document.getElementById("product-brand").value : null;
        let productPrice = document.getElementById("product-price") ? document.getElementById("product-price").value == "" ? null : document.getElementById("product-price").value : null;
        let productCategory = document.getElementById("product-category") ? document.getElementById("product-category").value == "" ? null : document.getElementById("product-category").value : null;
        // Create a product object
        let product = {
            name: productName,
            brand: productBrand,
            price: productPrice,
            category: productCategory
        }
        // Add the product to the list of products
        products = [...products, product];

        localStorage.setItem("products", JSON.stringify(products));
    }
    
    function editProduct(product) {
        // Store the chosen product as a variable
        currentProduct = product;
        // Show the modal
        editProductModal.showModal();
    }

    function deleteProduct() {
        if (confirm("Soll das Produkt wirklich gelöscht werden?")) {
            for (let i = 0; i < products.length; i++) {
                if (products[i] == currentProduct) {
                    products.splice(i, 1);
                    products = [...products];
                    localStorage.setItem("products", JSON.stringify(products));
                    break;
                }
            }
            editProductModal.close();
            currentProduct = null;
        }
    }

    function saveProduct() {
        //  Fetch the data from the inputs using IDs
        let productName = document.getElementById("edit-product-name").value;
        let productBrand = document.getElementById("edit-product-brand") ? document.getElementById("edit-product-brand").value == "" ? null : document.getElementById("edit-product-brand").value : null;
        let productPrice = document.getElementById("edit-product-price") ? document.getElementById("edit-product-price").value == "" ? null : document.getElementById("edit-product-price").value : null;
        let productCategory = document.getElementById("edit-product-category") ? document.getElementById("edit-product-category").value == "Auswählen..." ? null : document.getElementById("edit-product-category").value : null;
        // Create a new product object
        let product = {
            name: productName,
            brand: productBrand,
            price: productPrice,
            category: productCategory
        }
        // Find the the chosen product (currentProduct) and replace it with the new one (product)
        for (let i = 0; i < products.length; i++) {
            if (products[i] == currentProduct) {
                products[i] = product;
                localStorage.setItem("products", JSON.stringify(products));
            }    
        }
        
    }

    function addProductToList(product) {
        // Create item with product name and start off unchecked
        let item = {
            name: product.name,
            checked: false
        }
        // Add item to list of items
        items = [...items, item];
    }
    
    function removeItemFromList(item) {
        // Find the first exact match (considering the checked state) and remove it from the list
        for (let i = 0; i < items.length; i++) {
            if (items[i] == item) {
                items.splice(i, 1);
                items = [...items];
                break;
            }
        }
    }

    function filterCategory(category) {
        for (let i = 0; i < categories.length; i++) {
            if (categories[i] == category) {
                if (categories[i].active) {
                    categories[i].active = false;
                    currentCategory = null;
                } else {
                    categories[i].active = true;
                    currentCategory = category.name;
                }
            } else {
                categories[i].active = false;
            }
        }
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
        <button class={category.active ? "active category" : "category"} on:click={() => {filterCategory(category)}}>
            {category.name}
        </button>
    {/each}
    <!--<button class="add category">+</button>-->
</div>

<section>
    <div class="products">
        <ul>
            {#each products as product}
                <!-- Show product if its category matches the current category, no category is selected, or category is sonstige and product has no category -->
                {#if product.category == currentCategory || currentCategory == null || (currentCategory == "Sonstige" && product.category == null)}
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
                {/if}
            {/each}
        </ul>
        <dialog id="edit-product-modal" class="modal">
            <h1>Produkt bearbeiten</h1>
            {#if currentProduct}
                <form method="dialog" id="edit-product-form" on:submit={saveProduct}>
                    <div class="input-text">
                        <label for="edit-product-name">Produktname</label>
                        <input type="text" id="edit-product-name" value={currentProduct.name} required>
                    </div>

                    <div class="input-text">
                        <label for="edit-product-brand">Marke</label>
                        <input type="text" id="edit-product-brand" value={currentProduct.brand == null ? "" : currentProduct.brand}>
                    </div>

                    <div class="input-text">
                        <label for="edit-product-price">Preis (in Euro)</label>
                        <input type="number" step="0.01" id="edit-product-price" value={currentProduct.price == null ? null : currentProduct.price}>
                    </div>

                    <div class="input-text">
                        <label for="edit-product-category">Kategorie</label>
                        <select id="edit-product-category">
                            <option>{currentProduct.category == null ? "Auswählen..." : currentProduct.category}</option>
                            {#each categories as category}
                                {#if category.name != currentProduct.category}
                                    <option>{category.name}</option>
                                {/if}
                            {/each}
                        </select>
                    </div>

                    <button class="button btn-delete" type="button" on:click={deleteProduct}>Produkt löschen</button>

                    <div class="buttons">
                        <button class="button btn-primary-light" type="button" on:click={() => {editProductModal.close(); currentProduct = null;}}>Abbrechen</button>
                        <button class="button btn-primary" type="submit">Speichern</button>
                    </div>
                </form>
            {/if}
        </dialog>
    </div>
</section>

<button class="add-product" on:click={openAddProductModal}>
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
            <button class="button btn-primary-light" type="button" on:click={() => {modalAddProductMore = true;}}>Mehr Optionen...</button>
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
                        <option>{category.name}</option>
                    {/each}
                </select>
            </div>
            <button class="button btn-primary-light" type="button" on:click={() => {modalAddProductMore = false;}}>Weniger Optionen...</button>
        {/if}

        <div class="buttons">
            <button class="button btn-primary-light" type="reset" on:click={() => {addProductModal.close();}}>Abbrechen</button>
            <button class="button btn-primary" type="submit">Hinzufügen</button>
        </div>
    </form>
</dialog>