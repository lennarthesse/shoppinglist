<script>
    let items = ["Item 1", "Item 2", "Item 3", "Item 4"]
    let categories = ["Gemüse", "Category 2", "Category 3", "Category 4", "Category 5", "Category 6"]
    let products = ["Product 1", "Product 2", "Product 3", "Product 4", "Product 5", "Product 6", "Product 7", "Product 8", "Product 9"];

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
                        ✕
                    </button>
                </li>
            {/each}
        </ul>
    </div>

    <div class="categories">
        {#each categories as category}
            <button class="category" on:click={filterCategory}>
                {category}
            </button>
        {/each}
    </div>

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

<button class="add-product">
    <svg viewBox="0 0 100 100"
        stroke="#ffffff"
        stroke-width="10">
        <line x1="50" y1="0" x2="50" y2="100" />
        <line x1="0" y1="50" x2="100" y2="50" />
    </svg>
</button>