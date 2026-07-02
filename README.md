# js-loyihalar-2

Mana, barcha talablarga to'liq javob beradigan, zamonaviy va funksional Todo App (Vazifalar menejeri) ilovasi. Kodlar bitta HTML faylga jamlangan, uni osongina ishga tushirishingiz mumkin.

HTML, CSS va JavaScript kodi
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo App — Intizom</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: #f0f4f8;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .todo-container {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
            width: 100%;
            max-width: 480px;
        }

        h2 {
            color: #1e293b;
            margin-bottom: 20px;
            text-align: center;
        }

        /* Input va Qo'shish tugmasi */
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .input-group input {
            flex: 1;
            padding: 12px 16px;
            border: 2px solid #e2e8f0;
            border-radius: 8px;
            font-size: 1rem;
            outline: none;
            transition: border-color 0.2s;
        }

        .input-group input:focus {
            border-color: #3b82f6;
        }

        .add-btn {
            padding: 12px 20px;
            background: #3b82f6;
            color: white;
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
        }

        .add-btn:hover {
            background: #2563eb;
        }

        /* Filtr tugmalari */
        .filter-group {
            display: flex;
            gap: 8px;
            margin-bottom: 20px;
        }

        .filter-btn {
            flex: 1;
            padding: 8px;
            border: 1px solid #e2e8f0;
            background: #f8fafc;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9rem;
            color: #64748b;
            font-weight: 500;
            transition: all 0.2s;
        }

        .filter-btn.active {
            background: #3b82f6;
            color: white;
            border-color: #3b82f6;
        }

        /* Vazifalar ro'yxati */
        .todo-list {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .todo-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 12px 16px;
            background: #f8fafc;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
            transition: all 0.2s;
        }

        .todo-item.completed {
            background: #f1f5f9;
            opacity: 0.7;
        }

        .todo-left {
            display: flex;
            align-items: center;
            gap: 12px;
            flex: 1;
        }

        .todo-text {
            font-size: 1rem;
            color: #334155;
            word-break: break-word;
        }

        .todo-item.completed .todo-text {
            text-decoration: line-through;
            color: #94a3b8;
        }

        .delete-btn {
            background: transparent;
            border: none;
            color: #ef4444;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.1rem;
            padding: 4px 8px;
            border-radius: 4px;
            transition: background 0.2s;
        }

        .delete-btn:hover {
            background: #fee2e2;
        }

        /* Motivatsion xabar */
        .empty-message {
            text-align: center;
            padding: 30px 10px;
            color: #64748b;
            font-style: italic;
            font-size: 0.95rem;
            line-height: 1.5;
        }
    </style>
