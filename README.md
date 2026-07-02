# js-loyihalar-2
Mana, siz aytgan barcha shartlarga javob beradigan — pitssalarni tanlash, savatchada miqdorni o'zgartirish, o'chirish, localStorage bilan ishlash va buyurtmani tasdiqlash modal oynasiga ega to'liq HTML, CSS va JavaScript kodi.

Buni bitta faylga (masalan, pizza.html) saqlab, brauzerda ishlatishingiz mumkin:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pizza Buyurtma Tizimi</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f8f9fa;
            color: #333;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 30px;
        }

        @media (max-width: 900px) {
            .container {
                grid-template-columns: 1fr;
            }
        }

        h1, h2 {
            margin-bottom: 20px;
            color: #2c3e50;
        }

        .header-title {
            grid-column: 1 / -1;
            text-align: center;
            padding: 10px 0;
            color: #e74c3c;
        }

        /* Pizza Menyusi */
        .menu-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
        }

        .pizza-card {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            border: 1px solid #eee;
        }

        .pizza-emoji {
            font-size: 50px;
            margin-bottom: 10px;
        }

        .pizza-name {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 8px;
        }

        .pizza-price {
            color: #e74c3c;
            font-weight: bold;
            font-size: 16px;
            margin-bottom: 15px;
        }

        button {
            padding: 10px 15px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s;
        }

        .btn-add {
            background-color: #ff9f43;
            color: white;
            width: 100%;
        }

        .btn-add:hover { background-color: #ee5253; }

        /* Savatcha Bo'limi */
        .cart-box {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.08);
            height: fit-content;
            position: sticky;
            top: 20px;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }

        .cart-item-info h4 {
            font-size: 15px;
            margin-bottom: 4px;
        }

        .cart-item-info p {
            color: #7f8c8d;
            font-size: 13px;
        }

        .quantity-controls {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn-qty {
            background-color: #eee;
            padding: 5px 10px;
            font-size: 14px;
        }

        .btn-qty:hover { background-color: #ddd; }

        .btn-del {
            background-color: #ff6b6b;
            color: white;
            padding: 5px 8px;
            font-size: 12px;
        }

        .btn-del:hover { background-color: #ee5253; }

        .cart-total {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
        }

        .btn-order {
            background-color: #10ac84;
            color: white;
            width: 100%;
            margin-top: 20px;
            padding: 12px;
            font-size: 16px;
        }

        .btn-order:hover { background-color: #0bd38a; }
        .btn-order:disabled { background-color: #ccc; cursor: not-allowed; }

        .empty-cart-msg {
            color: #7f8c8d;
            text-align: center;
            padding: 20px 0;
            font-style: italic;
        }

        /* Modal (Tasdiqlash oynasi) */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.6);
            display: flex;
            justify-content: center;
            align-items: center;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
            z-index: 1000;
        }

        .modal-overlay.active {
            opacity: 1;
            pointer-events: auto;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 12px;
            max-width: 400px;
            width: 90%;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            transform: scale(0.8);
            transition: transform 0.3s ease;
        }

        .modal-overlay.active .modal-content {
            transform: scale(1);
        }

        .modal-content h3 {
            color: #10ac84;
            margin-bottom: 15px;
        }

        .modal-content p {
            margin-bottom: 20px;
            color: #555;
            line-height: 1.5;
        }

        .btn-close-modal {
            background-color: #2c3e50;
            color: white;
            width: 100%;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header-title">
        <h1>🍕 Pizzeria Interaktiv Menyusi</h1>
    </div>

    <div>
        <h2>Menyu</h2>
        <div class="menu-grid" id="menuGrid">
            </div>
    </div>

    <div class="cart-box">
        <h2>Savatchangiz</h2>
        <div id="cartItemsContainer">
            </div>
        <div class="cart-total">
            <span>Jami summa:</span>
            <span id="cartTotalAmount">0 so'm</span>
        </div>
        <button class="btn-order" id="orderBtn" disabled>Buyurtma berish</button>
    </div>
</div>

<div class="modal-overlay" id="orderModal">
    <div class="modal-content">
        <h3>🎉 Buyurtma qabul qilindi!</h3>
        <p id="modalMessage">Sizning buyurtmangiz tayyorlanishga topshirildi.</p>
        <button class="btn-close-modal" id="closeModalBtn">Yopish</button>
    </div>
</div>

<script>
    // Pitssalar ro'yxati (Baza)
    const pizzaMenu = [
        { id: 1, name: "Margarita", price: 55000, emoji: "🧀" },
        { id: 2, name: "Pepperoni", price: 65000, emoji: "🍕" },
        { id: 3, name: "Tovuqli va Qo'ziqorinli", price: 70000, emoji: "🍗" },
        { id: 4, name: "To'rt fasl (4 Seasons)", price: 75000, emoji: "🍃" },
        { id: 5, name: "Go'shtli (Meat Feast)", price: 80000, emoji: "🥩" },
        { id: 6, name: "Meksikancha (Achchiq)", price: 68000, emoji: "🌶️" }
    ];

    // Savatchani localStorage'dan yuklash yoki yangi massiv ochish
    let cart = JSON.parse(localStorage.getItem('pizza_cart')) || [];

    const menuGrid = document.getElementById('menuGrid');
    const cartItemsContainer = document.getElementById('cartItemsContainer');
    const cartTotalAmount = document.getElementById('cartTotalAmount');
    const orderBtn = document.getElementById('orderBtn');
    const orderModal = document.getElementById('orderModal');
    const closeModalBtn = document.getElementById('closeModalBtn');
    const modalMessage = document.getElementById('modalMessage');

    // Menyuni ekranga chiqarish
    function renderMenu() {
        menuGrid.innerHTML = '';
        pizzaMenu.forEach(pizza => {
            const card = document.createElement('div');
            card.className = 'pizza-card';
            card.innerHTML = `
                <div class="pizza-emoji">${pizza.emoji}</div>
                <div class="pizza-name">${pizza.name}</div>
                <div class="pizza-price">${pizza.price.toLocaleString()} so'm</div>
                <button class="btn-add" onclick="addToCart(${pizza.id})">Savatchaga qo'shish</button>
            `;
            menuGrid.appendChild(card);
        });
    }

    // Savatchaga qo'shish
    function addToCart(pizzaId) {
        const foundPizza = pizzaMenu.find(p => p.id === pizzaId);
        const cartItem = cart.find(item => item.id === pizzaId);

        if (cartItem) {
            cartItem.quantity += 1;
        } else {
            cart.push({
                ...foundPizza,
                quantity: 1
            });
        }
        updateCart();
    }

    // Miqdorni o'zgartirish (+/-)
    function changeQuantity(pizzaId, action) {
        const cartItem = cart.find(item => item.id === pizzaId);
        if (!cartItem) return;

        if (action === 'increase') {
            cartItem.quantity += 1;
        } else if (action === 'decrease') {
            cartItem.quantity -= 1;
            if (cartItem.quantity === 0) {
                removeFromCart(pizzaId);
                return;
            }
        }
        updateCart();
    }

    // Mahsulotni o'chirish
    function removeFromCart(pizzaId) {
        cart = cart.filter(item => item.id !== pizzaId);
        updateCart();
    }

    // Savatchani yangilash (Hisob-kitob va render)
    function updateCart() {
        // LocalStorage'ga saqlash
        localStorage.setItem('pizza_cart', JSON.stringify(cart));
        
        cartItemsContainer.innerHTML = '';
        
        if (cart.length === 0) {
            cartItemsContainer.innerHTML = '<div class="empty-cart-msg">Savatchangiz hozircha bo\'sh</div>';
            cartTotalAmount.innerText = "0 so'm";
            orderBtn.disabled = true;
            return;
        }

        let totalSum = 0;

        cart.forEach(item => {
            const itemTotal = item.price * item.quantity;
            totalSum += itemTotal;

            const div = document.createElement('div');
            div.className = 'cart-item';
            div.innerHTML = `
                <div class="cart-item-info">
                    <h4>${item.name}</h4>
                    <p>${item.price.toLocaleString()} x ${item.quantity} = ${itemTotal.toLocaleString()} so'm</p>
                </div>
                <div class="quantity-controls">
                    <button class="btn-qty" onclick="changeQuantity(${item.id}, 'decrease')">-</button>
                    <span>${item.quantity}</span>
                    <button class="btn-qty" onclick="changeQuantity(${item.id}, 'increase')">+</button>
                    <button class="btn-del" onclick="removeFromCart(${item.id})">🗑️</button>
                </div>
            `;
            cartItemsContainer.appendChild(div);
        });

        cartTotalAmount.innerText = `${totalSum.toLocaleString()} so'm`;
        orderBtn.disabled = false;
    }

    // Buyurtma berish (Modal oynani ochish)
    orderBtn.addEventListener('click', () => {
        const total = cartTotalAmount.innerText;
        modalMessage.innerHTML = `Siz muvaffaqiyatli buyurtma berdingiz!<br><b>Jami summa: ${total}</b><br>Kuryer tez orada siz bilan bog'lanadi.`;
        orderModal.classList.add('active');
    });

    // Modalni yopish va savatchani tozalash
    closeModalBtn.addEventListener('click', () => {
        orderModal.classList.remove('active');
        cart = []; // Buyurtmadan so'ng savat tozalanadi
        updateCart();
    });

    // Dastlabki yuklash
    renderMenu();
    updateCart();
</script>

</body>
</html>
Kod qanday talablarni bajardi?
Menyu va savatchaga qo'shish: pizzaMenu obyekti asosida kartochkalar yaratildi. "Savatchaga qo'shish" tugmasi bosilganda mahsulot savatchaga o'tadi.
