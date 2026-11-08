# Campus-cart
   
   senior sell
         juniors save!

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Campus Cart</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: #f4f6fb;
    color: #222;
}

header {
    background: #142d4e;
    color: white;
    padding: 20px 7%;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

header h1 {
    color: #ffcc45;
}

nav a {
    color: white;
    text-decoration: none;
    margin-left: 20px;
}

.hero {
    background: linear-gradient(120deg, #142d4e, #2675a9);
    color: white;
    text-align: center;
    padding: 65px 15px;
}

.hero h2 {
    font-size: 36px;
    margin-bottom: 15px;
}

.hero p {
    margin-bottom: 20px;
}

.search {
    padding: 12px;
    width: 65%;
    max-width: 450px;
    border: none;
    border-radius: 6px;
}

section {
    padding: 35px 7%;
}

h2 {
    margin-bottom: 20px;
}

.categories {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
    margin-bottom: 25px;
}

.categories button {
    padding: 10px 18px;
    border: none;
    background: #dce9f5;
    border-radius: 20px;
    cursor: pointer;
}

.categories button:hover {
    background: #2675a9;
    color: white;
}

.products {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
    gap: 22px;
}

.product {
    background: white;
    padding: 18px;
    border-radius: 12px;
    box-shadow: 0 3px 12px #00000012;
    text-align: center;
}

.product .emoji {
    font-size: 65px;
    padding: 15px;
    background: #edf3f9;
    border-radius: 10px;
}

.product h3 {
    margin: 12px 0;
}

.product p {
    margin: 8px 0;
    color: #666;
}

.price {
    color: #16834b !important;
    font-size: 21px;
    font-weight: bold;
}

.product button, .sell button {
    background: #1769aa;
    color: white;
    border: none;
    padding: 11px 18px;
    border-radius: 6px;
    cursor: pointer;
    margin-top: 10px;
}

.product button:hover, .sell button:hover {
    background: #104a78;
}

.sell {
    background: white;
    border-radius: 12px;
    padding: 25px;
}

.sell input, .sell select {
    width: 100%;
    padding: 12px;
    margin: 8px 0;
    border: 1px solid #ccc;
    border-radius: 6px;
}

.cart {
    background: #e4edf7;
    padding: 20px;
    border-radius: 10px;
}

footer {
    background: #142d4e;
    color: white;
    text-align: center;
    padding: 22px;
    margin-top: 30px;
}

@media(max-width:600px) {
    header {
        flex-direction: column;
        gap: 15px;
    }

    nav a {
        margin: 5px;
    }

    .hero h2 {
        font-size: 27px;
    }
}
</style>
</head>

<body>

<header>
    <h1>🛒 Campus Cart</h1>
    <nav>
        <a href="#home">Home</a>
        <a href="#products">Products</a>
        <a href="#sell">Sell</a>
        <a href="#cart">Cart (<span id="cartCount">0</span>)</a>
    </nav>
</header>

<div class="hero" id="home">
    <h2>Seniors Sell, Juniors Save!</h2>
    <p>Your Campus, Your Marketplace.</p>

    <input class="search" type="text"
    id="search" placeholder="Search books, notes, calculators..."
    oninput="displayProducts()">
</div>

<section id="products">
    <h2>Explore Products</h2>

    <div class="categories">
        <button onclick="setCategory('All')">All</button>
        <button onclick="setCategory('Books')">Books</button>
        <button onclick="setCategory('Notes')">Notes</button>
        <button onclick="setCategory('Calculators')">Calculators</button>
        <button onclick="setCategory('Stationery')">Stationery</button>
    </div>

    <div class="products" id="productList"></div>
</section>

<section id="sell">
    <h2>Sell Your Product</h2>

    <div class="sell">
        <input type="text" id="name"
        placeholder="Product Name">

        <input type="number" id="price"
        placeholder="Price in ₹">

        <select id="category">
            <option value="Books">Books</option>
            <option value="Notes">Notes</option>
            <option value="Calculators">Calculators</option>
            <option value="Stationery">Stationery</option>
        </select>

        <input type="text" id="seller"
        placeholder="Seller Name">

        <button onclick="addProduct()">List Product</button>
    </div>
</section>

<section id="cart">
    <h2>🛒 Your Shopping Cart</h2>

    <div class="cart">
        <div id="cartItems">Your cart is empty.</div>
        <h3 id="total">Total: ₹0</h3>
    </div>
</section>

<footer>
    <h3>Campus Cart</h3>
    <p>Seniors Sell, Juniors Save!</p>
    <p>© 2026 Campus Cart. All Rights Reserved.</p>
</footer>

<script>

let products = [
    {
        name: "Engineering Mathematics",
        price: 250,
        category: "Books",
        seller: "Rahul",
        emoji: "📚"
    },
    {
        name: "C Programming Notes",
        price: 100,
        category: "Notes",
        seller: "Amit",
        emoji: "📝"
    },
    {
        name: "Scientific Calculator",
        price: 450,
        category: "Calculators",
        seller: "Sneha",
        emoji: "🧮"
    },
    {
        name: "Engineering Drawing Kit",
        price: 180,
        category: "Stationery",
        seller: "Akash",
        emoji: "📐"
    }
];

let cart = [];
let selectedCategory = "All";

function displayProducts() {

    let list = document.getElementById("productList");
    let search = document.getElementById("search")
        .value.toLowerCase();

    list.innerHTML = "";

    let filtered = products.filter(p => {
        return (selectedCategory === "All" ||
                p.category === selectedCategory) &&
                p.name.toLowerCase().includes(search);
    });

    if(filtered.length === 0) {
        list.innerHTML = "<p>No products found.</p>";
        return;
    }

    filtered.forEach((p, index) => {

        let actualIndex = products.indexOf(p);

        list.innerHTML += `
        <div class="product">
            <div class="emoji">${p.emoji || "📦"}</div>
            <h3>${p.name}</h3>
            <p>Category: ${p.category}</p>
            <p>Seller: ${p.seller}</p>
            <p class="price">₹${p.price}</p>
            <button onclick="addToCart(${actualIndex})">
                Add to Cart
            </button>
        </div>`;
    });
}

function setCategory(category) {
    selectedCategory = category;
    displayProducts();
}

function addProduct() {

    let name = document.getElementById("name").value.trim();
    let price = Number(document.getElementById("price").value);
    let category = document.getElementById("category").value;
    let seller = document.getElementById("seller").value.trim();

    if(!name || !seller || !price || price < 1) {
        alert("Please enter valid product details!");
        return;
    }

    products.push({
        name: name,
        price: price,
        category: category,
        seller: seller,
        emoji: "📦"
    });

    alert("Product listed successfully!");

    document.getElementById("name").value = "";
    document.getElementById("price").value = "";
    document.getElementById("seller").value = "";

    displayProducts();
}

function addToCart(index) {
    cart.push(products[index]);
    updateCart();
}

function updateCart() {

    let items = document.getElementById("cartItems");
    let total = 0;

    items.innerHTML = "";

    if(cart.length === 0) {
        items.innerHTML = "Your cart is empty.";
    }

    cart.forEach((p, index) => {
        total += p.price;

        items.innerHTML += `
            <p>
            ${p.name} - ₹${p.price}
            <button onclick="removeItem(${index})">
                Remove
            </button>
            </p><br>`;
    });

    document.getElementById("total").innerText =
        "Total: ₹" + total;

    document.getElementById("cartCount").innerText =
        cart.length;
}

function removeItem(index) {
    cart.splice(index, 1);
    updateCart();
}

displayProducts();

</script>

</body>
</html>

   