</head>
<body>

    <div class="todo-container">
        <h2>Mening Vazifalarim</h2>

        <div class="input-group">
            <input type="text" id="todoInput" placeholder="Yangi vazifa kiriting...">
            <button class="add-btn" id="addBtn">Qo'shish</button>
        </div>

        <div class="filter-group">
            <button class="filter-btn active" data-filter="all">Barchasi</button>
            <button class="filter-btn" data-filter="pending">Kutilayotgan</button>
            <button class="filter-btn" data-filter="completed">Bajarilgan</button>
        </div>

        <ul class="todo-list" id="todoList"></ul>
    </div>

    <script>
        // DOM Elementlari
        const todoInput = document.getElementById('todoInput');
        const addBtn = document.getElementById('addBtn');
        const todoList = document.getElementById('todoList');
        const filterButtons = document.querySelectorAll('.filter-btn');

        // LocalStorage'dan ma'lumotlarni yuklash yoki bo'sh massiv olish
        let todos = JSON.parse(localStorage.getItem('todos')) || [];
        let currentFilter = 'all';

        // Motivatsion xabarlar ro'yxati
        const motivations = [
            "✨ Hech qanday vazifa yo'q! Katta ishlarni boshlashning ayni vaqti.",
            "🚀 Rejalar ro'yxati bo'sh. Kuningizni samarali rejalashtiring!",
            "🎯 Hammasi bajarildi! O'zingizga dam bering yoki yangi marralarni ko'zlang.",
            "🌅 Toza ro'yxat — toza zehn. Yangi vazifalar qo'shishga tayyormisiz?"
        ];

        // Tasodifiy motivatsiya tanlash
        function getRandomMotivation() {
            return motivations[Math.floor(Math.random() * motivations.length)];
        }

        // Vazifalarni ekranga render qilish
        function renderTodos() {
            todoList.innerHTML = '';

            // Filtrlash mantiqi
            const filteredTodos = todos.filter(todo => {
                if (currentFilter === 'pending') return !todo.completed;
                if (currentFilter === 'completed') return todo.completed;
                return true; // 'all' bo'lsa hammasi
            });

            // Agar ro'yxat bo'sh bo'lsa motivatsion xabar ko'rsatish
            if (filteredTodos.length === 0) {
                const emptyDiv = document.createElement('div');
                emptyDiv.className = 'empty-message';
                emptyDiv.textContent = getRandomMotivation();
                todoList.appendChild(emptyDiv);
                return;
            }

            // Elementlarni yaratish
            filteredTodos.forEach(todo => {
                const li = document.createElement('li');
                li.className = `todo-item ${todo.completed ? 'completed' : ''}`;

                li.innerHTML = `
                    <div class="todo-left">
                        <input type="checkbox" ${todo.completed ? 'checked' : ''} onchange="toggleTodo(${todo.id})">
                        <span class="todo-text">${todo.text}</span>
                    </div>
                    <button class="delete-btn" onclick="deleteTodo(${todo.id})">×</button>
                `;

                todoList.appendChild(li);
            });
        }

        // Yangi vazifa qo'shish funksiyasi
        function addTodo() {
            const text = todoInput.value.trim();
            
            // Validatsiya (Bo'sh bo'lmasligi kerak)
            if (text === '') return;

            const newTodo = {
                id: Date.now(), // Noyob ID
                text: text,
                completed: false
            };

            todos.push(newTodo);
            saveToStorage();
            renderTodos();
            todoInput.value = ''; // Inputni tozalash
        }

        // Bajarilgan/Bajarilmagan holatga o'tkazish
        window.toggleTodo = function(id) {
            todos = todos.map(todo => {
                if (todo.id === id) {
                    return { ...todo, completed: !todo.completed };
                }
                return todo;
            });
            saveToStorage();
            renderTodos();
        };

        // Vazifani o'chirish
        window.deleteTodo = function(id) {
            todos = todos.filter(todo => todo.id !== id);
            saveToStorage();
            renderTodos();
        };

        // Ma'lumotni saqlash
        function saveToStorage() {
            localStorage.setItem('todos', JSON.stringify(todos));
        }

        // Hodisalarni bog'lash (Tugma va Enter)
        addBtn.addEventListener('click', addTodo);
        todoInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter') addTodo();
        });

        // Filtr tugmalari bosilishi
        filterButtons.forEach(btn => {
            btn.addEventListener('click', (e) => {
                filterButtons.forEach(b => b.classList.remove('active'));
                e.target.classList.add('active');
                
                currentFilter = e.target.getAttribute('data-filter');
                renderTodos();
            });
        });

        // Ilovani ilk bor yuklash
        renderTodos();
    </script>
</body>
</html>
Talablar qanday bajarildi?
Vazifa qo'shish: Qo'shish tugmasi bosilganda yoki input ichida Enter klaviaturasi bosilganda xavfsiz tarzda (bo'sh joylar trim() orqali tozalanib) vazifa ro'yxatga qo'shiladi.

Checkbox orqali bajarilgan deb belgilash: Checkbox holati o'zgarganda (onchange) toggleTodo ishga tushadi, obyekt ichidagi completed qiymati teskarisiga o'zgaradi va matn chiziladi.

O'chirish: Har bir element yonidagi qizil × tugmasi deleteTodo funksiyasini chaqirib, massivdan o'sha idli elementni butunlay o'chiradi.

Filtrlash: Uchta rejim (Barchasi / Bajarilgan / Kutilayotgan) massivning .filter() metodi yordamida real vaqtda saralanadi.

localStorage: Barcha amallardan keyin (qo'shish, o'chirish, o'zgartirish) saveToStorage() funksiyasi ishlab, ma'lumotlarni brauzer xotirasiga yozib qo'yadi. Sahifa yangilansa ham ma'lumot yo'qolmaydi.

Motivatsion xabar: Agar joriy filtr bo'yicha yoki umuman ro'yxatda vazifa qolmagan bo'lsa, motivations massividan tasodifiy (Math.random()) chiroyli va ruhlantiruvchi gap chiqib turadi.
