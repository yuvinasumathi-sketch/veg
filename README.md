<!DOCTYPE html>
<html>
<head>
  <title>Local Farmer Connect</title>
  <style>
    body { font-family: Arial; margin:0; background:#f0fff0; }
    header { background:green; color:white; padding:15px; text-align:center; }
    .container { padding:20px; text-align:center; }
    .card { border:1px solid #ccc; margin:10px; padding:10px; display:inline-block; width:200px; }
    button { padding:5px 10px; margin-top:5px; cursor:pointer; }
    .hidden { display:none; }
  </style>
</head>

<body>

<header>
  <h1>🌿 Local Farmer Connect</h1>
</header>

<div class="container">

  <!-- Welcome Page -->
  <div id="welcome">
    <h2>Fresh from Farm to Your Home</h2>
    <p>🍎 Fruits | 🥕 Vegetables | 🌿 Greens</p>
    <button onclick="showPage('home')">Explore Products</button>
  </div>

  <!-- Home Page -->
  <div id="home" class="hidden">
    <h2>Products</h2>
    <div id="products"></div>
    <button onclick="showPage('cart')">Go to Cart</button>
  </div>

  <!-- Cart Page -->
  <div id="cart" class="hidden">
    <h2>🛒 Cart</h2>
    <div id="cartItems"></div>
    <h3 id="total"></h3>
    <button onclick="showPage('checkout')">Checkout</button>
  </div>

  <!-- Checkout -->
  <div id="checkout" class="hidden">
    <h2>Payment</h2>
    <p>Select Payment Method:</p>
    <button onclick="placeOrder()">UPI</button>
    <button onclick="placeOrder()">Card</button>
    <button onclick="placeOrder()">Cash on Delivery</button>
  </div>

  <!-- Order Confirmation -->
  <div id="order" class="hidden">
    <h2>✅ Order Placed!</h2>
    <p id="orderDetails"></p>
    <button onclick="showPage('tracking')">Track Order</button>
  </div>

  <!-- Tracking -->
  <div id="tracking" class="hidden">
    <h2>🚚 Delivery Status</h2>
    <p id="status">Processing...</p>
  </div>

</div>

<script>
let products = [
  {id:1, name:"Tomato", price:30},
  {id:2, name:"Carrot", price:50},
  {id:3, name:"Spinach", price:20}
];

let cart = [];

function showPage(page) {
  document.querySelectorAll('.container > div').forEach(div => div.classList.add('hidden'));
  document.getElementById(page).classList.remove('hidden');
}

function loadProducts() {
  let html = "";
  products.forEach(p => {
    html += `
      <div class="card">
        <h3>${p.name}</h3>
        <p>₹${p.price}</p>
        <button onclick='addToCart(${JSON.stringify(p)})'>Add to Cart</button>
      </div>
    `;
  });
  document.getElementById("products").innerHTML = html;
}

function addToCart(p) {
  cart.push(p);
  alert(p.name + " added to cart");
}

function loadCart() {
  let html = "";
  let total = 0;

  cart.forEach(p => {
    html += `<p>${p.name} - ₹${p.price}</p>`;
    total += p.price;
  });

  let discount = total * 0.1;
  let final = total - discount;

  document.getElementById("cartItems").innerHTML = html;
  document.getElementById("total").innerHTML =
    `Total: ₹${total} <br> Discount: ₹${discount} <br> Final: ₹${final}`;
}

function placeOrder() {
  document.getElementById("orderDetails").innerText =
    "Items: " + cart.length + " | Total Paid";

  cart = [];
  showPage('order');

  setTimeout(() => {
    document.getElementById("status").innerText = "Out for Delivery 🚚";
  }, 3000);
}

// Auto load
loadProducts();

// Update cart when opening cart page
