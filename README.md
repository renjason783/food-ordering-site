# food-ordering-site
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>阿明小吃線上點餐</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css">
  <style>
    .menu-item img {
      width: 100%;
      height: 180px;
      object-fit: cover;
    }
  </style>
</head>
<body class="bg-light">
  <div class="container py-4">
    <h1 class="text-center mb-4">阿明小吃 線上點餐</h1>
    
    <div class="row" id="menu">
      <!-- 菜單項目會由 JS 產生 -->
    </div>

    <h3 class="mt-5">🛒 購物車</h3>
    <ul id="cart" class="list-group mb-3"></ul>
    <h5>總金額：<span id="total">0</span> 元</h5>

    <h4 class="mt-4">📋 填寫訂單</h4>
    <form id="order-form">
      <div class="mb-2">
        <label class="form-label">姓名</label>
        <input type="text" class="form-control" id="name" required>
      </div>
      <div class="mb-2">
        <label class="form-label">電話</label>
        <input type="tel" class="form-control" id="phone" required>
      </div>
      <div class="mb-2">
        <label class="form-label">取餐方式</label>
        <select class="form-select" id="method">
          <option value="外帶">外帶</option>
          <option value="內用">內用</option>
        </select>
      </div>
      <button class="btn btn-primary" type="submit">送出訂單</button>
    </form>

    <div class="alert alert-success mt-3 d-none" id="success-msg">
      ✅ 訂單已送出，請耐心等候！
    </div>
  </div>

  <script>
    const menu = [
      { name: "雞皮", price: 40, img: "https://i.imgur.com/3aXhX6e.jpg" },
      { name: "鴨頭", price: 50, img: "https://i.imgur.com/fTjO4Ol.jpg" },
      { name: "樓梯", price: 35, img: "https://i.imgur.com/HJg7TmT.jpg" },
    ];

    const menuContainer = document.getElementById("menu");
    const cart = [];
    
    function updateCart() {
      const cartList = document.getElementById("cart");
      const total = document.getElementById("total");
      cartList.innerHTML = "";
      let sum = 0;

      cart.forEach((item, idx) => {
        sum += item.price;
        const li = document.createElement("li");
        li.className = "list-group-item d-flex justify-content-between";
        li.innerHTML = `${item.name} - ${item.price}元
          <button class="btn btn-sm btn-danger" onclick="removeFromCart(${idx})">移除</button>`;
        cartList.appendChild(li);
      });
      total.textContent = sum;
    }

    function addToCart(item) {
      cart.push(item);
      updateCart();
    }

    function removeFromCart(index) {
      cart.splice(index, 1);
      updateCart();
    }

    menu.forEach(item => {
      const col = document.createElement("div");
      col.className = "col-md-4 mb-4";
      col.innerHTML = `
        <div class="card menu-item">
          <img src="${item.img}" class="card-img-top" alt="${item.name}">
          <div class="card-body">
            <h5 class="card-title">${item.name}</h5>
            <p class="card-text">${item.price} 元</p>
            <button class="btn btn-success" onclick='addToCart(${JSON.stringify(item)})'>加入購物車</button>
          </div>
        </div>
      `;
      menuContainer.appendChild(col);
    });

    document.getElementById("order-form").addEventListener("submit", function(e) {
      e.preventDefault();
      if (cart.length === 0) return alert("請先選擇餐點！");
      document.getElementById("success-msg").classList.remove("d-none");
      // 這裡可以改為發送到 Google 表單、LINE Notify、Email API 等
      console.log("訂單資訊", {
        name: document.getElementById("name").value,
        phone: document.getElementById("phone").value,
        method: document.getElementById("method").value,
        items: cart
      });
    });
  </script>
</body>
</html>
