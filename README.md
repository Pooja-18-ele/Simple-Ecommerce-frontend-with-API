# Simple-Ecommerce-frontend-with-API
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>E-commerce Frontend</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    button, input { margin: 5px; }
    .section { margin-bottom: 30px; }
    pre { background: #f0f0f0; padding: 10px; }
  </style>
</head>
<body>

  <h1>Simple E-commerce Frontend</h1>

  <!-- Login -->
  <div class="section">
    <h2>Login</h2>
    <input type="text" id="username" placeholder="Username">
    <input type="password" id="password" placeholder="Password">
    <button onclick="login()">Login</button>
  </div>

  <!-- Product List -->
  <div class="section">
    <h2>Products</h2>
    <button onclick="fetchProducts()">Load Products</button>
    <pre id="products"></pre>
  </div>

  <!-- Add to Cart -->
  <div class="section">
    <h2>Add to Cart</h2>
    <input type="text" id="productId" placeholder="Product ID">
    <input type="number" id="quantity" placeholder="Quantity">
    <button onclick="addToCart()">Add to Cart</button>
  </div>

  <!-- View Cart -->
  <div class="section">
    <h2>Cart</h2>
    <button onclick="viewCart()">View Cart</button>
    <pre id="cart"></pre>
  </div>

  <!-- Place Order -->
  <div class="section">
    <h2>Order</h2>
    <button onclick="placeOrder()">Place Order</button>
    <pre id="orderResponse"></pre>
  </div>

  <script>
    let token = '';

    async function login() {
      const res = await fetch('http://localhost:8080/api/auth/login', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({
          username: document.getElementById('username').value,
          password: document.getElementById('password').value
        })
      });
      const data = await res.json();
      if (data.token) {
        token = data.token;
        alert('Login successful!');
      } else {
        alert('Login failed');
      }
    }

    async function fetchProducts() {
      const res = await fetch('http://localhost:8080/api/products');
      const data = await res.json();
      document.getElementById('products').textContent = JSON.stringify(data, null, 2);
    }

    async function addToCart() {
      const res = await fetch('http://localhost:8080/api/cart/add', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer ' + token
        },
        body: JSON.stringify({
          productId: document.getElementById('productId').value,
          quantity: parseInt(document.getElementById('quantity').value)
        })
      });
      const data = await res.json();
      alert('Item added to cart');
    }

    async function viewCart() {
      const res = await fetch('http://localhost:8080/api/cart', {
        headers: { 'Authorization': 'Bearer ' + token }
      });
      const data = await res.json();
      document.getElementById('cart').textContent = JSON.stringify(data, null, 2);
    }

    async function placeOrder() {
      const res = await fetch('http://localhost:8080/api/orders', {
        method: 'POST',
        headers: { 'Authorization': 'Bearer ' + token }
      });
      const data = await res.json();
      document.getElementById('orderResponse').textContent = JSON.stringify(data, null, 2);
    }
  </script>

</body>
</html>
