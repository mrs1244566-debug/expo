<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة الإرساليات</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        .login-container, .dashboard {
            background: white;
            border-radius: 15px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            margin-top: 50px;
        }
        
        .hidden {
            display: none;
        }
        
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 30px;
            font-size: 2.5em;
        }
        
        h2 {
            color: #555;
            margin-bottom: 20px;
            border-bottom: 2px solid #667eea;
            padding-bottom: 10px;
        }
        
        .user-type-selector {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .user-type-btn {
            padding: 15px 30px;
            border: 2px solid #667eea;
            background: white;
            color: #667eea;
            border-radius: 10px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
        }
        
        .user-type-btn.active {
            background: #667eea;
            color: white;
        }
        
        .user-type-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: 600;
        }
        
        input, select {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            transition: border 0.3s;
        }
        
        input:focus, select:focus {
            border-color: #667eea;
            outline: none;
        }
        
        .btn {
            background: #667eea;
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            width: 100%;
            transition: background 0.3s;
        }
        
        .btn:hover {
            background: #5a67d8;
        }
        
        .btn-secondary {
            background: #48bb78;
        }
        
        .btn-secondary:hover {
            background: #38a169;
        }
        
        .btn-danger {
            background: #f56565;
        }
        
        .btn-danger:hover {
            background: #e53e3e;
        }
        
        .logout-btn {
            position: absolute;
            top: 20px;
            left: 20px;
            padding: 10px 20px;
            background: #f56565;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        
        .tabs {
            display: flex;
            border-bottom: 2px solid #eee;
            margin-bottom: 20px;
        }
        
        .tab {
            padding: 15px 30px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
        }
        
        .tab.active {
            border-bottom: 3px solid #667eea;
            color: #667eea;
            font-weight: bold;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: right;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background: #f8f9fa;
            color: #333;
            font-weight: bold;
        }
        
        tr:hover {
            background: #f5f7fa;
        }
        
        .status-badge {
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: bold;
        }
        
        .status-pending {
            background: #fed7d7;
            color: #c53030;
        }
        
        .status-assigned {
            background: #feebc8;
            color: #c05621;
        }
        
        .status-completed {
            background: #c6f6d5;
            color: #22543d;
        }
        
        .lab-status {
            display: flex;
            gap: 20px;
            margin-top: 20px;
        }
        
        .status-btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }
        
        .status-working {
            background: #48bb78;
            color: white;
        }
        
        .status-not-working {
            background: #f56565;
            color: white;
        }
        
        .alert {
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
        }
        
        .alert-success {
            background: #c6f6d5;
            color: #22543d;
            border: 1px solid #9ae6b4;
        }
        
        .alert-error {
            background: #fed7d7;
            color: #c53030;
            border: 1px solid #fc8181;
        }
        
        .user-info {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- شاشة الدخول -->
        <div id="loginPage" class="login-container">
            <h1><i class="fas fa-shipping-fast"></i> نظام إدارة الإرساليات</h1>
            
            <div class="user-type-selector">
                <button class="user-type-btn active" onclick="selectUserType('clearance')">
                    <i class="fas fa-building"></i> مكتب التخليص
                </button>
                <button class="user-type-btn" onclick="selectUserType('employee')">
                    <i class="fas fa-users"></i> موظف
                </button>
                <button class="user-type-btn" onclick="selectUserType('lab')">
                    <i class="fas fa-flask"></i> مختبر
                </button>
            </div>
            
            <div id="alertMessage" class="alert hidden"></div>
            
            <form id="loginForm" onsubmit="login(event)">
                <div class="form-group">
                    <label for="username"><i class="fas fa-user"></i> اسم المستخدم</label>
                    <input type="text" id="username" required>
                </div>
                
                <div class="form-group">
                    <label for="password"><i class="fas fa-lock"></i> كلمة المرور</label>
                    <input type="password" id="password" required>
                </div>
                
                <input type="hidden" id="userType" value="clearance">
                
                <button type="submit" class="btn">
                    <i class="fas fa-sign-in-alt"></i> دخول
                </button>
            </form>
        </div>
        
        <!-- لوحة التحكم -->
        <div id="dashboard" class="dashboard hidden">
            <button class="logout-btn" onclick="logout()">
                <i class="fas fa-sign-out-alt"></i> خروج
            </button>
            
            <div id="userInfo" class="user-info"></div>
            
            <div class="tabs" id="tabsContainer"></div>
            
            <!-- محتوى الألسنة -->
            <div id="tabContents">
                <!-- محتوى سيتم تعبئته ديناميكياً -->
            </div>
        </div>
    </div>
    
    <script>
        // تحديد نوع المستخدم
        function selectUserType(type) {
            document.getElementById('userType').value = type;
            
            // تحديث الأزرار النشطة
            document.querySelectorAll('.user-type-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
        }
        
        // تسجيل الدخول
        function login(event) {
            event.preventDefault();
            
            const userType = document.getElementById('userType').value;
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            google.script.run
                .withSuccessHandler(handleLoginSuccess)
                .withFailureHandler(handleLoginError)
                .authenticate(userType, username, password);
        }
        
        // معالجة نجاح الدخول
        function handleLoginSuccess(response) {
            if (response.authenticated) {
                // حفظ بيانات المستخدم
                sessionStorage.setItem('userType', response.userType);
                sessionStorage.setItem('username', response.username);
                sessionStorage.setItem('name', response.name);
                
                // إظهار لوحة التحكم
                document.getElementById('loginPage').classList.add('hidden');
                document.getElementById('dashboard').classList.remove('hidden');
                
                // تحميل لوحة التحكم المناسبة
                loadDashboard(response.userType);
            } else {
                showAlert(response.message, 'error');
            }
        }
        
        // معالجة خطأ الدخول
        function handleLoginError(error) {
            showAlert('حدث خطأ في الاتصال', 'error');
            console.error(error);
        }
        
        // تحميل لوحة التحكم حسب نوع المستخدم
        function loadDashboard(userType) {
            const userInfo = document.getElementById('userInfo');
            const name = sessionStorage.getItem('name');
            const username = sessionStorage.getItem('username');
            
            userInfo.innerHTML = `
                <div>
                    <h3>مرحباً ${name}</h3>
                    <p>اسم المستخدم: ${username}</p>
                </div>
                <div>
                    <span class="status-badge ${userType === 'clearance' ? 'status-pending' : userType === 'employee' ? 'status-assigned' : 'status-completed'}">
                        ${userType === 'clearance' ? 'مكتب تخليص' : userType === 'employee' ? 'موظف' : 'مختبر'}
                    </span>
                </div>
            `;
            
            const tabsContainer = document.getElementById('tabsContainer');
            const tabContents = document.getElementById('tabContents');
            
            // إنشاء الألسنة حسب نوع المستخدم
            if (userType === 'clearance') {
                tabsContainer.innerHTML = `
                    <div class="tab active" onclick="showTab('register')">تسجيل إرسالية</div>
                    <div class="tab" onclick="showTab('myShipments')">إرسالياتي</div>
                `;
                
                tabContents.innerHTML = `
                    <div id="registerTab" class="tab-content active">
                        <h2><i class="fas fa-plus-circle"></i> تسجيل إرسالية جديدة</h2>
                        <form id="shipmentForm" onsubmit="addShipment(event)">
                            <div class="form-group">
                                <label>اسم المصدر</label>
                                <input type="text" id="source" required>
                            </div>
                            <div class="form-group">
                                <label>اسم المستورد</label>
                                <input type="text" id="importer" required>
                            </div>
                            <div class="form-group">
                                <label>الناقل</label>
                                <input type="text" id="carrier" required>
                            </div>
                            <div class="form-group">
                                <label>رقم السيارة</label>
                                <input type="text" id="carNumber" required>
                            </div>
                            <div class="form-group">
                                <label>رقم الهاتف</label>
                                <input type="text" id="phone" required>
                            </div>
                            <div class="form-group">
                                <label>الأصناف</label>
                                <textarea id="items" rows="3" required></textarea>
                            </div>
                            <button type="submit" class="btn">
                                <i class="fas fa-save"></i> تسجيل الإرسالية
                            </button>
                        </form>
                    </div>
                    
                    <div id="myShipmentsTab" class="tab-content">
                        <h2><i class="fas fa-list"></i> الإرساليات المسجلة</h2>
                        <button class="btn btn-secondary" onclick="loadMyShipments()">
                            <i class="fas fa-sync"></i> تحديث القائمة
                        </button>
                        <div id="shipmentsList"></div>
                    </div>
                `;
                
            } else if (userType === 'employee') {
                tabsContainer.innerHTML = `
                    <div class="tab active" onclick="showTab('allShipments')">جميع الإرساليات</div>
                    <div class="tab" onclick="showTab('distribute')">توزيع الإرساليات</div>
                `;
                
                tabContents.innerHTML = `
                    <div id="allShipmentsTab" class="tab-content active">
                        <h2><i class="fas fa-list-alt"></i> جميع الإرساليات</h2>
                        <button class="btn btn-secondary" onclick="loadAllShipments()">
                            <i class="fas fa-sync"></i> تحديث القائمة
                        </button>
                        <div id="allShipmentsList"></div>
                    </div>
                    
                    <div id="distributeTab" class="tab-content">
                        <h2><i class="fas fa-random"></i> توزيع الإرساليات على المختبرات</h2>
                        <p>سيتم توزيع الإرساليات المعلقة على المختبرات العاملة بشكل متوازن</p>
                        <button class="btn btn-secondary" onclick="distributeShipments()">
                            <i class="fas fa-share-alt"></i> توزيع الإرساليات
                        </button>
                        <div id="distributionResult"></div>
                    </div>
                `;
                
                // تحميل الإرساليات تلقائياً
                loadAllShipments();
                
            } else if (userType === 'lab') {
                tabsContainer.innerHTML = `
                    <div class="tab active" onclick="showTab('labStatus')">حالة المختبر</div>
                    <div class="tab" onclick="showTab('assignedShipments')">الإرساليات المسندة</div>
                `;
                
                tabContents.innerHTML = `
                    <div id="labStatusTab" class="tab-content active">
                        <h2><i class="fas fa-flask"></i> حالة المختبر</h2>
                        <p>اسم المختبر: ${name}</p>
                        <div class="lab-status">
                            <button class="status-btn status-working" onclick="updateLabStatus('يعمل')">
                                <i class="fas fa-check-circle"></i> يعمل
                            </button>
                            <button class="status-btn status-not-working" onclick="updateLabStatus('لا يعمل')">
                                <i class="fas fa-times-circle"></i> لا يعمل
                            </button>
                        </div>
                        <div id="labStatusMessage"></div>
                    </div>
                    
                    <div id="assignedShipmentsTab" class="tab-content">
                        <h2><i class="fas fa-tasks"></i> الإرساليات المسندة إلي</h2>
                        <button class="btn btn-secondary" onclick="loadAssignedShipments()">
                            <i class="fas fa-sync"></i> تحديث القائمة
                        </button>
                        <div id="assignedShipmentsList"></div>
                    </div>
                `;
                
                // تحميل الإرساليات المسندة تلقائياً
                loadAssignedShipments();
            }
        }
        
        // إظهار اللسان المحدد
        function showTab(tabName) {
            // تحديث الألسنة النشطة
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // إظهار المحتوى المناسب
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            document.getElementById(tabName + 'Tab').classList.add('active');
        }
        
        // تسجيل إرسالية جديدة (لمكاتب التخليص)
        function addShipment(event) {
            event.preventDefault();
            
            const shipmentData = {
                المصدر: document.getElementById('source').value,
                المستورد: document.getElementById('importer').value,
                الناقل: document.getElementById('carrier').value,
                رقم_السيارة: document.getElementById('carNumber').value,
                رقم_الهاتف: document.getElementById('phone').value,
                الأصناف: document.getElementById('items').value
            };
            
            const userType = sessionStorage.getItem('userType');
            const username = sessionStorage.getItem('username');
            
            google.script.run
                .withSuccessHandler(function(response) {
                    showAlert(response.message, response.success ? 'success' : 'error');
                    if (response.success) {
                        document.getElementById('shipmentForm').reset();
                    }
                })
                .withFailureHandler(handleError)
                .addShipment(shipmentData, userType, username);
        }
        
        // تحديث حالة المختبر
        function updateLabStatus(status) {
            const labName = sessionStorage.getItem('name');
            const username = sessionStorage.getItem('username');
            
            google.script.run
                .withSuccessHandler(function(response) {
                    const messageDiv = document.getElementById('labStatusMessage');
                    messageDiv.className = `alert alert-${response.success ? 'success' : 'error'}`;
                    messageDiv.textContent = response.message;
                    messageDiv.classList.remove('hidden');
                })
                .withFailureHandler(handleError)
                .updateLabStatus(labName, status, username);
        }
        
        // توزيع الإرساليات على المختبرات (للموظفين)
        function distributeShipments() {
            google.script.run
                .withSuccessHandler(function(response) {
                    const resultDiv = document.getElementById('distributionResult');
                    resultDiv.className = `alert alert-${response.success ? 'success' : 'error'}`;
                    resultDiv.textContent = response.message;
                    resultDiv.classList.remove('hidden');
                    
                    // تحديث قائمة الإرساليات
                    if (response.success) {
                        loadAllShipments();
                    }
                })
                .withFailureHandler(handleError)
                .distributeShipmentsToLabs();
        }
        
        // تحميل الإرساليات الخاصة بمكتب التخليص
        function loadMyShipments() {
            const userType = sessionStorage.getItem('userType');
            const username = sessionStorage.getItem('username');
            
            google.script.run
                .withSuccessHandler(displayShipments)
                .withFailureHandler(handleError)
                .getUserShipments(userType, username);
        }
        
        // تحميل جميع الإرساليات (للموظفين)
        function loadAllShipments() {
            const userType = sessionStorage.getItem('userType');
            const username = sessionStorage.getItem('username');
            
            google.script.run
                .withSuccessHandler(function(shipments) {
                    const container = document.getElementById('allShipmentsList');
                    
                    if (shipments.length === 0) {
                        container.innerHTML = '<div class="alert">لا توجد إرساليات</div>';
                        return;
                    }
                    
                    let html = '<table><tr><th>المصدر</th><th>المستورد</th><th>الناقل</th><th>رقم السيارة</th><th>حالة</th><th>المختبر المسند</th><th>وقت التسجيل</th></tr>';
                    
                    shipments.forEach(shipment => {
                        html += `
                            <tr>
                                <td>${shipment.المصدر || ''}</td>
                                <td>${shipment.المستورد || ''}</td>
                                <td>${shipment.الناقل || ''}</td>
                                <td>${shipment.رقم_السيارة || ''}</td>
                                <td><span class="status-badge ${shipment.status === 'مسند' ? 'status-assigned' : 'status-pending'}">${shipment.status || 'معلقة'}</span></td>
                                <td>${shipment.المختبر_المسند || 'لم يتم التوزيع'}</td>
                                <td>${shipment.timestamp || ''}</td>
                            </tr>
                        `;
                    });
                    
                    html += '</table>';
                    container.innerHTML = html;
                })
                .withFailureHandler(handleError)
                .getUserShipments('employee', username);
        }
        
        // تحميل الإرساليات المسندة للمختبر
        function loadAssignedShipments() {
            const userType = sessionStorage.getItem('userType');
            const username = sessionStorage.getItem('username');
            
            google.script.run
                .withSuccessHandler(displayShipments)
                .withFailureHandler(handleError)
                .getUserShipments(userType, username);
        }
        
        // عرض الإرساليات في جدول
        function displayShipments(shipments) {
            let container;
            const userType = sessionStorage.getItem('userType');
            
            if (userType === 'clearance') {
                container = document.getElementById('shipmentsList');
            } else if (userType === 'lab') {
                container = document.getElementById('assignedShipmentsList');
            }
            
            if (!container) return;
            
            if (shipments.length === 0) {
                container.innerHTML = '<div class="alert">لا توجد إرساليات</div>';
                return;
            }
            
            let html = '<table><tr><th>المصدر</th><th>المستورد</th><th>الناقل</th><th>رقم السيارة</th><th>رقم الهاتف</th><th>الأصناف</th><th>وقت التسجيل</th></tr>';
            
            shipments.forEach(shipment => {
                html += `
                    <tr>
                        <td>${shipment.المصدر || ''}</td>
                        <td>${shipment.المستورد || ''}</td>
                        <td>${shipment.الناقل || ''}</td>
                        <td>${shipment.رقم_السيارة || ''}</td>
                        <td>${shipment.رقم_الهاتف || ''}</td>
                        <td>${shipment.الأصناف || ''}</td>
                        <td>${shipment.timestamp || ''}</td>
                    </tr>
                `;
            });
            
            html += '</table>';
            container.innerHTML = html;
        }
        
        // تسجيل الخروج
        function logout() {
            sessionStorage.clear();
            document.getElementById('dashboard').classList.add('hidden');
            document.getElementById('loginPage').classList.remove('hidden');
            document.getElementById('loginForm').reset();
            document.getElementById('alertMessage').classList.add('hidden');
        }
        
        // عرض رسالة تنبيه
        function showAlert(message, type) {
            const alertDiv = document.getElementById('alertMessage');
            alertDiv.textContent = message;
            alertDiv.className = `alert alert-${type}`;
            alertDiv.classList.remove('hidden');
            
            setTimeout(() => {
                alertDiv.classList.add('hidden');
            }, 5000);
        }
        
        // معالجة الأخطاء العامة
        function handleError(error) {
            console.error(error);
            showAlert('حدث خطأ في النظام', 'error');
        }
        
        // تهيئة الصفحة
        window.onload = function() {
            // إعادة تعيين حالة المستخدم
            sessionStorage.clear();
        };
    </script>
</body>
</html>