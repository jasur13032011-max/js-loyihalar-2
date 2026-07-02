# js-loyihalar-2
Bu loyihani talabalar yoki xodimlar ro'yxatini boshqarish misolida ko'rib chiqamiz. Kodni bitta index.html fayliga saqlab brauzerda ishlatishingiz mumkin:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ma'lumotlar Bazasi (CRUD)</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Arial, sans-serif;
        }

        body {
            background-color: #f5f7fa;
            color: #333;
            padding: 30px 15px;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
        }

        h2 {
            text-align: center;
            margin-bottom: 25px;
            color: #2c3e50;
        }

        /* Forma dizayni */
        .form-container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            margin-bottom: 30px;
        }

        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            font-size: 14px;
        }

        .form-group input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 14px;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            font-size: 14px;
            transition: background 0.2s;
        }

        .btn-submit {
            background-color: #3498db;
            color: white;
            width: 100%;
        }

        .btn-submit:hover { background-color: #2980b9; }
        
        .btn-edit { background-color: #f1c40f; color: #333; margin-right: 5px; }
        .btn-edit:hover { background-color: #d4ac0d; }

        .btn-delete { background-color: #e74c3c; color: white; }
        .btn-delete:hover { background-color: #c0392b; }

        .btn-cancel { background-color: #95a5a6; color: white; width: 100%; margin-top: 5px;}
        .btn-cancel:hover { background-color: #7f8c8d; }

        /* Jadval dizayni */
        .table-container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        th, td {
            padding: 12px 15px;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #f8f9fa;
            color: #2c3e50;
        }

        tr:hover { background-color: #fdfdfd; }

        /* Bo'sh xabar */
        .empty-message {
            text-align: center;
            color: #7f8c8d;
            padding: 30px;
            font-style: italic;
        }

        .hidden { display: none; }
    </style>
</head>
<body>

<div class="container">
    <h2 id="formTitle">Yangi foydalanuvchi qo'shish</h2>
    
    <div class="form-container">
        <form id="userForm">
            <input type="hidden" id="userId">
            <div class="form-grid">
                <div class="form-group">
                    <label>Ism va Familiya</label>
                    <input type="text" id="fullName" required placeholder="Masalan: Ali Valiyev">
                </div>
                <div class="form-group">
                    <label>Kasbi / Lavozimi</label>
                    <input type="text" id="job" required placeholder="Masalan: Dasturchi">
                </div>
                <div class="form-group">
                    <label>Telefon raqami</label>
                    <input type="text" id="phone" required placeholder="+998 90 123 45 67">
                </div>
            </div>
            <button type="submit" class="btn-submit" id="submitBtn">Qo'shish</button>
            <button type="button" class="btn-cancel hidden" id="cancelBtn">Tahrirlashni bekor qilish</button>
        </form>
    </div>

    <h2>Ro'yxat</h2>
    
    <div class="table-container">
        <table id="userTable">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Ism va Familiya</th>
                    <th>Kasbi</th>
                    <th>Telefon</th>
                    <th>Amallar</th>
                </tr>
            </thead>
            <tbody id="tableBody">
                </tbody>
        </table>
        <div id="emptyMessage" class="empty-message">Hozircha hech qanday yozuv kiritilmagan.</div>
    </div>
</div>

<script>
    // LocalStorage'dan ma'lumotlarni o'qish
    let users = JSON.parse(localStorage.getItem('crud_users')) || [];
    let isEditing = false;

    const userForm = document.getElementById('userForm');
    const tableBody = document.getElementById('tableBody');
    const emptyMessage = document.getElementById('emptyMessage');
    const userTable = document.getElementById('userTable');
    
    const userIdInput = document.getElementById('userId');
    const fullNameInput = document.getElementById('fullName');
    const jobInput = document.getElementById('job');
    const phoneInput = document.getElementById('phone');
    
    const submitBtn = document.getElementById('submitBtn');
    const cancelBtn = document.getElementById('cancelBtn');
    const formTitle = document.getElementById('formTitle');

    // Jadvalni yangilash funksiyasi
    function renderTable() {
        tableBody.innerHTML = '';

        if (users.length === 0) {
            userTable.classList.add('hidden');
            emptyMessage.classList.remove('hidden');
            return;
        }

        userTable.classList.remove('hidden');
        emptyMessage.classList.add('hidden');

        users.forEach((user, index) => {
            const tr = document.createElement('tr');
            tr.innerHTML = `
                <td>${index + 1}</td>
                <td>${user.fullName}</td>
                <td>${user.job}</td>
                <td>${user.phone}</td>
                <td>
                    <button class="btn-edit" onclick="editUser('${user.id}')">Tahrirlash</button>
                    <button class="btn-delete" onclick="deleteUser('${user.id}')">O'chirish</button>
                </td>
            `;
            tableBody.appendChild(tr);
        });
    }

    // Ma'lumot qo'shish yoki tahrirlash (Submit)
    userForm.addEventListener('submit', (e) => {
        e.preventDefault();

        const userData = {
            fullName: fullNameInput.value.trim(),
            job: jobInput.value.trim(),
            phone: phoneInput.value.trim()
        };

        if (isEditing) {
            // Tahrirlash rejimi
            const id = userIdInput.value;
            users = users.map(user => user.id === id ? { ...user, ...userData } : user);
            resetForm();
        } else {
            // Yangi qo'shish rejimi
            userData.id = Date.now().toString(); // Noyob ID yaratish
            users.push(userData);
        }

        saveToLocalStorage();
        renderTable();
        userForm.reset();
    });

    // Tahrirlash tugmasi bosilganda (Ma'lumotni formaga yuklash)
    function editUser(id) {
        const user = users.find(u => u.id === id);
        if (!user) return;

        userIdInput.value = user.id;
        fullNameInput.value = user.fullName;
        jobInput.value = user.job;
        phoneInput.value = user.phone;

        isEditing = true;
        formTitle.innerText = "Yozuvni tahrirlash";
        submitBtn.innerText = "Saqlash";
        cancelBtn.classList.remove('hidden');
        window.scrollTo({ top: 0, behavior: 'smooth' }); // Sahifani yuqoriga chiqaradi
    }

    // O'chirish funksiyasi (Tasdiqlash so'raladi)
    function deleteUser(id) {
        const confirmDelete = confirm("Haqiqatan ham ushbu yozuvni o'chirib tashlamoqchimisiz?");
        if (confirmDelete) {
            users = users.filter(user => user.id !== id);
            
            // Agar tahrirlanayotgan yozuv o'chib ketsa, formani tozalaydi
            if (userIdInput.value === id) resetForm();
            
            saveToLocalStorage();
            renderTable();
        }
    }

    // Tahrirlashni bekor qilish
    cancelBtn.addEventListener('click', resetForm);

    function resetForm() {
        isEditing = false;
        userForm.reset();
        userIdInput.value = '';
        formTitle.innerText = "Yangi foydalanuvchi qo'shish";
        submitBtn.innerText = "Qo'shish";
        cancelBtn.classList.add('hidden');
    }

    // LocalStorage'ga yozish
    function saveToLocalStorage() {
        localStorage.setItem('crud_users', JSON.stringify(users));
    }

    // Dastur ishga tushganda jadvalni chizish
    renderTable();
</script>

</body>
</html>
Kod qanday ishlaydi va shartlar bajarilishi:
Yangi yozuv qo'shish: Forma to'ldirilib Qo'shish bosilganda users massiviga yangi obyekt qo'shiladi va jadval yangilanadi.

Jadvalda ko'rsatish: renderTable() funksiyasi orqali massivdagi barcha elementlar jadval ko'rinishida (<tr> va <td> lar orqali) generatsiya qilinadi.

Yozuvni tahrirlash: Tahrirlash tugmasi bosilganda, JavaScript ushbu element ma'lumotlarini formadagi inputlarga qayta yuklaydi. Formaning ko'rinishi o'zgaradi ("Saqlash" rejimiga o'tadi) hamda tahrirlashni bekor qilish imkoni paydo bo'ladi.

O'chirish (Tasdiqlash): O'chirish tugmasi bosilganda brauzerning ichki confirm() oynasi ochiladi. Foydalanuvchi "OK" bossagina o'chadi.

localStorage: Barcha amallardan keyin (qo'shish, o'chirish, tahrirlash) saveToLocalStorage() funksiyasi ishlaydi, shuning uchun brauzer yopilib ochilsa ham ma'lumot saqlanib qoladi.

Jadval bo'sh bo'lgandagi xabar: Agar massivda ma'lumot bo'lmasa, CSS orqali jadval yashiriladi va "Hozircha hech qanday yozuv kiritilmagan." degan maxsus blok ko'rinadi.
