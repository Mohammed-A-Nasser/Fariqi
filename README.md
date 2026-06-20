<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes" />
    <meta name="theme-color" content="#00897B" />
    <link rel="manifest" href="manifest.json" />
    <title>فريقي - نظام إدارة المكتب الفني المتطور</title>
    
    <!-- Bootstrap RTL -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css" />
    <!-- Font Awesome 6 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-storage-compat.js"></script>
    
    <style>
        /* ===== متغيرات محسنة ===== */
        :root {
            --teal: #00897B;
            --teal-dark: #00695C;
            --teal-light: #e0f2f1;
            --teal-gradient: linear-gradient(135deg, #00897B 0%, #004D40 100%);
            --sidebar-width: 280px;
            --shadow-sm: 0 2px 8px rgba(0,0,0,0.08);
            --shadow-md: 0 4px 16px rgba(0,0,0,0.12);
            --shadow-lg: 0 8px 32px rgba(0,0,0,0.16);
            --radius: 16px;
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        /* ===== الوضع الليلي ===== */
        [data-theme="dark"] {
            --teal: #26A69A;
            --teal-dark: #00897B;
            --teal-light: #1a2a2a;
            background: #121212;
            color: #e0e0e0;
        }
        [data-theme="dark"] .card,
        [data-theme="dark"] .sidebar,
        [data-theme="dark"] .modal-content,
        [data-theme="dark"] .list-group-item,
        [data-theme="dark"] .department-report-card {
            background: #1e1e1e !important;
            color: #e0e0e0 !important;
            border-color: #333 !important;
        }
        [data-theme="dark"] .card-header,
        [data-theme="dark"] .modal-header {
            background: #2a2a2a !important;
            color: #e0e0e0 !important;
            border-color: #333 !important;
        }
        [data-theme="dark"] .table {
            color: #e0e0e0;
        }
        [data-theme="dark"] .table-light {
            background: #2a2a2a;
            color: #e0e0e0;
        }
        [data-theme="dark"] .text-muted {
            color: #aaa !important;
        }
        [data-theme="dark"] .form-control,
        [data-theme="dark"] .form-select {
            background: #2a2a2a;
            color: #e0e0e0;
            border-color: #444;
        }
        [data-theme="dark"] .form-control:focus,
        [data-theme="dark"] .form-select:focus {
            background: #333;
            color: #e0e0e0;
        }

        /* ===== عام ===== */
        * { box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, sans-serif;
            background: #f4f6f8;
            color: #333;
            margin: 0;
            padding: 0;
            transition: var(--transition);
        }
        .page-section { display: none; animation: fadeSlideIn 0.4s ease; }
        .page-section.active { display: block; }
        @keyframes fadeSlideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ===== ألوان ===== */
        .text-teal { color: var(--teal) !important; }
        .bg-teal { background: var(--teal-gradient) !important; color: #fff; }
        .btn-teal {
            background: var(--teal-gradient);
            color: #fff;
            border: none;
            transition: var(--transition);
        }
        .btn-teal:hover, .btn-teal:focus {
            transform: translateY(-2px);
            box-shadow: var(--shadow-md);
            color: #fff;
        }
        .btn-outline-teal {
            border: 2px solid var(--teal);
            color: var(--teal);
            background: transparent;
            transition: var(--transition);
        }
        .btn-outline-teal:hover {
            background: var(--teal);
            color: #fff;
            transform: translateY(-2px);
        }
        .btn-success {
            background: linear-gradient(135deg, #43A047, #1B5E20);
            border: none;
            transition: var(--transition);
        }
        .btn-success:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow-md);
        }

        /* ===== صفحة تسجيل الدخول ===== */
        .login-page {
            background: var(--teal-gradient);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        .login-page .card {
            border-radius: var(--radius);
            animation: fadeInUp 0.6s ease;
            backdrop-filter: blur(10px);
            background: rgba(255,255,255,0.95);
        }
        [data-theme="dark"] .login-page .card {
            background: rgba(30,30,30,0.95);
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px) scale(0.95); }
            to { opacity: 1; transform: translateY(0) scale(1); }
        }
        .logo-circle {
            width: 80px;
            height: 80px;
            background: var(--teal-light);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 16px;
            transition: var(--transition);
        }
        .logo-circle:hover {
            transform: scale(1.05) rotate(-5deg);
        }

        /* ===== الشريط الجانبي ===== */
        .sidebar {
            position: fixed;
            top: 0;
            right: 0;
            width: var(--sidebar-width);
            height: 100vh;
            background: #fff;
            box-shadow: var(--shadow-md);
            display: flex;
            flex-direction: column;
            padding: 20px 0;
            z-index: 1000;
            overflow-y: auto;
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .sidebar::-webkit-scrollbar { width: 4px; }
        .sidebar::-webkit-scrollbar-thumb { background: var(--teal); border-radius: 4px; }
        .sidebar-header {
            padding: 0 20px 16px;
            border-bottom: 1px solid #eee;
            margin-bottom: 8px;
        }
        .sidebar .nav-link {
            color: #555;
            padding: 12px 20px;
            border-radius: 0;
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 0.95rem;
            transition: var(--transition);
            cursor: pointer;
            border: none;
            background: none;
            width: 100%;
            text-align: right;
            position: relative;
        }
        .sidebar .nav-link::before {
            content: '';
            position: absolute;
            right: 0;
            top: 0;
            bottom: 0;
            width: 4px;
            background: var(--teal);
            transform: scaleY(0);
            transition: var(--transition);
        }
        .sidebar .nav-link:hover::before,
        .sidebar .nav-link.active::before {
            transform: scaleY(1);
        }
        .sidebar .nav-link:hover,
        .sidebar .nav-link.active {
            background: var(--teal-light);
            color: var(--teal);
            font-weight: 600;
        }
        .sidebar .nav-link i { width: 20px; text-align: center; font-size: 1.1rem; }
        .sidebar .badge {
            margin-right: auto;
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }
        .sidebar .logout-btn {
            margin-top: auto;
            padding: 14px 20px;
            display: block;
            border-top: 1px solid #eee;
        }
        .avatar-circle {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.1rem;
            flex-shrink: 0;
            background: var(--teal-gradient);
            color: #fff;
            transition: var(--transition);
        }
        .avatar-circle:hover {
            transform: scale(1.05);
        }

        /* ===== المحتوى الرئيسي ===== */
        .main-content {
            margin-right: var(--sidebar-width);
            padding: 28px 30px;
            min-height: 100vh;
            transition: var(--transition);
        }

        /* ===== بانر الترحيب ===== */
        .welcome-banner {
            background: var(--teal-gradient);
            border-radius: var(--radius);
            padding: 30px;
            position: relative;
            overflow: hidden;
        }
        .welcome-banner::after {
            content: '';
            position: absolute;
            top: -50%;
            right: -10%;
            width: 300px;
            height: 300px;
            background: rgba(255,255,255,0.05);
            border-radius: 50%;
            animation: floatBanner 6s ease-in-out infinite;
        }
        @keyframes floatBanner {
            0%, 100% { transform: translate(0, 0); }
            50% { transform: translate(-20px, -20px); }
        }

        /* ===== بطاقات الإحصائيات ===== */
        .stat-card {
            border-radius: var(--radius);
            padding: 24px;
            color: #fff;
            position: relative;
            overflow: hidden;
            transition: var(--transition);
            cursor: pointer;
        }
        .stat-card:hover {
            transform: translateY(-4px);
            box-shadow: var(--shadow-lg);
        }
        .stat-card .number {
            font-size: 2.6rem;
            font-weight: 700;
            line-height: 1;
        }
        .stat-card .label { font-size: 0.9rem; opacity: 0.9; margin-top: 4px; }
        .stat-card .trend { font-size: 0.78rem; margin-top: 10px; opacity: 0.8; }
        .stat-card .icon {
            position: absolute;
            top: 16px;
            left: 16px;
            font-size: 2.8rem;
            opacity: 0.15;
        }
        .stat-card.total { background: linear-gradient(135deg, #00897B, #004D40); }
        .stat-card.pending { background: linear-gradient(135deg, #FB8C00, #E65100); }
        .stat-card.completed { background: linear-gradient(135deg, #43A047, #1B5E20); }
        .stat-card.overdue { background: linear-gradient(135deg, #E53935, #B71C1C); }

        /* ===== تحسينات البطاقات ===== */
        .card {
            border: none;
            border-radius: var(--radius);
            box-shadow: var(--shadow-sm);
            transition: var(--transition);
            overflow: hidden;
        }
        .card:hover {
            box-shadow: var(--shadow-md);
        }
        .card-header {
            background: transparent;
            border-bottom: 1px solid rgba(0,0,0,0.06);
            padding: 16px 20px;
            font-weight: 600;
        }

        /* ===== المهام الحرجة ===== */
        .critical-task {
            border-right: 4px solid transparent;
            transition: var(--transition);
            padding: 14px 20px;
        }
        .critical-task.overdue { border-right-color: #dc3545; background: #fff5f5; }
        .critical-task.warning { border-right-color: #ffc107; background: #fffbf0; }
        .critical-task:hover {
            background: #f8f9fa;
            transform: translateX(-4px);
        }

        /* ===== بطاقات الإدارات ===== */
        .department-card {
            transition: var(--transition);
            cursor: pointer;
        }
        .department-card:hover {
            transform: translateY(-6px);
            box-shadow: var(--shadow-lg) !important;
        }

        /* ===== بطاقات التقارير ===== */
        .report-card {
            cursor: pointer;
            transition: var(--transition);
            padding: 24px;
        }
        .report-card:hover {
            transform: translateY(-6px);
            box-shadow: var(--shadow-lg) !important;
        }
        .report-card i {
            transition: var(--transition);
        }
        .report-card:hover i {
            transform: scale(1.1);
        }

        /* ===== تحسينات الجدول ===== */
        .table-hover tbody tr {
            transition: var(--transition);
        }
        .table-hover tbody tr:hover {
            background: var(--teal-light);
            transform: scale(1.01);
        }

        /* ===== المودال ===== */
        .modal-content {
            border: none;
            border-radius: var(--radius);
            box-shadow: var(--shadow-lg);
        }
        .modal-header {
            border-bottom: 1px solid rgba(0,0,0,0.06);
            padding: 20px 24px;
        }
        .modal-footer {
            border-top: 1px solid rgba(0,0,0,0.06);
            padding: 16px 24px;
        }

        /* ===== زر الطباعة ===== */
        .print-btn {
            position: fixed;
            bottom: 30px;
            left: 30px;
            z-index: 999;
            padding: 16px 32px;
            border-radius: 50px;
            box-shadow: var(--shadow-lg);
            transition: var(--transition);
        }
        .print-btn:hover {
            transform: translateY(-4px) scale(1.05);
        }

        /* ===== شريط التقدم ===== */
        .progress {
            border-radius: 8px;
            overflow: hidden;
            height: 8px;
        }
        .progress-bar {
            transition: width 1s cubic-bezier(0.4, 0, 0.2, 1);
        }

        /* ===== تذكيرات ===== */
        .reminder-badge {
            animation: glow 2s ease-in-out infinite;
        }
        @keyframes glow {
            0%, 100% { box-shadow: 0 0 5px rgba(255, 193, 7, 0.3); }
            50% { box-shadow: 0 0 20px rgba(255, 193, 7, 0.6); }
        }

        /* ===== استجابة ===== */
        @media (max-width: 992px) {
            .main-content { padding: 20px; }
        }
        @media (max-width: 768px) {
            .main-content {
                margin-right: 0;
                padding: 16px;
            }
            .sidebar {
                transform: translateX(100%);
                width: 300px;
            }
            .sidebar.open { transform: translateX(0); }
            .stat-card .number { font-size: 2rem; }
            .welcome-banner { padding: 20px; }
        }
        @media (max-width: 576px) {
            .main-content { padding: 12px; }
            .stat-card { padding: 16px; }
            .stat-card .number { font-size: 1.6rem; }
        }

        /* ===== طباعة ===== */
        @media print {
            .no-print { display: none !important; }
            .sidebar { display: none !important; }
            .main-content { margin: 0 !important; padding: 20px !important; }
            .page-section { display: block !important; }
            .page-section:not(.active) { display: none !important; }
            #page-department-report { display: block !important; }
            .card { box-shadow: none !important; border: 1px solid #ddd !important; }
            body { background: white !important; }
        }
        @page { 
            size: A4; 
            margin: 15mm;
        }

        /* ===== تحميل ===== */
        .loading-spinner {
            display: inline-block;
            width: 24px;
            height: 24px;
            border: 3px solid rgba(0,0,0,0.1);
            border-radius: 50%;
            border-top-color: var(--teal);
            animation: spin 0.8s ease-in-out infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* ===== شريط التقدم العلوي ===== */
        .top-progress {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            height: 3px;
            z-index: 9999;
            background: var(--teal-gradient);
            transform-origin: right;
            transform: scaleX(0);
            transition: transform 0.3s ease;
        }
        .top-progress.loading {
            transform: scaleX(1);
        }

        /* ===== كرة التحميل ===== */
        .float-loading {
            position: fixed;
            bottom: 100px;
            left: 30px;
            z-index: 998;
            background: white;
            padding: 12px 20px;
            border-radius: 50px;
            box-shadow: var(--shadow-lg);
            display: none;
            align-items: center;
            gap: 12px;
        }
        .float-loading.show {
            display: flex;
            animation: fadeSlideIn 0.3s ease;
        }
        [data-theme="dark"] .float-loading {
            background: #1e1e1e;
            color: #e0e0e0;
        }
    </style>
</head>
<body>

<!-- ===== شريط التقدم العلوي ===== -->
<div class="top-progress" id="topProgress"></div>

<!-- ===== كرة التحميل ===== -->
<div class="float-loading" id="floatLoading">
    <div class="loading-spinner"></div>
    <span>جاري التحميل...</span>
</div>

<!-- ============================================= -->
<!-- ===== صفحة تسجيل الدخول ===== -->
<!-- ============================================= -->
<div id="page-login" class="login-page">
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-5 col-lg-4">
                <div class="card shadow-lg border-0 rounded-4 p-4">
                    <div class="card-body">
                        <div class="text-center mb-4">
                            <div class="logo-circle">
                                <i class="fas fa-tasks fa-3x text-teal"></i>
                            </div>
                            <h1 class="fw-bold text-success">فريقي</h1>
                            <p class="text-muted">نظام إدارة المكتب الفني والتكليفات</p>
                            <p class="text-muted small">مستشفى القنطرة شرق</p>
                        </div>
                        <div id="loginError" class="alert alert-danger d-none" role="alert">
                            <i class="fas fa-exclamation-circle me-2"></i>
                            اسم المستخدم أو كلمة المرور غير صحيحة
                        </div>
                        <div id="loginSuccess" class="alert alert-success d-none" role="alert">
                            <i class="fas fa-check-circle me-2"></i>
                            تم تسجيل الدخول بنجاح...
                        </div>
                        <form id="loginForm">
                            <div class="mb-3">
                                <label class="form-label fw-semibold"><i class="fas fa-user me-2"></i>اسم المستخدم</label>
                                <input type="text" class="form-control form-control-lg" id="loginUsername" placeholder="أدخل اسم المستخدم" required autofocus />
                            </div>
                            <div class="mb-3">
                                <label class="form-label fw-semibold"><i class="fas fa-lock me-2"></i>كلمة المرور</label>
                                <div class="input-group">
                                    <input type="password" class="form-control form-control-lg" id="loginPassword" placeholder="أدخل كلمة المرور" required />
                                    <button class="btn btn-outline-secondary" type="button" id="togglePassword"><i class="fas fa-eye"></i></button>
                                </div>
                            </div>
                            <div class="mb-3">
                                <small class="text-muted"><i class="fas fa-info-circle me-1"></i>تجريبي: admin / admin123</small>
                            </div>
                            <button type="submit" class="btn btn-success btn-lg w-100 fw-semibold" id="loginBtn">
                                <i class="fas fa-sign-in-alt me-2"></i>تسجيل الدخول
                            </button>
                        </form>
                        <div class="text-center mt-3">
                            <small class="text-muted"><i class="fas fa-headset me-1"></i>للدعم: support@hospital.local</small>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- ============================================= -->
<!-- ===== التطبيق الرئيسي ===== -->
<!-- ============================================= -->
<div id="app-container">

    <!-- ===== القائمة الجانبية ===== -->
    <nav class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <h3 class="text-success fw-bold"><i class="fas fa-tasks text-teal"></i> فريقي</h3>
            <small class="text-muted">مستشفى القنطرة شرق</small>
        </div>
        <ul class="nav flex-column">
            <li class="nav-item"><button class="nav-link active" data-page="dashboard"><i class="fas fa-chart-pie"></i> لوحة التحكم</button></li>
            <li class="nav-item"><button class="nav-link" data-page="tasks"><i class="fas fa-tasks"></i> التكليفات <span class="badge bg-danger rounded-pill ms-2" id="taskBadge">3</span></button></li>
            <li class="nav-item"><button class="nav-link" data-page="users"><i class="fas fa-users"></i> المستخدمون</button></li>
            <li class="nav-item"><button class="nav-link" data-page="departments"><i class="fas fa-building"></i> الإدارات</button></li>
            <li class="nav-item"><button class="nav-link" data-page="reports"><i class="fas fa-file-alt"></i> التقارير</button></li>
            <li class="nav-item"><button class="nav-link" data-page="notifications"><i class="fas fa-bell"></i> الإشعارات <span class="badge bg-danger rounded-pill ms-2" id="notifBadge">5</span></button></li>
            <li class="nav-item"><button class="nav-link" data-page="settings"><i class="fas fa-cog"></i> الإعدادات</button></li>
        </ul>
        <hr />
        <div class="user-info p-3">
            <div class="d-flex align-items-center">
                <div class="avatar-circle" id="sidebarAvatar"><span id="sidebarUserInitial">أ</span></div>
                <div class="ms-3">
                    <p class="mb-0 fw-semibold" id="sidebarUserName">أحمد محمد</p>
                    <small class="text-muted" id="sidebarUserRole">مدير المكتب الفني</small>
                </div>
            </div>
        </div>
        <button class="nav-link text-danger fw-semibold logout-btn" id="logoutBtn">
            <i class="fas fa-sign-out-alt"></i> تسجيل الخروج
        </button>
    </nav>

    <!-- ===== المحتوى الرئيسي ===== -->
    <div class="main-content" id="mainContent">
        <button class="btn btn-teal d-md-none mb-3" id="sidebarToggle">
            <i class="fas fa-bars"></i>
        </button>

        <!-- ========================================= -->
        <!-- صفحة لوحة التحكم -->
        <!-- ========================================= -->
        <div class="page-section active" id="page-dashboard">
            <div class="welcome-banner text-white mb-4">
                <div class="row align-items-center">
                    <div class="col-md-8">
                        <h4><i class="fas fa-user-circle me-2"></i> مرحباً، <span id="dashboardUserName">أحمد محمد</span>!</h4>
                        <p class="mb-0">هذه هي لوحة التحكم الرئيسية لنظام فريقي. يمكنك متابعة التكليفات والإحصائيات من هنا.</p>
                    </div>
                    <div class="col-md-4 text-md-end mt-3 mt-md-0">
                        <span class="badge bg-light text-dark p-2">
                            <i class="fas fa-calendar-alt me-1"></i>
                            <span id="currentDate"></span>
                        </span>
                    </div>
                </div>
            </div>

            <!-- الإحصائيات -->
            <div class="row g-4 mb-4">
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card total">
                        <div class="d-flex justify-content-between align-items-center">
                            <div><div class="number" id="statTotalTasks">0</div><div class="label">إجمالي التكليفات</div></div>
                            <div class="icon"><i class="fas fa-tasks"></i></div>
                        </div>
                        <div class="trend"><i class="fas fa-arrow-up me-1"></i> <span id="trendTotal">0%</span> هذا الأسبوع</div>
                    </div>
                </div>
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card pending">
                        <div class="d-flex justify-content-between align-items-center">
                            <div><div class="number" id="statPendingTasks">0</div><div class="label">قيد التنفيذ</div></div>
                            <div class="icon"><i class="fas fa-clock"></i></div>
                        </div>
                        <div class="trend"><i class="fas fa-spinner me-1"></i> <span id="trendPending">0</span> مهام جديدة اليوم</div>
                    </div>
                </div>
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card completed">
                        <div class="d-flex justify-content-between align-items-center">
                            <div><div class="number" id="statCompletedTasks">0</div><div class="label">مكتملة</div></div>
                            <div class="icon"><i class="fas fa-check-circle"></i></div>
                        </div>
                        <div class="trend"><i class="fas fa-check me-1"></i> <span id="trendCompleted">0%</span> إنجاز</div>
                    </div>
                </div>
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card overdue">
                        <div class="d-flex justify-content-between align-items-center">
                            <div><div class="number" id="statOverdueTasks">0</div><div class="label">متأخرة</div></div>
                            <div class="icon"><i class="fas fa-exclamation-triangle"></i></div>
                        </div>
                        <div class="trend"><i class="fas fa-exclamation-circle me-1"></i> بحاجة للتدخل</div>
                    </div>
                </div>
            </div>

            <!-- المخططات -->
            <div class="row g-4 mb-4">
                <div class="col-lg-6">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-chart-bar text-teal me-2"></i> أداء الإدارات</div>
                        <div class="card-body" id="deptPerformance">
                            <div class="department-progress mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span><i class="fas fa-circle text-teal me-1"></i> المكتب الفني</span>
                                    <span class="fw-semibold" id="deptTechPercent">92%</span>
                                </div>
                                <div class="progress"><div class="progress-bar bg-teal" id="deptTechBar" style="width: 92%;"></div></div>
                                <small class="text-muted"><span id="deptTechTasks">12</span> مهمة | <span id="deptTechDone">11</span> مكتملة</small>
                            </div>
                            <div class="department-progress mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span><i class="fas fa-circle text-success me-1"></i> الجودة</span>
                                    <span class="fw-semibold" id="deptQualityPercent">78%</span>
                                </div>
                                <div class="progress"><div class="progress-bar bg-success" id="deptQualityBar" style="width: 78%;"></div></div>
                                <small class="text-muted"><span id="deptQualityTasks">8</span> مهام | <span id="deptQualityDone">6</span> مكتملة</small>
                            </div>
                            <div class="department-progress mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span><i class="fas fa-circle text-warning me-1"></i> الموارد البشرية</span>
                                    <span class="fw-semibold" id="deptHRPercent">65%</span>
                                </div>
                                <div class="progress"><div class="progress-bar bg-warning" id="deptHRBar" style="width: 65%;"></div></div>
                                <small class="text-muted"><span id="deptHRTasks">6</span> مهام | <span id="deptHRDone">4</span> مكتملة</small>
                            </div>
                            <div class="department-progress mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span><i class="fas fa-circle text-danger me-1"></i> مكافحة العدوى</span>
                                    <span class="fw-semibold" id="deptInfectionPercent">43%</span>
                                </div>
                                <div class="progress"><div class="progress-bar bg-danger" id="deptInfectionBar" style="width: 43%;"></div></div>
                                <small class="text-muted"><span id="deptInfectionTasks">5</span> مهام | <span id="deptInfectionDone">2</span> مكتملة</small>
                            </div>
                            <div class="department-progress mb-3">
                                <div class="d-flex justify-content-between mb-1">
                                    <span><i class="fas fa-circle text-info me-1"></i> التمريض</span>
                                    <span class="fw-semibold" id="deptNursingPercent">70%</span>
                                </div>
                                <div class="progress"><div class="progress-bar bg-info" id="deptNursingBar" style="width: 70%;"></div></div>
                                <small class="text-muted"><span id="deptNursingTasks">10</span> مهام | <span id="deptNursingDone">7</span> مكتملة</small>
                            </div>
                            <div class="department-progress">
                                <div class="d-flex justify-content-between mb-1">
                                    <span><i class="fas fa-circle text-secondary me-1"></i> الصيدلة</span>
                                    <span class="fw-semibold" id="deptPharmacyPercent">55%</span>
                                </div>
                                <div class="progress"><div class="progress-bar bg-secondary" id="deptPharmacyBar" style="width: 55%;"></div></div>
                                <small class="text-muted"><span id="deptPharmacyTasks">4</span> مهام | <span id="deptPharmacyDone">2</span> مكتملة</small>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-6">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-chart-pie text-teal me-2"></i> توزيع المهام</div>
                        <div class="card-body"><canvas id="taskPieChart" height="250"></canvas></div>
                    </div>
                    <div class="card mt-4">
                        <div class="card-header"><i class="fas fa-star text-warning me-2"></i> تقييم الإدارات</div>
                        <div class="card-body">
                            <div class="table-responsive">
                                <table class="table table-hover" id="deptRatingTable">
                                    <thead><tr><th>الإدارة</th><th>المهام</th><th>المكتملة</th><th>نسبة الإنجاز</th><th>التقييم</th></tr></thead>
                                    <tbody id="deptRatingBody"></tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- المهام الحرجة و KPI -->
            <div class="row g-4">
                <div class="col-lg-7">
                    <div class="card">
                        <div class="card-header d-flex justify-content-between align-items-center">
                            <span><i class="fas fa-fire text-danger me-2"></i> المهام الحرجة</span>
                            <button class="btn btn-sm btn-teal no-print" onclick="navigateTo('tasks')">عرض الكل</button>
                        </div>
                        <div class="card-body p-0" id="criticalTasksList">
                            <div class="list-group list-group-flush">
                                <div class="list-group-item border-0 py-3 critical-task overdue">
                                    <div class="d-flex justify-content-between align-items-center">
                                        <div>
                                            <span class="badge bg-danger me-2">متأخرة</span>
                                            <span class="fw-semibold">تقرير المكافحة الشهري</span>
                                            <br><small class="text-muted">مكافحة العدوى - أ. خالد</small>
                                        </div>
                                        <div class="text-end">
                                            <small class="text-danger d-block">متبقي 0 يوم</small>
                                            <button class="btn btn-sm btn-outline-teal mt-1" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        </div>
                                    </div>
                                </div>
                                <div class="list-group-item border-0 py-3 critical-task warning">
                                    <div class="d-flex justify-content-between align-items-center">
                                        <div>
                                            <span class="badge bg-warning text-dark me-2">قيد التنفيذ</span>
                                            <span class="fw-semibold">تحديث سياسات الجودة</span>
                                            <br><small class="text-muted">الجودة - د. سارة</small>
                                        </div>
                                        <div class="text-end">
                                            <small class="text-warning d-block">متبقي 3 أيام</small>
                                            <button class="btn btn-sm btn-outline-teal mt-1" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        </div>
                                    </div>
                                </div>
                                <div class="list-group-item border-0 py-3 critical-task warning">
                                    <div class="d-flex justify-content-between align-items-center">
                                        <div>
                                            <span class="badge bg-warning text-dark me-2">قيد التنفيذ</span>
                                            <span class="fw-semibold">جرد الأدوية الفصلية</span>
                                            <br><small class="text-muted">الصيدلة - ص. نادر</small>
                                        </div>
                                        <div class="text-end">
                                            <small class="text-warning d-block">متبقي 5 أيام</small>
                                            <button class="btn btn-sm btn-outline-teal mt-1" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-5">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-chart-line text-teal me-2"></i> مؤشرات الأداء الرئيسية (KPI)</div>
                        <div class="card-body">
                            <div class="mb-3">
                                <div class="d-flex justify-content-between">
                                    <span>متوسط وقت الإنجاز</span>
                                    <span class="fw-semibold" id="kpiAvgTime">4.2 يوم</span>
                                </div>
                                <small class="text-muted">من <span id="kpiTotalTasks">24</span> مهمة</small>
                            </div>
                            <div class="mb-3">
                                <div class="d-flex justify-content-between">
                                    <span>نسبة المهام المتأخرة</span>
                                    <span class="fw-semibold text-danger" id="kpiOverdueRate">8.3%</span>
                                </div>
                                <small class="text-muted"><span id="kpiOverdueCount">2</span> من <span id="kpiTotalTasks2">24</span> متأخرة</small>
                            </div>
                            <div class="mb-3">
                                <div class="d-flex justify-content-between">
                                    <span>معدل الإنجاز الأسبوعي</span>
                                    <span class="fw-semibold text-success" id="kpiWeeklyRate">4 مهام/أسبوع</span>
                                </div>
                                <small class="text-muted">متوسط 4 مهام مكتملة أسبوعياً</small>
                            </div>
                            <div>
                                <div class="d-flex justify-content-between">
                                    <span>تقييم الأداء العام</span>
                                    <span class="fw-semibold text-teal" id="kpiOverall">جيد جداً (78%)</span>
                                </div>
                                <div class="progress mt-1">
                                    <div class="progress-bar bg-teal" id="kpiOverallBar" style="width: 78%;"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="text-center mt-4 no-print">
                <button class="btn btn-lg btn-success" onclick="navigateTo('department-report')">
                    <i class="fas fa-file-pdf me-2"></i> إنشاء تقرير PDF شامل للإدارات
                </button>
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة التكليفات -->
        <!-- ========================================= -->
        <div class="page-section" id="page-tasks">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-tasks text-teal me-2"></i> التكليفات</h4>
                <button class="btn btn-success" data-bs-toggle="modal" data-bs-target="#newTaskModal">
                    <i class="fas fa-plus me-2"></i> تكليف جديد
                </button>
            </div>

            <!-- فلاتر -->
            <div class="card mb-4">
                <div class="card-body">
                    <div class="row g-3">
                        <div class="col-md-4">
                            <input type="text" class="form-control" id="searchTask" placeholder="بحث عن تكليف..." />
                        </div>
                        <div class="col-md-3">
                            <select class="form-select" id="filterStatus">
                                <option value="">كل الحالات</option>
                                <option value="pending">قيد التنفيذ</option>
                                <option value="completed">مكتملة</option>
                                <option value="overdue">متأخرة</option>
                            </select>
                        </div>
                        <div class="col-md-3">
                            <select class="form-select" id="filterDept">
                                <option value="">كل الإدارات</option>
                                <option value="tech">المكتب الفني</option>
                                <option value="quality">الجودة</option>
                                <option value="hr">الموارد البشرية</option>
                                <option value="infection">مكافحة العدوى</option>
                                <option value="nursing">التمريض</option>
                                <option value="pharmacy">الصيدلة</option>
                            </select>
                        </div>
                        <div class="col-md-2">
                            <button class="btn btn-outline-teal w-100" id="clearFilters"><i class="fas fa-times me-1"></i> مسح</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- جدول التكليفات -->
            <div class="card">
                <div class="card-body p-0">
                    <div class="table-responsive">
                        <table class="table table-hover mb-0">
                            <thead class="table-light">
                                <tr>
                                    <th>#</th>
                                    <th>عنوان التكليف</th>
                                    <th>الإدارة</th>
                                    <th>المسؤول</th>
                                    <th>تاريخ التسليم</th>
                                    <th>الحالة</th>
                                    <th>الأولوية</th>
                                    <th>الإجراءات</th>
                                </tr>
                            </thead>
                            <tbody id="tasksTableBody">
                                <tr data-status="overdue" data-dept="infection">
                                    <td>1</td>
                                    <td>تقرير المكافحة الشهري</td>
                                    <td>مكافحة العدوى</td>
                                    <td>أ. خالد</td>
                                    <td>2026-06-18</td>
                                    <td><span class="badge bg-danger">متأخرة</span></td>
                                    <td><span class="badge bg-danger">حرجة</span></td>
                                    <td>
                                        <button class="btn btn-sm btn-outline-teal" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-edit"></i></button>
                                        <button class="btn btn-sm btn-outline-danger" onclick="deleteTask(this)"><i class="fas fa-trash"></i></button>
                                    </td>
                                </tr>
                                <tr data-status="pending" data-dept="quality">
                                    <td>2</td>
                                    <td>تحديث سياسات الجودة</td>
                                    <td>الجودة</td>
                                    <td>د. سارة</td>
                                    <td>2026-06-21</td>
                                    <td><span class="badge bg-warning text-dark">قيد التنفيذ</span></td>
                                    <td><span class="badge bg-danger">حرجة</span></td>
                                    <td>
                                        <button class="btn btn-sm btn-outline-teal" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-edit"></i></button>
                                        <button class="btn btn-sm btn-outline-danger" onclick="deleteTask(this)"><i class="fas fa-trash"></i></button>
                                    </td>
                                </tr>
                                <tr data-status="pending" data-dept="pharmacy">
                                    <td>3</td>
                                    <td>جرد الأدوية الفصلية</td>
                                    <td>الصيدلة</td>
                                    <td>ص. نادر</td>
                                    <td>2026-06-23</td>
                                    <td><span class="badge bg-warning text-dark">قيد التنفيذ</span></td>
                                    <td><span class="badge bg-warning text-dark">متوسطة</span></td>
                                    <td>
                                        <button class="btn btn-sm btn-outline-teal" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-edit"></i></button>
                                        <button class="btn btn-sm btn-outline-danger" onclick="deleteTask(this)"><i class="fas fa-trash"></i></button>
                                    </td>
                                </tr>
                                <tr data-status="completed" data-dept="tech">
                                    <td>4</td>
                                    <td>اجتماع المكتب الفني الأسبوعي</td>
                                    <td>المكتب الفني</td>
                                    <td>أحمد محمد</td>
                                    <td>2026-06-15</td>
                                    <td><span class="badge bg-success">مكتملة</span></td>
                                    <td><span class="badge bg-success">عادية</span></td>
                                    <td>
                                        <button class="btn btn-sm btn-outline-teal" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-edit"></i></button>
                                        <button class="btn btn-sm btn-outline-danger" onclick="deleteTask(this)"><i class="fas fa-trash"></i></button>
                                    </td>
                                </tr>
                                <tr data-status="completed" data-dept="hr">
                                    <td>5</td>
                                    <td>تقييم أداء الموظفين الربعي</td>
                                    <td>الموارد البشرية</td>
                                    <td>م. يوسف</td>
                                    <td>2026-06-10</td>
                                    <td><span class="badge bg-success">مكتملة</span></td>
                                    <td><span class="badge bg-warning text-dark">متوسطة</span></td>
                                    <td>
                                        <button class="btn btn-sm btn-outline-teal" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-edit"></i></button>
                                        <button class="btn btn-sm btn-outline-danger" onclick="deleteTask(this)"><i class="fas fa-trash"></i></button>
                                    </td>
                                </tr>
                                <tr data-status="pending" data-dept="nursing">
                                    <td>6</td>
                                    <td>برنامج تدريب التمريض الجديد</td>
                                    <td>التمريض</td>
                                    <td>م. نورة</td>
                                    <td>2026-06-28</td>
                                    <td><span class="badge bg-warning text-dark">قيد التنفيذ</span></td>
                                    <td><span class="badge bg-warning text-dark">متوسطة</span></td>
                                    <td>
                                        <button class="btn btn-sm btn-outline-teal" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                                        <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-edit"></i></button>
                                        <button class="btn btn-sm btn-outline-danger" onclick="deleteTask(this)"><i class="fas fa-trash"></i></button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة المستخدمون -->
        <!-- ========================================= -->
        <div class="page-section" id="page-users">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-users text-teal me-2"></i> المستخدمون</h4>
                <button class="btn btn-success" data-bs-toggle="modal" data-bs-target="#newUserModal">
                    <i class="fas fa-user-plus me-2"></i> مستخدم جديد
                </button>
            </div>
            <div class="row g-4 mb-4">
                <div class="col-md-3">
                    <div class="stat-card total p-3">
                        <div class="number" id="totalUsers">0</div>
                        <div class="label">إجمالي المستخدمين</div>
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="stat-card pending p-3">
                        <div class="number" id="activeUsers">0</div>
                        <div class="label">نشط</div>
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="stat-card completed p-3">
                        <div class="number" id="inactiveUsers">0</div>
                        <div class="label">غير نشط</div>
                    </div>
                </div>
                <div class="col-md-3">
                    <div class="stat-card overdue p-3">
                        <div class="number" id="suspendedUsers">0</div>
                        <div class="label">معلق</div>
                    </div>
                </div>
            </div>
            <div class="card">
                <div class="card-body p-0">
                    <div class="table-responsive">
                        <table class="table table-hover mb-0" id="usersTable">
                            <thead class="table-light">
                                <tr>
                                    <th>#</th>
                                    <th>الاسم</th>
                                    <th>البريد الإلكتروني</th>
                                    <th>الإدارة</th>
                                    <th>الدور</th>
                                    <th>الحالة</th>
                                    <th>الإجراءات</th>
                                </tr>
                            </thead>
                            <tbody id="usersBody">
                                <tr><td colspan="7" class="text-center">⏳ جاري تحميل المستخدمين...</td></tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة الإدارات -->
        <!-- ========================================= -->
        <div class="page-section" id="page-departments">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-building text-teal me-2"></i> الإدارات</h4>
                <button class="btn btn-success" data-bs-toggle="modal" data-bs-target="#newDeptModal">
                    <i class="fas fa-plus me-2"></i> إدارة جديدة
                </button>
            </div>
            <div class="row g-4" id="departmentsContainer">
                <!-- سيتم ملؤها بواسطة JavaScript -->
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة التقارير -->
        <!-- ========================================= -->
        <div class="page-section" id="page-reports">
            <h4 class="mb-4"><i class="fas fa-file-alt text-teal me-2"></i> التقارير</h4>
            <div class="row g-4 mb-4">
                <div class="col-md-6 col-lg-3">
                    <div class="card report-card text-center p-3">
                        <i class="fas fa-file-pdf fa-3x text-danger mb-3"></i>
                        <h6>تقرير التكليفات</h6>
                        <p class="text-muted small">تقرير شامل بجميع التكليفات</p>
                        <button class="btn btn-outline-teal btn-sm" onclick="navigateTo('department-report')">تحميل PDF</button>
                    </div>
                </div>
                <div class="col-md-6 col-lg-3">
                    <div class="card report-card text-center p-3">
                        <i class="fas fa-file-excel fa-3x text-success mb-3"></i>
                        <h6>تقرير الأداء</h6>
                        <p class="text-muted small">أداء الإدارات والموظفين</p>
                        <button class="btn btn-outline-teal btn-sm" onclick="navigateTo('department-report')">تحميل PDF</button>
                    </div>
                </div>
                <div class="col-md-6 col-lg-3">
                    <div class="card report-card text-center p-3">
                        <i class="fas fa-file-alt fa-3x text-primary mb-3"></i>
                        <h6>تقرير المهام المتأخرة</h6>
                        <p class="text-muted small">المهام المتأخرة والحرجة</p>
                        <button class="btn btn-outline-teal btn-sm" onclick="navigateTo('department-report')">تحميل PDF</button>
                    </div>
                </div>
                <div class="col-md-6 col-lg-3">
                    <div class="card report-card text-center p-3">
                        <i class="fas fa-users-cog fa-3x text-teal mb-3"></i>
                        <h6>تقرير المستخدمين</h6>
                        <p class="text-muted small">إحصائيات المستخدمين</p>
                        <button class="btn btn-outline-teal btn-sm" onclick="navigateTo('department-report')">تحميل PDF</button>
                    </div>
                </div>
            </div>
            <div class="card">
                <div class="card-header"><i class="fas fa-building text-teal me-2"></i> تقارير الإدارات</div>
                <div class="card-body">
                    <div class="table-responsive">
                        <table class="table table-hover">
                            <thead>
                                <tr><th>الإدارة</th><th>المهام</th><th>المكتملة</th><th>نسبة الإنجاز</th><th>التقييم</th><th>التقرير</th></tr>
                            </thead>
                            <tbody id="reportsDeptBody"></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة الإشعارات -->
        <!-- ========================================= -->
        <div class="page-section" id="page-notifications">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-bell text-teal me-2"></i> الإشعارات</h4>
                <button class="btn btn-outline-teal btn-sm" id="markAllRead">
                    <i class="fas fa-check-double me-1"></i> تحديد الكل كمقروء
                </button>
            </div>
            <div class="card">
                <div class="card-body p-0" id="notificationsList">
                    <div class="list-group list-group-flush">
                        <div class="list-group-item d-flex justify-content-between align-items-start py-3">
                            <div class="ms-3 me-2">
                                <div class="fw-bold text-primary"><i class="fas fa-plus-circle me-2"></i>مهمة جديدة</div>
                                <p class="mb-1">تم إنشاء تكليف جديد: "تحديث سياسات الجودة" بواسطة أحمد محمد</p>
                                <small class="text-muted">منذ 5 دقائق</small>
                            </div>
                            <span class="badge bg-primary rounded-pill">جديد</span>
                        </div>
                        <div class="list-group-item d-flex justify-content-between align-items-start py-3">
                            <div class="ms-3 me-2">
                                <div class="fw-bold text-warning"><i class="fas fa-exclamation-triangle me-2"></i>اقتراب الموعد</div>
                                <p class="mb-1">مهمة "تقرير المكافحة الشهري" تنتهي بعد 3 أيام</p>
                                <small class="text-muted">منذ ساعة</small>
                            </div>
                            <span class="badge bg-warning rounded-pill">مهم</span>
                        </div>
                        <div class="list-group-item d-flex justify-content-between align-items-start py-3">
                            <div class="ms-3 me-2">
                                <div class="fw-bold text-success"><i class="fas fa-check-circle me-2"></i>مهمة مكتملة</div>
                                <p class="mb-1">تم إكمال مهمة "جرد الأدوية الفصلية" بواسطة ص. نادر</p>
                                <small class="text-muted">منذ 3 ساعات</small>
                            </div>
                            <span class="badge bg-success rounded-pill">مقروء</span>
                        </div>
                        <div class="list-group-item d-flex justify-content-between align-items-start py-3">
                            <div class="ms-3 me-2">
                                <div class="fw-bold text-danger"><i class="fas fa-times-circle me-2"></i>تأخير</div>
                                <p class="mb-1">تم تأخير مهمة "تقرير المكافحة الشهري" إلى 2026-06-23</p>
                                <small class="text-muted">منذ 5 ساعات</small>
                            </div>
                            <span class="badge bg-danger rounded-pill">متأخر</span>
                        </div>
                        <div class="list-group-item d-flex justify-content-between align-items-start py-3">
                            <div class="ms-3 me-2">
                                <div class="fw-bold text-info"><i class="fas fa-comment me-2"></i>تعليق جديد</div>
                                <p class="mb-1">د. سارة أضافت تعليقاً على مهمة "تحديث سياسات الجودة"</p>
                                <small class="text-muted">منذ يوم</small>
                            </div>
                            <span class="badge bg-info rounded-pill">جديد</span>
                        </div>
                        <div class="list-group-item d-flex justify-content-between align-items-start py-3">
                            <div class="ms-3 me-2">
                                <div class="fw-bold text-secondary"><i class="fas fa-user-plus me-2"></i>مستخدم جديد</div>
                                <p class="mb-1">تم إضافة مستخدم جديد: م. نورة إلى إدارة التمريض</p>
                                <small class="text-muted">منذ يومين</small>
                            </div>
                            <span class="badge bg-secondary rounded-pill">مقروء</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة الإعدادات -->
        <!-- ========================================= -->
        <div class="page-section" id="page-settings">
            <h4 class="mb-4"><i class="fas fa-cog text-teal me-2"></i> الإعدادات</h4>
            <div class="row g-4">
                <div class="col-lg-8">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-user-edit me-2"></i> الملف الشخصي</div>
                        <div class="card-body">
                            <form id="profileForm">
                                <div class="row g-3">
                                    <div class="col-md-6">
                                        <label class="form-label fw-semibold">الاسم الكامل</label>
                                        <input type="text" class="form-control" id="profileFullName" />
                                    </div>
                                    <div class="col-md-6">
                                        <label class="form-label fw-semibold">البريد الإلكتروني</label>
                                        <input type="email" class="form-control" id="profileEmail" />
                                    </div>
                                    <div class="col-md-6">
                                        <label class="form-label fw-semibold">رقم الهاتف</label>
                                        <input type="tel" class="form-control" id="profilePhone" />
                                    </div>
                                    <div class="col-md-6">
                                        <label class="form-label fw-semibold">الوظيفة</label>
                                        <input type="text" class="form-control" id="profileJob" />
                                    </div>
                                    <div class="col-md-6">
                                        <label class="form-label fw-semibold">الإدارة</label>
                                        <input type="text" class="form-control" id="profileDept" disabled />
                                    </div>
                                    <div class="col-md-6">
                                        <label class="form-label fw-semibold">الدور</label>
                                        <input type="text" class="form-control" id="profileRole" disabled />
                                    </div>
                                    <div class="col-12">
                                        <button type="submit" class="btn btn-teal"><i class="fas fa-save me-2"></i>حفظ التغييرات</button>
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-shield-alt me-2"></i> الأمان</div>
                        <div class="card-body">
                            <form id="passwordForm">
                                <div class="mb-3">
                                    <label class="form-label fw-semibold">كلمة المرور الحالية</label>
                                    <input type="password" class="form-control" placeholder="أدخل كلمة المرور الحالية" required />
                                </div>
                                <div class="mb-3">
                                    <label class="form-label fw-semibold">كلمة المرور الجديدة</label>
                                    <input type="password" class="form-control" placeholder="أدخل كلمة المرور الجديدة" required />
                                </div>
                                <div class="mb-3">
                                    <label class="form-label fw-semibold">تأكيد كلمة المرور</label>
                                    <input type="password" class="form-control" placeholder="أعد إدخال كلمة المرور" required />
                                </div>
                                <button type="submit" class="btn btn-teal w-100"><i class="fas fa-key me-2"></i>تغيير كلمة المرور</button>
                            </form>
                        </div>
                    </div>
                    <div class="card mt-4">
                        <div class="card-header"><i class="fas fa-sliders-h me-2"></i> إعدادات النظام</div>
                        <div class="card-body">
                            <div class="form-check form-switch mb-2">
                                <input class="form-check-input" type="checkbox" id="notifSwitch" checked />
                                <label class="form-check-label" for="notifSwitch">الإشعارات الفورية</label>
                            </div>
                            <div class="form-check form-switch mb-2">
                                <input class="form-check-input" type="checkbox" id="emailSwitch" checked />
                                <label class="form-check-label" for="emailSwitch">إشعارات البريد الإلكتروني</label>
                            </div>
                            <div class="form-check form-switch mb-2">
                                <input class="form-check-input" type="checkbox" id="darkModeSwitch" />
                                <label class="form-check-label" for="darkModeSwitch">الوضع الليلي</label>
                            </div>
                            <div class="form-check form-switch">
                                <input class="form-check-input" type="checkbox" id="autoBackupSwitch" />
                                <label class="form-check-label" for="autoBackupSwitch">نسخ احتياطي تلقائي</label>
                            </div>
                            <hr />
                            <div class="mb-3">
                                <label class="form-label fw-semibold">رفع شعار النظام</label>
                                <input type="file" class="form-control" id="logoInput" accept="image/*" />
                                <button class="btn btn-teal mt-2" id="saveLogoBtn"><i class="fas fa-upload me-1"></i> رفع الشعار</button>
                            </div>
                            <hr />
                            <button class="btn btn-outline-danger btn-sm w-100" id="clearDataBtn">
                                <i class="fas fa-trash me-1"></i> مسح البيانات
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة تفاصيل التكليف -->
        <!-- ========================================= -->
        <div class="page-section" id="page-task-detail">
            <div class="mb-4">
                <button class="btn btn-outline-teal" onclick="navigateTo('tasks')">
                    <i class="fas fa-arrow-right me-1"></i> العودة للقائمة
                </button>
            </div>
            <div class="row g-4">
                <div class="col-lg-8">
                    <div class="card">
                        <div class="card-body">
                            <div class="d-flex justify-content-between align-items-start">
                                <div>
                                    <h3 class="mb-1">تحديث سياسات الجودة</h3>
                                    <span class="badge bg-warning text-dark">قيد التنفيذ</span>
                                    <span class="badge bg-danger ms-2">حرجة</span>
                                    <span class="badge bg-info ms-2 reminder-badge"><i class="fas fa-bell me-1"></i>تذكير</span>
                                </div>
                                <div class="btn-group">
                                    <button class="btn btn-outline-teal btn-sm"><i class="fas fa-edit"></i> تعديل</button>
                                    <button class="btn btn-outline-danger btn-sm"><i class="fas fa-trash"></i></button>
                                </div>
                            </div>
                            <hr />
                            <div class="row g-3">
                                <div class="col-md-6">
                                    <p class="text-muted mb-1"><i class="fas fa-user me-2"></i>المنشئ</p>
                                    <p class="fw-semibold">أحمد محمد</p>
                                </div>
                                <div class="col-md-6">
                                    <p class="text-muted mb-1"><i class="fas fa-user-check me-2"></i>المسؤول</p>
                                    <p class="fw-semibold">د. سارة</p>
                                </div>
                                <div class="col-md-6">
                                    <p class="text-muted mb-1"><i class="fas fa-building me-2"></i>الإدارة</p>
                                    <p class="fw-semibold">الجودة</p>
                                </div>
                                <div class="col-md-6">
                                    <p class="text-muted mb-1"><i class="fas fa-calendar-alt me-2"></i>تاريخ التسليم</p>
                                    <p class="fw-semibold text-warning">2026-06-21 (3 أيام متبقية)</p>
                                </div>
                                <div class="col-md-6">
                                    <p class="text-muted mb-1"><i class="fas fa-clock me-2"></i>آخر تحديث</p>
                                    <p class="fw-semibold">منذ ساعة</p>
                                </div>
                                <div class="col-md-6">
                                    <p class="text-muted mb-1"><i class="fas fa-tag me-2"></i>الأولوية</p>
                                    <p class="fw-semibold text-danger">حرجة <i class="fas fa-exclamation-circle ms-1"></i></p>
                                </div>
                            </div>
                            <hr />
                            <h6 class="fw-bold"><i class="fas fa-align-left me-2"></i>الوصف</h6>
                            <p>مراجعة وتحديث سياسات الجودة في المستشفى بما يتوافق مع معايير الهيئة العامة للرعاية الصحية. يشمل ذلك تحديث الإجراءات وتدريب الموظفين على السياسات الجديدة.</p>
                        </div>
                    </div>
                    <div class="card mt-4">
                        <div class="card-header"><i class="fas fa-paperclip me-2"></i> المرفقات</div>
                        <div class="card-body">
                            <div class="row g-3">
                                <div class="col-md-4">
                                    <div class="attachment-item p-3 border rounded-3 text-center">
                                        <i class="fas fa-file-pdf fa-2x text-danger mb-2"></i>
                                        <p class="mb-1 fw-semibold">سياسة الجودة.pdf</p>
                                        <small class="text-muted">2.4 MB</small>
                                        <div><button class="btn btn-sm btn-outline-teal mt-2"><i class="fas fa-download"></i> تحميل</button></div>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="attachment-item p-3 border rounded-3 text-center">
                                        <i class="fas fa-file-word fa-2x text-primary mb-2"></i>
                                        <p class="mb-1 fw-semibold">تقرير الجودة.docx</p>
                                        <small class="text-muted">1.1 MB</small>
                                        <div><button class="btn btn-sm btn-outline-teal mt-2"><i class="fas fa-download"></i> تحميل</button></div>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="attachment-item p-3 border rounded-3 text-center">
                                        <i class="fas fa-file-image fa-2x text-success mb-2"></i>
                                        <p class="mb-1 fw-semibold">مخطط العمل.png</p>
                                        <small class="text-muted">856 KB</small>
                                        <div><button class="btn btn-sm btn-outline-teal mt-2"><i class="fas fa-download"></i> تحميل</button></div>
                                    </div>
                                </div>
                            </div>
                            <div class="mt-3">
                                <button class="btn btn-outline-teal btn-sm"><i class="fas fa-upload me-1"></i> رفع ملف جديد</button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-comments me-2"></i> التعليقات</div>
                        <div class="card-body">
                            <div class="comments-list" id="commentsList">
                                <div class="comment-item mb-3">
                                    <div class="d-flex">
                                        <div class="avatar-circle bg-teal text-white me-2" style="width:32px;height:32px;font-size:12px;"><span>أ</span></div>
                                        <div>
                                            <p class="fw-semibold mb-0">أحمد محمد</p>
                                            <small class="text-muted">منذ ساعتين</small>
                                            <p class="mb-0 mt-1">تم إرسال مسودة السياسات الجديدة للمراجعة</p>
                                            <div class="mt-1">
                                                <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-reply"></i> رد</button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="comment-item mb-3">
                                    <div class="d-flex">
                                        <div class="avatar-circle bg-secondary text-white me-2" style="width:32px;height:32px;font-size:12px;"><span>س</span></div>
                                        <div>
                                            <p class="fw-semibold mb-0">د. سارة</p>
                                            <small class="text-muted">منذ 45 دقيقة</small>
                                            <p class="mb-0 mt-1">جاري المراجعة، سأرسل الملاحظات خلال ساعة</p>
                                            <div class="mt-1">
                                                <button class="btn btn-sm btn-outline-secondary"><i class="fas fa-reply"></i> رد</button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <hr />
                            <form id="commentForm">
                                <div class="input-group">
                                    <input type="text" class="form-control" placeholder="أضف تعليقاً..." id="commentInput" />
                                    <button class="btn btn-teal" type="submit">
                                        <i class="fas fa-paper-plane"></i>
                                    </button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ========================================= -->
        <!-- صفحة تقرير الإدارات PDF -->
        <!-- ========================================= -->
        <div class="page-section" id="page-department-report">
            <div class="report-container" id="reportContainer">
                <div class="text-center mb-4 no-print">
                    <button class="btn btn-success btn-lg print-btn" onclick="window.print()">
                        <i class="fas fa-print me-2"></i> طباعة التقرير (PDF) - A4
                    </button>
                    <button class="btn btn-outline-secondary btn-lg ms-2" onclick="navigateTo('dashboard')">
                        <i class="fas fa-arrow-right me-1"></i> العودة للوحة
                    </button>
                </div>
                <div class="report-header">
                    <div class="row align-items-center">
                        <div class="col-md-8">
                            <h1><i class="fas fa-tasks me-2"></i> فريقي</h1>
                            <p style="font-size:1rem; margin:0;">تقرير أداء الإدارات والتقييم الشامل</p>
                            <small>مستشفى القنطرة شرق - نظام إدارة المكتب الفني والتكليفات</small>
                        </div>
                        <div class="col-md-4 text-md-end">
                            <p class="mb-0 fw-bold">التاريخ: <span id="reportDate"></span></p>
                            <small>الإصدار: V2.0 | التقرير دوري</small>
                        </div>
                    </div>
                </div>
                <div class="row g-4 mb-4">
                    <div class="col-md-3">
                        <div class="bg-white p-3 rounded-3 text-center shadow-sm">
                            <h3 class="text-teal fw-bold" id="reportTotalTasks">24</h3>
                            <p class="text-muted mb-0">إجمالي التكليفات</p>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="bg-white p-3 rounded-3 text-center shadow-sm">
                            <h3 class="text-success fw-bold" id="reportCompletedTasks">14</h3>
                            <p class="text-muted mb-0">مكتملة</p>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="bg-white p-3 rounded-3 text-center shadow-sm">
                            <h3 class="text-warning fw-bold" id="reportPendingTasks">8</h3>
                            <p class="text-muted mb-0">قيد التنفيذ</p>
                        </div>
                    </div>
                    <div class="col-md-3">
                        <div class="bg-white p-3 rounded-3 text-center shadow-sm">
                            <h3 class="text-danger fw-bold" id="reportOverdueTasks">2</h3>
                            <p class="text-muted mb-0">متأخرة</p>
                        </div>
                    </div>
                </div>
                <div class="bg-white p-4 rounded-3 shadow-sm mb-4">
                    <div class="row">
                        <div class="col-md-4">
                            <p class="mb-0 text-muted">نسبة الإنجاز الإجمالية</p>
                            <h3 class="fw-bold text-teal" id="reportOverallPercent">78%</h3>
                        </div>
                        <div class="col-md-4">
                            <p class="mb-0 text-muted">متوسط التقييم</p>
                            <h3 class="fw-bold text-warning" id="reportAvgRating">
                                <i class="fas fa-star"></i><i class="fas fa-star"></i><i class="fas fa-star"></i>
                                <i class="fas fa-star"></i><i class="fas fa-star text-muted"></i>
                                <span class="text-dark fs-6"> (3.8/5)</span>
                            </h3>
                        </div>
                        <div class="col-md-4">
                            <p class="mb-0 text-muted">أكثر إدارة تميزاً</p>
                            <h3 class="fw-bold text-success">المكتب الفني <i class="fas fa-crown text-warning"></i></h3>
                        </div>
                    </div>
                </div>
                <h4 class="fw-bold mb-3"><i class="fas fa-building text-teal me-2"></i> تقارير الإدارات</h4>
                <div id="reportDepartmentsList"></div>
                <div class="bg-white p-4 rounded-3 shadow-sm mt-4 text-center">
                    <h5 class="fw-bold"><i class="fas fa-check-circle text-success me-2"></i> ملخص التقرير</h5>
                    <p class="text-muted">تم إنشاء هذا التقرير تلقائياً من نظام "فريقي" لإدارة المكتب الفني والتكليفات بمستشفى القنطرة شرق.</p>
                    <p class="text-muted small mb-0">التقرير يشمل تقييم أداء جميع الإدارات مع نسب الإنجاز والتوصيات.</p>
                    <hr />
                    <small class="text-muted">تم الإنشاء بواسطة: <span id="reportUser">أحمد محمد</span> (مدير المكتب الفني) - <span id="reportDateFooter"></span></small>
                </div>
            </div>
        </div>

    </div><!-- end main-content -->
</div><!-- end app-container -->

<!-- ========================================= -->
<!-- ===== مودالات ===== -->
<!-- ========================================= -->

<!-- مودال تكليف جديد -->
<div class="modal fade" id="newTaskModal" tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title"><i class="fas fa-plus-circle text-teal me-2"></i> تكليف جديد</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <form id="newTaskForm">
                    <div class="row g-3">
                        <div class="col-12">
                            <label class="form-label fw-semibold">عنوان التكليف *</label>
                            <input type="text" class="form-control" placeholder="أدخل عنوان التكليف" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الإدارة *</label>
                            <select class="form-select" required>
                                <option value="">اختر الإدارة</option>
                                <option>المكتب الفني</option><option>الجودة</option>
                                <option>الموارد البشرية</option><option>مكافحة العدوى</option>
                                <option>التمريض</option><option>الصيدلة</option>
                            </select>
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">المسؤول *</label>
                            <select class="form-select" required>
                                <option value="">اختر المسؤول</option>
                                <option>أحمد محمد</option><option>د. سارة</option>
                                <option>أ. خالد</option><option>م. يوسف</option>
                                <option>م. نورة</option><option>ص. نادر</option>
                            </select>
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">تاريخ التسليم *</label>
                            <input type="date" class="form-control" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الأولوية</label>
                            <select class="form-select">
                                <option value="normal">عادية</option>
                                <option value="medium">متوسطة</option>
                                <option value="critical">حرجة</option>
                            </select>
                        </div>
                        <div class="col-12">
                            <label class="form-label fw-semibold">الوصف</label>
                            <textarea class="form-control" rows="3" placeholder="وصف التكليف..."></textarea>
                        </div>
                        <div class="col-12">
                            <label class="form-label fw-semibold">رفع ملفات</label>
                            <input type="file" class="form-control" multiple accept=".pdf,.doc,.docx,.png,.jpg,.jpeg" />
                            <small class="text-muted">يمكنك رفع ملفات متعددة (PDF, Word, صور)</small>
                        </div>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">إلغاء</button>
                <button type="button" class="btn btn-success" id="createTaskBtn"><i class="fas fa-plus me-1"></i> إنشاء التكليف</button>
            </div>
        </div>
    </div>
</div>

<!-- مودال مستخدم جديد -->
<div class="modal fade" id="newUserModal" tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title"><i class="fas fa-user-plus text-teal me-2"></i> مستخدم جديد</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <form id="newUserForm">
                    <div class="row g-3">
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الاسم الكامل *</label>
                            <input type="text" class="form-control" id="userFullName" placeholder="أدخل الاسم الكامل" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">اسم المستخدم *</label>
                            <input type="text" class="form-control" id="userUsername" placeholder="أدخل اسم المستخدم" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">البريد الإلكتروني *</label>
                            <input type="email" class="form-control" id="userEmail" placeholder="example@hospital.local" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">رقم الهاتف</label>
                            <input type="tel" class="form-control" id="userPhone" placeholder="رقم الهاتف" />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الإدارة *</label>
                            <select class="form-select" id="userDepartment" required>
                                <option value="">اختر الإدارة</option>
                                <option>المكتب الفني</option><option>الجودة</option>
                                <option>الموارد البشرية</option><option>مكافحة العدوى</option>
                                <option>التمريض</option><option>المعمل</option>
                                <option>الأشعة</option><option>الصيدلة</option>
                                <option>الهندسة</option>
                            </select>
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الدور *</label>
                            <select class="form-select" id="userRole" required>
                                <option value="">اختر الدور</option>
                                <option>مدير المستشفى</option>
                                <option>مدير المكتب الفني</option>
                                <option>عضو المكتب الفني</option>
                                <option>مدير إدارة</option>
                                <option>مستخدم إدارة</option>
                                <option>مراقب</option>
                            </select>
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">كلمة المرور *</label>
                            <input type="password" class="form-control" id="userPassword" placeholder="أدخل كلمة المرور" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">تأكيد كلمة المرور *</label>
                            <input type="password" class="form-control" id="userConfirmPassword" placeholder="أعد إدخال كلمة المرور" required />
                        </div>
                        <div class="col-12">
                            <label class="form-label fw-semibold">صورة شخصية</label>
                            <input type="file" class="form-control" id="userAvatar" accept="image/*" />
                        </div>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">إلغاء</button>
                <button type="button" class="btn btn-success" id="saveUserBtn"><i class="fas fa-user-plus me-1"></i> إضافة المستخدم</button>
            </div>
        </div>
    </div>
</div>

<!-- مودال إدارة جديدة -->
<div class="modal fade" id="newDeptModal" tabindex="-1">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title"><i class="fas fa-plus-circle text-teal me-2"></i> إدارة جديدة</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <form id="newDeptForm">
                    <div class="mb-3">
                        <label class="form-label fw-semibold">اسم الإدارة *</label>
                        <input type="text" class="form-control" placeholder="أدخل اسم الإدارة" required />
                    </div>
                    <div class="mb-3">
                        <label class="form-label fw-semibold">المدير</label>
                        <select class="form-select">
                            <option value="">اختر المدير</option>
                            <option>أحمد محمد</option><option>د. سارة</option>
                            <option>أ. خالد</option><option>م. يوسف</option>
                            <option>م. نورة</option><option>ص. نادر</option>
                        </select>
                    </div>
                    <div class="mb-3">
                        <label class="form-label fw-semibold">الوصف</label>
                        <textarea class="form-control" rows="3" placeholder="وصف الإدارة"></textarea>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">إلغاء</button>
                <button type="button" class="btn btn-success" id="createDeptBtn"><i class="fas fa-plus me-1"></i> إضافة الإدارة</button>
            </div>
        </div>
    </div>
</div>

<!-- ===== Firebase و JavaScript ===== -->
<script>
// ==========================================
// 1. إعدادات Firebase
// ==========================================
const firebaseConfig = {
    apiKey: "YOUR_API_KEY_HERE",
    authDomain: "fareeqy-hospital.firebaseapp.com",
    projectId: "fareeqy-hospital",
    storageBucket: "fareeqy-hospital.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};

let db = null;
let auth = null;
let storage = null;

try {
    if (typeof firebase !== 'undefined') {
        firebase.initializeApp(firebaseConfig);
        db = firebase.firestore();
        auth = firebase.auth();
        storage = firebase.storage();
        console.log('✅ Firebase initialized successfully');
        
        // تفعيل الإشعارات الفورية
        if ('Notification' in window) {
            Notification.requestPermission();
        }
    }
} catch (e) {
    console.warn('⚠️ Firebase initialization skipped:', e.message);
}

// ==========================================
// 2. المستخدم الحالي وإدارة الصلاحيات
// ==========================================
let currentUser = null;

const DEFAULT_USERS = {
    admin: { 
        username: 'admin', 
        password: 'admin123', 
        fullName: 'أحمد محمد', 
        role: 'مدير المكتب الفني', 
        department: 'المكتب الفني',
        phone: '+20 100 123 4567',
        job: 'مدير المكتب الفني',
        status: 'نشط',
        avatar: null
    },
    ahmed: { 
        username: 'ahmed', 
        password: 'ahmed123', 
        fullName: 'أحمد محمد', 
        role: 'مدير المكتب الفني', 
        department: 'المكتب الفني',
        phone: '+20 100 123 4567',
        job: 'مدير المكتب الفني',
        status: 'نشط',
        avatar: null
    }
};

function getStoredUsers() {
    try {
        const data = localStorage.getItem('hospitalUsers');
        return data ? JSON.parse(data) : DEFAULT_USERS;
    } catch(e) { 
        return DEFAULT_USERS; 
    }
}

function saveStoredUsers(users) {
    localStorage.setItem('hospitalUsers', JSON.stringify(users));
}

function getTasks() {
    try {
        const data = localStorage.getItem('hospitalTasks');
        return data ? JSON.parse(data) : [];
    } catch(e) { 
        return [];
    }
}

function saveTasks(tasks) {
    localStorage.setItem('hospitalTasks', JSON.stringify(tasks));
}

// ==========================================
// 3. وظائف النظام المحسنة
// ==========================================

// تشفير كلمة المرور (SHA-256)
async function hashPassword(password) {
    const encoder = new TextEncoder();
    const data = encoder.encode(password);
    const hash = await crypto.subtle.digest('SHA-256', data);
    return Array.from(new Uint8Array(hash))
        .map(b => b.toString(16).padStart(2, '0'))
        .join('');
}

// تسجيل النشاطات
function logActivity(action, details) {
    const logs = JSON.parse(localStorage.getItem('activityLogs') || '[]');
    logs.unshift({
        timestamp: new Date().toISOString(),
        user: currentUser?.username || 'system',
        action,
        details,
        id: Date.now()
    });
    if (logs.length > 100) logs.pop(); // الاحتفاظ بآخر 100 نشاط
    localStorage.setItem('activityLogs', JSON.stringify(logs));
}

// التحقق من صلاحيات الحذف
function canDelete() {
    return currentUser && ['مدير المكتب الفني', 'مدير المستشفى'].includes(currentUser.role);
}

// ==========================================
// 4. إدارة تسجيل الدخول
// ==========================================
async function login(username, password) {
    const users = getStoredUsers();
    const user = users[username];
    
    if (user) {
        // التحقق من كلمة المرور
        if (user.password === password) {
            currentUser = { username, ...user };
            // إزالة كلمة المرور من الجلسة
            delete currentUser.password;
            localStorage.setItem('currentUser', JSON.stringify(currentUser));
            
            // تسجيل النشاط
            logActivity('تسجيل الدخول', `قام ${user.fullName} بتسجيل الدخول`);
            
            // إرسال إشعار
            if (db) {
                try {
                    await db.collection('activities').add({
                        user: user.fullName,
                        action: 'تسجيل الدخول',
                        timestamp: firebase.firestore.FieldValue.serverTimestamp()
                    });
                } catch(e) { /* ignore */ }
            }
            
            showApp();
            return true;
        }
    }
    return false;
}

function logout() {
    logActivity('تسجيل الخروج', `قام ${currentUser?.fullName || 'مستخدم'} بتسجيل الخروج`);
    currentUser = null;
    localStorage.removeItem('currentUser');
    document.getElementById('page-login').style.display = 'flex';
    document.getElementById('app-container').style.display = 'none';
    
    // إخفاء كرة التحميل
    document.getElementById('floatLoading').classList.remove('show');
}

function showApp() {
    document.getElementById('page-login').style.display = 'none';
    document.getElementById('app-container').style.display = 'block';
    updateUI();
    applyPermissions();
    fetchUsersList();
    loadTasks();
    loadDepartments();
    updateDashboardStats();
    updateReportsData();
    showNotification('مرحباً بك في نظام فريقي!', 'success');
}

function updateUI() {
    if (!currentUser) return;
    
    // تحديث معلومات المستخدم
    document.getElementById('sidebarUserName').textContent = currentUser.fullName || currentUser.username;
    document.getElementById('sidebarUserRole').textContent = currentUser.role || 'مستخدم';
    document.getElementById('sidebarUserInitial').textContent = (currentUser.fullName || currentUser.username).charAt(0);
    document.getElementById('dashboardUserName').textContent = currentUser.fullName || currentUser.username;
    document.getElementById('reportUser').textContent = currentUser.fullName || currentUser.username;
    
    // تحديث التاريخ
    const now = new Date().toLocaleDateString('ar-EG', {
        year: 'numeric', month: 'long', day: 'numeric'
    });
    document.querySelectorAll('#currentDate, #reportDate, #reportDateFooter').forEach(el => {
        if (el) el.textContent = now;
    });
    
    // تحديث ملف التعريف
    if (document.getElementById('profileFullName')) {
        document.getElementById('profileFullName').value = currentUser.fullName || '';
        document.getElementById('profileEmail').value = currentUser.username + '@hospital.local';
        document.getElementById('profilePhone').value = currentUser.phone || '';
        document.getElementById('profileJob').value = currentUser.job || '';
        document.getElementById('profileDept').value = currentUser.department || '';
        document.getElementById('profileRole').value = currentUser.role || '';
    }
    
    // تطبيق الشعار المحفوظ
    applySavedLogo();
    
    // تطبيق الوضع الليلي
    const darkMode = localStorage.getItem('darkMode') === 'true';
    if (darkMode) {
        document.documentElement.setAttribute('data-theme', 'dark');
        document.getElementById('darkModeSwitch').checked = true;
    }
}

function applyPermissions() {
    const isAdmin = canDelete();
    
    // إخفاء أزرار الحذف لغير المديرين
    document.querySelectorAll('.btn-outline-danger, .btn-danger, [data-delete]').forEach(btn => {
        if (!isAdmin) {
            btn.style.display = 'none';
        }
    });
    
    // إخفاء خيارات الحذف في القوائم
    document.querySelectorAll('.delete-option').forEach(el => {
        if (!isAdmin) el.style.display = 'none';
    });
}

// ==========================================
// 5. إدارة المستخدمين
// ==========================================
async function fetchUsersList() {
    const usersBody = document.getElementById('usersBody');
    if (!usersBody) return;

    // محاولة جلب المستخدمين من Firebase
    let users = getStoredUsers();
    
    if (db) {
        try {
            const snapshot = await db.collection('users').get();
            if (!snapshot.empty) {
                const fbUsers = {};
                snapshot.forEach(doc => {
                    const data = doc.data();
                    fbUsers[data.username] = {
                        username: data.username,
                        fullName: data.fullName,
                        role: data.role,
                        department: data.department,
                        phone: data.phone || '',
                        job: data.job || '',
                        status: data.status || 'نشط',
                        avatar: data.avatar || null,
                        email: data.email || data.username + '@hospital.local'
                    };
                });
                // دمج مع المستخدمين المحليين
                const localUsers = getStoredUsers();
                Object.keys(localUsers).forEach(key => {
                    if (!fbUsers[key]) {
                        fbUsers[key] = localUsers[key];
                    }
                });
                users = fbUsers;
                saveStoredUsers(users);
            }
        } catch(e) {
            console.warn('⚠️ Could not fetch users from Firebase:', e);
        }
    }

    const userList = Object.keys(users).map(key => ({
        id: key,
        ...users[key],
        email: users[key].email || key + '@hospital.local'
    }));

    if (userList.length === 0) {
        usersBody.innerHTML = `<tr><td colspan="7" class="text-center">لا يوجد مستخدمون</td></tr>`;
        return;
    }

    usersBody.innerHTML = '';
    userList.forEach((user, idx) => {
        const statusBadge = user.status === 'نشط' ? 'bg-success' : 
                           user.status === 'معلق' ? 'bg-warning text-dark' : 'bg-secondary';
        const isAdmin = canDelete();
        usersBody.innerHTML += `
            <tr>
                <td>${idx + 1}</td>
                <td>
                    <div class="d-flex align-items-center">
                        <div class="avatar-circle me-2" style="width:32px;height:32px;font-size:12px;">
                            <span>${(user.fullName || user.username).charAt(0)}</span>
                        </div>
                        <strong>${user.fullName || user.username}</strong>
                    </div>
                </td>
                <td>${user.email || user.username + '@hospital.local'}</td>
                <td>${user.department || 'غير محدد'}</td>
                <td>${user.role || 'مستخدم'}</td>
                <td><span class="badge ${statusBadge}">${user.status}</span></td>
                <td>
                    <button class="btn btn-sm btn-outline-teal" onclick="viewUser('${user.username}')"><i class="fas fa-eye"></i></button>
                    <button class="btn btn-sm btn-outline-secondary" onclick="editUser('${user.username}')"><i class="fas fa-edit"></i></button>
                    ${isAdmin ? `<button class="btn btn-sm btn-outline-danger" onclick="deleteUser('${user.username}')"><i class="fas fa-trash"></i></button>` : ''}
                </td>
            </tr>
        `;
    });

    // تحديث الإحصائيات
    document.getElementById('totalUsers').textContent = userList.length;
    document.getElementById('activeUsers').textContent = userList.filter(u => u.status === 'نشط').length;
    document.getElementById('inactiveUsers').textContent = userList.filter(u => u.status === 'غير نشط').length;
    document.getElementById('suspendedUsers').textContent = userList.filter(u => u.status === 'معلق').length;
}

// وظائف إدارة المستخدمين
function viewUser(username) {
    const users = getStoredUsers();
    const user = users[username];
    if (user) {
        alert(`👤 معلومات المستخدم:\n\nالاسم: ${user.fullName}\nالدور: ${user.role}\nالإدارة: ${user.department}\nالحالة: ${user.status}`);
    }
}

function editUser(username) {
    alert(`✏️ سيتم فتح صفحة تعديل المستخدم: ${username}`);
    // يمكن إضافة مودال تعديل هنا
}

function deleteUser(username) {
    if (!canDelete()) {
        alert('⚠️ ليس لديك صلاحية لحذف المستخدمين');
        return;
    }
    
    if (confirm(`⚠️ هل أنت متأكد من حذف المستخدم "${username}"؟`)) {
        const users = getStoredUsers();
        if (users[username]) {
            delete users[username];
            saveStoredUsers(users);
            
            // حذف من Firebase
            if (db) {
                db.collection('users').where('username', '==', username).get()
                    .then(snapshot => {
                        snapshot.forEach(doc => doc.ref.delete());
                    })
                    .catch(e => console.warn('Firebase delete error:', e));
            }
            
            logActivity('حذف مستخدم', `تم حذف المستخدم ${username}`);
            fetchUsersList();
            showNotification(`✅ تم حذف المستخدم ${username} بنجاح`, 'success');
        }
    }
}

// ==========================================
// 6. إدارة التكليفات
// ==========================================
function loadTasks() {
    const tasks = getTasks();
    const tbody = document.getElementById('tasksTableBody');
    if (!tbody) return;
    
    if (tasks.length === 0) {
        // استخدام البيانات الافتراضية
        const defaultTasks = [
            { id: 1, title: 'تقرير المكافحة الشهري', dept: 'مكافحة العدوى', assignee: 'أ. خالد', dueDate: '2026-06-18', status: 'overdue', priority: 'critical' },
            { id: 2, title: 'تحديث سياسات الجودة', dept: 'الجودة', assignee: 'د. سارة', dueDate: '2026-06-21', status: 'pending', priority: 'critical' },
            { id: 3, title: 'جرد الأدوية الفصلية', dept: 'الصيدلة', assignee: 'ص. نادر', dueDate: '2026-06-23', status: 'pending', priority: 'medium' },
            { id: 4, title: 'اجتماع المكتب الفني الأسبوعي', dept: 'المكتب الفني', assignee: 'أحمد محمد', dueDate: '2026-06-15', status: 'completed', priority: 'normal' },
            { id: 5, title: 'تقييم أداء الموظفين الربعي', dept: 'الموارد البشرية', assignee: 'م. يوسف', dueDate: '2026-06-10', status: 'completed', priority: 'medium' },
            { id: 6, title: 'برنامج تدريب التمريض الجديد', dept: 'التمريض', assignee: 'م. نورة', dueDate: '2026-06-28', status: 'pending', priority: 'medium' }
        ];
        saveTasks(defaultTasks);
        return loadTasks();
    }
    
    const statusMap = {
        'pending': { class: 'bg-warning text-dark', text: 'قيد التنفيذ' },
        'completed': { class: 'bg-success', text: 'مكتملة' },
        'overdue': { class: 'bg-danger', text: 'متأخرة' }
    };
    const priorityMap = {
        'critical': { class: 'bg-danger', text: 'حرجة' },
        'medium': { class: 'bg-warning text-dark', text: 'متوسطة' },
        'normal': { class: 'bg-success', text: 'عادية' }
    };
    
    const isAdmin = canDelete();
    tbody.innerHTML = tasks.map((task, idx) => `
        <tr data-status="${task.status}" data-dept="${task.dept.toLowerCase().replace(' ', '')}">
            <td>${idx + 1}</td>
            <td>${task.title}</td>
            <td>${task.dept}</td>
            <td>${task.assignee}</td>
            <td>${task.dueDate}</td>
            <td><span class="badge ${statusMap[task.status]?.class || 'bg-secondary'}">${statusMap[task.status]?.text || task.status}</span></td>
            <td><span class="badge ${priorityMap[task.priority]?.class || 'bg-secondary'}">${priorityMap[task.priority]?.text || task.priority}</span></td>
            <td>
                <button class="btn btn-sm btn-outline-teal" onclick="navigateTo('task-detail')"><i class="fas fa-eye"></i></button>
                <button class="btn btn-sm btn-outline-secondary" onclick="editTask(${task.id})"><i class="fas fa-edit"></i></button>
                ${isAdmin ? `<button class="btn btn-sm btn-outline-danger" onclick="deleteTaskById(${task.id})"><i class="fas fa-trash"></i></button>` : ''}
            </td>
        </tr>
    `).join('');
    
    // تحديث العدد
    document.getElementById('taskBadge').textContent = tasks.filter(t => t.status === 'pending').length;
}

function editTask(id) {
    alert(`✏️ سيتم فتح صفحة تعديل التكليف #${id}`);
}

function deleteTaskById(id) {
    if (!canDelete()) {
        alert('⚠️ ليس لديك صلاحية لحذف التكليفات');
        return;
    }
    
    if (confirm(`⚠️ هل أنت متأكد من حذف التكليف #${id}؟`)) {
        let tasks = getTasks();
        tasks = tasks.filter(t => t.id !== id);
        saveTasks(tasks);
        
        logActivity('حذف تكليف', `تم حذف التكليف #${id}`);
        loadTasks();
        updateDashboardStats();
        showNotification(`✅ تم حذف التكليف بنجاح`, 'success');
    }
}

// ==========================================
// 7. إدارة الإدارات
// ==========================================
function loadDepartments() {
    const container = document.getElementById('departmentsContainer');
    if (!container) return;
    
    const departments = [
        { name: 'المكتب الفني', icon: 'fa-star', color: 'teal', members: 3, manager: 'أحمد محمد', tasks: 12, progress: 92 },
        { name: 'الجودة', icon: 'fa-check-circle', color: 'success', members: 2, manager: 'د. سارة', tasks: 8, progress: 78 },
        { name: 'الموارد البشرية', icon: 'fa-users', color: 'warning', members: 2, manager: 'م. يوسف', tasks: 6, progress: 65 },
        { name: 'مكافحة العدوى', icon: 'fa-shield-virus', color: 'danger', members: 2, manager: 'أ. خالد', tasks: 5, progress: 43 },
        { name: 'التمريض', icon: 'fa-user-md', color: 'info', members: 3, manager: 'م. نورة', tasks: 10, progress: 70 },
        { name: 'الصيدلة', icon: 'fa-pills', color: 'secondary', members: 1, manager: 'ص. نادر', tasks: 4, progress: 55 }
    ];
    
    container.innerHTML = departments.map(dept => `
        <div class="col-lg-4 col-md-6">
            <div class="card department-card">
                <div class="card-body">
                    <div class="d-flex justify-content-between align-items-start">
                        <div>
                            <h5 class="fw-bold"><i class="fas ${dept.icon} text-${dept.color} me-1"></i> ${dept.name}</h5>
                            <p class="text-muted mb-1">${dept.members} أعضاء</p>
                            <p class="text-muted small">المدير: ${dept.manager}</p>
                        </div>
                        <div class="badge bg-${dept.color} p-2"><i class="fas fa-check-circle"></i> ${dept.tasks} مهمة</div>
                    </div>
                    <div class="mt-3">
                        <div class="progress">
                            <div class="progress-bar bg-${dept.color}" style="width: ${dept.progress}%;"></div>
                        </div>
                        <small class="text-muted">نسبة الإنجاز ${dept.progress}%</small>
                    </div>
                    <button class="btn btn-sm btn-outline-teal mt-2" onclick="navigateTo('department-report')">
                        <i class="fas fa-file-pdf me-1"></i> تقرير PDF
                    </button>
                </div>
            </div>
        </div>
    `).join('');
}

// ==========================================
// 8. تحديث لوحة التحكم
// ==========================================
function updateDashboardStats() {
    const tasks = getTasks();
    const total = tasks.length;
    const pending = tasks.filter(t => t.status === 'pending').length;
    const completed = tasks.filter(t => t.status === 'completed').length;
    const overdue = tasks.filter(t => t.status === 'overdue').length;
    
    document.getElementById('statTotalTasks').textContent = total;
    document.getElementById('statPendingTasks').textContent = pending;
    document.getElementById('statCompletedTasks').textContent = completed;
    document.getElementById('statOverdueTasks').textContent = overdue;
    
    // تحديث النسب
    const completionRate = total > 0 ? Math.round((completed / total) * 100) : 0;
    document.getElementById('trendTotal').textContent = `${completionRate}%`;
    document.getElementById('trendCompleted').textContent = `${completionRate}%`;
    document.getElementById('trendPending').textContent = pending;
    
    // تحديث KPI
    const avgTime = total > 0 ? (4.2 + (overdue * 0.5)).toFixed(1) : 0;
    document.getElementById('kpiAvgTime').textContent = `${avgTime} يوم`;
    document.getElementById('kpiTotalTasks').textContent = total;
    document.getElementById('kpiTotalTasks2').textContent = total;
    document.getElementById('kpiOverdueCount').textContent = overdue;
    document.getElementById('kpiOverdueRate').textContent = total > 0 ? `${Math.round((overdue / total) * 100)}%` : '0%';
    
    const weeklyRate = Math.round(completed / 4);
    document.getElementById('kpiWeeklyRate').textContent = `${weeklyRate || 0} مهام/أسبوع`;
    
    // التقييم العام
    const overallRating = completionRate >= 90 ? 'ممتاز' : 
                         completionRate >= 70 ? 'جيد جداً' : 
                         completionRate >= 50 ? 'جيد' : 'يحتاج تحسين';
    document.getElementById('kpiOverall').textContent = `${overallRating} (${completionRate}%)`;
    document.getElementById('kpiOverallBar').style.width = `${completionRate}%`;
}

// ==========================================
// 9. تحديث التقارير
// ==========================================
function updateReportsData() {
    const tasks = getTasks();
    const departments = [
        { name: 'المكتب الفني', icon: 'fa-star', color: 'teal', tasks: 12, done: 11 },
        { name: 'الجودة', icon: 'fa-check-circle', color: 'success', tasks: 8, done: 6 },
        { name: 'الموارد البشرية', icon: 'fa-users', color: 'warning', tasks: 6, done: 4 },
        { name: 'مكافحة العدوى', icon: 'fa-shield-virus', color: 'danger', tasks: 5, done: 2 },
        { name: 'التمريض', icon: 'fa-user-md', color: 'info', tasks: 10, done: 7 },
        { name: 'الصيدلة', icon: 'fa-pills', color: 'secondary', tasks: 4, done: 2 }
    ];
    
    const tbody = document.getElementById('reportsDeptBody');
    if (tbody) {
        tbody.innerHTML = departments.map(dept => {
            const percent = dept.tasks > 0 ? Math.round((dept.done / dept.tasks) * 100) : 0;
            const stars = Math.round(percent / 20);
            const starHtml = Array(5).fill(0).map((_, i) => 
                `<i class="fas fa-star ${i < stars ? 'text-warning' : 'text-muted'}"></i>`
            ).join('');
            const color = percent >= 80 ? 'teal' : percent >= 60 ? 'success' : percent >= 40 ? 'warning' : 'danger';
            return `
                <tr>
                    <td><i class="fas ${dept.icon} text-${dept.color} me-1"></i> ${dept.name}</td>
                    <td>${dept.tasks}</td>
                    <td>${dept.done}</td>
                    <td><span class="badge bg-${color}">${percent}%</span></td>
                    <td>${starHtml}</td>
                    <td><button class="btn btn-sm btn-outline-teal" onclick="navigateTo('department-report')"><i class="fas fa-file-pdf me-1"></i> PDF</button></td>
                </tr>
            `;
        }).join('');
    }
    
    // تحديث تقرير الإدارات
    const reportList = document.getElementById('reportDepartmentsList');
    if (reportList) {
        reportList.innerHTML = departments.map(dept => {
            const percent = dept.tasks > 0 ? Math.round((dept.done / dept.tasks) * 100) : 0;
            const rating = percent >= 90 ? 'ممتاز' : percent >= 70 ? 'جيد جداً' : percent >= 50 ? 'متوسط' : 'ضعيف';
            const ratingClass = percent >= 90 ? 'rating-excellent' : percent >= 70 ? 'rating-good' : percent >= 50 ? 'rating-average' : 'rating-poor';
            const stars = Math.round(percent / 20);
            const starHtml = Array(5).fill(0).map((_, i) => 
                `<i class="fas fa-star ${i < stars ? 'text-warning' : 'text-muted'}"></i>`
            ).join('');
            
            return `
                <div class="department-report-card" style="border-right-color: var(--${dept.color});">
                    <div class="row">
                        <div class="col-md-6">
                            <div class="dept-name"><i class="fas ${dept.icon} text-${dept.color} me-2"></i> ${dept.name}</div>
                            <p class="text-muted mb-2">مدير الإدارة: ${dept.manager}</p>
                            <div class="rating-stars">${starHtml} <span class="badge ${ratingClass} me-2">${rating}</span></div>
                        </div>
                        <div class="col-md-6">
                            <div class="stat-item d-flex justify-content-between"><span>إجمالي المهام</span><span class="fw-semibold">${dept.tasks}</span></div>
                            <div class="stat-item d-flex justify-content-between"><span>المهام المكتملة</span><span class="fw-semibold text-success">${dept.done}</span></div>
                            <div class="stat-item d-flex justify-content-between"><span>نسبة الإنجاز</span><span class="fw-semibold text-${dept.color}">${percent}%</span></div>
                            <div class="stat-item d-flex justify-content-between"><span>التقييم النهائي</span><span class="fw-bold text-${dept.color}">${'★'.repeat(Math.round(percent/20))}${'☆'.repeat(5 - Math.round(percent/20))} (${rating})</span></div>
                        </div>
                    </div>
                    <div class="mt-2"><div class="progress"><div class="progress-bar bg-${dept.color}" style="width: ${percent}%;"></div></div></div>
                    <small class="text-muted mt-2 d-block">ملاحظات: ${percent >= 90 ? 'أداء متميز، جميع المهام أنجزت قبل الموعد' : 
                        percent >= 70 ? 'أداء جيد، يحتاج إلى تحسين سرعة الإنجاز' : 
                        percent >= 50 ? 'يحتاج إلى تحسين الأداء وزيادة الإنتاجية' : 
                        'يحتاج إلى تدخل عاجل لتحسين الأداء وتجنب التأخير'}</small>
                </div>
            `;
        }).join('');
    }
}

// ==========================================
// 10. الإشعارات
// ==========================================
function showNotification(message, type = 'info') {
    // إشعار داخل التطبيق
    const notifDiv = document.createElement('div');
    notifDiv.className = `alert alert-${type} alert-dismissible fade show position-fixed top-0 start-50 translate-middle-x mt-3`;
    notifDiv.style.zIndex = '9999';
    notifDiv.style.maxWidth = '500px';
    notifDiv.style.boxShadow = 'var(--shadow-lg)';
    notifDiv.innerHTML = `
        ${message}
        <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    `;
    document.body.appendChild(notifDiv);
    
    // إشعار المتصفح
    if ('Notification' in window && Notification.permission === 'granted') {
        new Notification('📋 فريقي', { body: message, icon: 'data:image/svg+xml,...' });
    }
    
    // إزالة الإشعار بعد 5 ثواني
    setTimeout(() => {
        if (notifDiv.parentNode) {
            notifDiv.remove();
        }
    }, 5000);
}

// ==========================================
// 11. التعامل مع رفع الشعار
// ==========================================
function handleLogoUpload() {
    const logoInput = document.getElementById('logoInput');
    const saveLogoBtn = document.getElementById('saveLogoBtn');
    if (!logoInput || !saveLogoBtn) return;

    saveLogoBtn.addEventListener('click', function(e) {
        e.preventDefault();
        const file = logoInput.files[0];
        if (file) {
            const reader = new FileReader();
            reader.onload = function(event) {
                localStorage.setItem('systemLogo', event.target.result);
                showNotification('🔄 تم تحديث لوجو النظام بنجاح', 'success');
                applySavedLogo();
                logActivity('تحديث الشعار', 'تم تغيير شعار النظام');
            };
            reader.readAsDataURL(file);
        } else {
            showNotification('⚠️ الرجاء اختيار ملف صورة أولاً', 'warning');
        }
    });
}

function applySavedLogo() {
    const savedLogo = localStorage.getItem('systemLogo');
    const sidebarHeader = document.querySelector('.sidebar-header');
    if (savedLogo && sidebarHeader) {
        const existingIcon = sidebarHeader.querySelector('h3 i');
        if (existingIcon) {
            const img = document.createElement('img');
            img.src = savedLogo;
            img.alt = 'الشعار';
            img.style.width = '30px';
            img.style.height = '30px';
            img.style.marginLeft = '8px';
            img.style.borderRadius = '4px';
            existingIcon.replaceWith(img);
        }
    }
}

// ==========================================
// 12. التنقل بين الصفحات
// ==========================================
function navigateTo(page) {
    // إخفاء الكل
    document.querySelectorAll('.page-section').forEach(el => el.classList.remove('active'));
    
    // إظهار المطلوب
    const target = document.getElementById('page-' + page);
    if (target) target.classList.add('active');
    
    // تحديث القائمة الجانبية
    document.querySelectorAll('.sidebar .nav-link').forEach(el => el.classList.remove('active'));
    const link = document.querySelector(`.sidebar .nav-link[data-page="${page}"]`);
    if (link) link.classList.add('active');
    
    // غلق القائمة الجانبية في الجوال
    document.getElementById('sidebar').classList.remove('open');
    
    // تحديث البيانات عند التنقل
    if (page === 'dashboard') updateDashboardStats();
    if (page === 'users') fetchUsersList();
    if (page === 'tasks') loadTasks();
    if (page === 'departments') loadDepartments();
    if (page === 'reports') updateReportsData();
}

// ==========================================
// 13. أحداث ومستمعات الصفحة
// ==========================================
document.addEventListener('DOMContentLoaded', function() {
    // التحقق من الجلسة
    const savedUser = localStorage.getItem('currentUser');
    if (savedUser) {
        try {
            currentUser = JSON.parse(savedUser);
            if (currentUser && getStoredUsers()[currentUser.username]) {
                showApp();
                return;
            }
        } catch(e) { /* ignore */ }
    }
    
    // عرض صفحة الدخول
    document.getElementById('page-login').style.display = 'flex';
    document.getElementById('app-container').style.display = 'none';

    // ===== نموذج تسجيل الدخول =====
    document.getElementById('loginForm').addEventListener('submit', async function(e) {
        e.preventDefault();
        const username = document.getElementById('loginUsername').value.trim();
        const password = document.getElementById('loginPassword').value.trim();
        const errorEl = document.getElementById('loginError');
        const successEl = document.getElementById('loginSuccess');
        const btn = document.getElementById('loginBtn');

        btn.innerHTML = '<span class="loading-spinner"></span> جاري التحقق...';
        btn.disabled = true;

        if (await login(username, password)) {
            errorEl.classList.add('d-none');
            successEl.classList.remove('d-none');
            successEl.textContent = '✅ تم تسجيل الدخول بنجاح...';
            setTimeout(() => {
                successEl.classList.add('d-none');
            }, 2000);
        } else {
            errorEl.classList.remove('d-none');
            btn.innerHTML = '<i class="fas fa-sign-in-alt me-2"></i>تسجيل الدخول';
            btn.disabled = false;
        }
    });

    // ===== تبديل إظهار كلمة المرور =====
    document.getElementById('togglePassword').addEventListener('click', function() {
        const pwd = document.getElementById('loginPassword');
        pwd.type = pwd.type === 'password' ? 'text' : 'password';
        this.querySelector('i').classList.toggle('fa-eye');
        this.querySelector('i').classList.toggle('fa-eye-slash');
    });

    // ===== التنقل في القائمة الجانبية =====
    document.querySelectorAll('.sidebar .nav-link[data-page]').forEach(link => {
        link.addEventListener('click', function(e) {
            e.preventDefault();
            navigateTo(this.dataset.page);
        });
    });

    // ===== زر تبديل القائمة الجانبية =====
    document.getElementById('sidebarToggle').addEventListener('click', function() {
        document.getElementById('sidebar').classList.toggle('open');
    });

    // ===== تسجيل الخروج =====
    document.getElementById('logoutBtn').addEventListener('click', function(e) {
        e.preventDefault();
        logout();
    });

    // ===== تحديد الكل كمقروء =====
    document.getElementById('markAllRead').addEventListener('click', function() {
        document.querySelectorAll('#page-notifications .list-group-item .badge').forEach(b => {
            if (b.textContent === 'جديد' || b.textContent === 'مهم' || b.textContent === 'متأخر') {
                b.textContent = 'مقروء';
                b.className = 'badge bg-secondary rounded-pill';
            }
        });
        document.getElementById('notifBadge').textContent = '0';
        showNotification('✅ تم تحديد جميع الإشعارات كمقروءة', 'success');
        logActivity('تحديد الإشعارات', 'تم تحديد جميع الإشعارات كمقروءة');
    });

    // ===== الوضع الليلي =====
    document.getElementById('darkModeSwitch').addEventListener('change', function() {
        if (this.checked) {
            document.documentElement.setAttribute('data-theme', 'dark');
            localStorage.setItem('darkMode', 'true');
        } else {
            document.documentElement.removeAttribute('data-theme');
            localStorage.setItem('darkMode', 'false');
        }
    });

    // ===== رفع الشعار =====
    handleLogoUpload();

    // ===== إنشاء تكليف جديد =====
    document.getElementById('createTaskBtn').addEventListener('click', function() {
        const form = document.getElementById('newTaskForm');
        const inputs = form.querySelectorAll('input, select, textarea');
        let valid = true;
        inputs.forEach(input => {
            if (input.hasAttribute('required') && !input.value.trim()) {
                valid = false;
                input.classList.add('is-invalid');
            } else {
                input.classList.remove('is-invalid');
            }
        });
        
        if (valid) {
            const tasks = getTasks();
            const newTask = {
                id: Date.now(),
                title: form.querySelector('input[type="text"]').value,
                dept: form.querySelector('select').value,
                assignee: form.querySelectorAll('select')[1].value,
                dueDate: form.querySelector('input[type="date"]').value,
                status: 'pending',
                priority: form.querySelectorAll('select')[2]?.value || 'normal'
            };
            tasks.push(newTask);
            saveTasks(tasks);
            
            // حفظ في Firebase
            if (db) {
                db.collection('tasks').add({
                    ...newTask,
                    createdAt: firebase.firestore.FieldValue.serverTimestamp()
                }).catch(e => console.warn('Firebase save error:', e));
            }
            
            logActivity('إنشاء تكليف', `تم إنشاء تكليف جديد: ${newTask.title}`);
            const modal = bootstrap.Modal.getInstance(document.getElementById('newTaskModal'));
            if (modal) modal.hide();
            loadTasks();
            updateDashboardStats();
            showNotification('✅ تم إنشاء التكليف بنجاح', 'success');
            form.reset();
        } else {
            showNotification('⚠️ يرجى ملء جميع الحقول المطلوبة', 'warning');
        }
    });

    // ===== إضافة مستخدم =====
    document.getElementById('saveUserBtn').addEventListener('click', async function() {
        const fullName = document.getElementById('userFullName').value.trim();
        const username = document.getElementById('userUsername').value.trim();
        const email = document.getElementById('userEmail').value.trim();
        const password = document.getElementById('userPassword').value;
        const confirmPassword = document.getElementById('userConfirmPassword').value;
        const role = document.getElementById('userRole').value;
        const dept = document.getElementById('userDepartment').value;
        const phone = document.getElementById('userPhone').value.trim();

        if (!fullName || !username || !email || !password || !role || !dept) {
            showNotification('⚠️ يرجى ملء جميع الحقول الإلزامية', 'warning');
            return;
        }
        if (password !== confirmPassword) {
            showNotification('⚠️ كلمتا المرور غير متطابقتين!', 'danger');
            return;
        }
        if (password.length < 6) {
            showNotification('⚠️ يجب ألا تقل كلمة المرور عن 6 خانات', 'warning');
            return;
        }

        const users = getStoredUsers();
        if (users[username]) {
            showNotification('⚠️ اسم المستخدم موجود بالفعل!', 'warning');
            return;
        }

        // تشفير كلمة المرور
        const hashedPassword = await hashPassword(password);
        
        users[username] = { 
            username, 
            password: hashedPassword,
            fullName, 
            role, 
            department: dept,
            phone: phone || '',
            job: role,
            status: 'نشط',
            email: email,
            avatar: null
        };
        saveStoredUsers(users);

        // حفظ في Firebase
        if (db) {
            try {
                await db.collection('users').add({
                    username,
                    fullName,
                    email,
                    role,
                    department: dept,
                    phone: phone || '',
                    status: 'نشط',
                    createdAt: firebase.firestore.FieldValue.serverTimestamp()
                });
            } catch(e) {
                console.warn('Firebase save error:', e);
            }
        }

        logActivity('إضافة مستخدم', `تم إضافة المستخدم ${fullName}`);
        const modal = bootstrap.Modal.getInstance(document.getElementById('newUserModal'));
        if (modal) modal.hide();
        fetchUsersList();
        showNotification(`✅ تم إضافة المستخدم ${fullName} بنجاح`, 'success');
        document.getElementById('newUserForm').reset();
    });

    // ===== إضافة إدارة =====
    document.getElementById('createDeptBtn').addEventListener('click', function() {
        const form = document.getElementById('newDeptForm');
        const name = form.querySelector('input').value.trim();
        if (!name) {
            showNotification('⚠️ يرجى إدخال اسم الإدارة', 'warning');
            return;
        }
        
        // حفظ الإدارة في localStorage
        const depts = JSON.parse(localStorage.getItem('departments') || '[]');
        depts.push({
            id: Date.now(),
            name: name,
            manager: form.querySelector('select').value,
            description: form.querySelector('textarea').value
        });
        localStorage.setItem('departments', JSON.stringify(depts));
        
        logActivity('إضافة إدارة', `تم إضافة الإدارة ${name}`);
        const modal = bootstrap.Modal.getInstance(document.getElementById('newDeptModal'));
        if (modal) modal.hide();
        loadDepartments();
        showNotification(`✅ تم إضافة الإدارة ${name} بنجاح`, 'success');
        form.reset();
    });

    // ===== التعليقات =====
    document.getElementById('commentForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const input = document.getElementById('commentInput');
        const text = input.value.trim();
        if (!text) return;
        
        const list = document.getElementById('commentsList');
        const newComment = document.createElement('div');
        newComment.className = 'comment-item mb-3';
        newComment.innerHTML = `
            <div class="d-flex">
                <div class="avatar-circle bg-teal text-white me-2" style="width:32px;height:32px;font-size:12px;">
                    <span>${currentUser?.fullName?.charAt(0) || 'م'}</span>
                </div>
                <div>
                    <p class="fw-semibold mb-0">${currentUser?.fullName || 'مستخدم'}</p>
                    <small class="text-muted">الآن</small>
                    <p class="mb-0 mt-1">${text}</p>
                </div>
            </div>
        `;
        list.appendChild(newComment);
        input.value = '';
        showNotification('✅ تم إضافة تعليقك', 'success');
    });

    // ===== مسح البيانات =====
    document.getElementById('clearDataBtn').addEventListener('click', function() {
        if (confirm('⚠️ هل أنت متأكد من مسح جميع البيانات؟ هذا الإجراء لا يمكن التراجع عنه!')) {
            if (confirm('🔴 تأكيد نهائي: مسح جميع البيانات؟')) {
                localStorage.clear();
                showNotification('🗑️ تم مسح جميع البيانات', 'warning');
                setTimeout(() => location.reload(), 1500);
            }
        }
    });

    // ===== بحث وتصفية التكليفات =====
    document.getElementById('searchTask')?.addEventListener('keyup', filterTasks);
    document.getElementById('filterStatus')?.addEventListener('change', filterTasks);
    document.getElementById('filterDept')?.addEventListener('change', filterTasks);
    document.getElementById('clearFilters')?.addEventListener('click', function() {
        document.getElementById('searchTask').value = '';
        document.getElementById('filterStatus').value = '';
        document.getElementById('filterDept').value = '';
        filterTasks();
    });

    function filterTasks() {
        const search = document.getElementById('searchTask')?.value.toLowerCase() || '';
        const status = document.getElementById('filterStatus')?.value || '';
        const dept = document.getElementById('filterDept')?.value || '';
        const rows = document.querySelectorAll('#tasksTableBody tr');
        let visible = 0;
        
        rows.forEach(row => {
            const text = row.textContent.toLowerCase();
            const rowStatus = row.dataset.status || '';
            const rowDept = row.dataset.dept || '';
            let show = true;
            
            if (search && !text.includes(search)) show = false;
            if (status && rowStatus !== status) show = false;
            if (dept && rowDept !== dept) show = false;
            
            row.style.display = show ? '' : 'none';
            if (show) visible++;
        });
        
        if (visible === 0 && rows.length > 0) {
            // إظهار رسالة "لا توجد نتائج"
            const firstRow = rows[0];
            if (!document.getElementById('noResults')) {
                const noResult = document.createElement('tr');
                noResult.id = 'noResults';
                noResult.innerHTML = `<td colspan="8" class="text-center py-3 text-muted">🔍 لا توجد تكليفات تطابق البحث</td>`;
                firstRow.parentNode.insertBefore(noResult, firstRow);
            }
        } else {
            const noResult = document.getElementById('noResults');
            if (noResult) noResult.remove();
        }
    }

    // ===== إغلاق القائمة الجانبية عند النقر خارجها =====
    document.addEventListener('click', function(e) {
        const sidebar = document.getElementById('sidebar');
        const toggle = document.getElementById('sidebarToggle');
        if (window.innerWidth < 768 && sidebar.classList.contains('open') &&
            !sidebar.contains(e.target) && !toggle.contains(e.target)) {
            sidebar.classList.remove('open');
        }
    });

    // ===== تحديث تلقائي كل 30 ثانية =====
    setInterval(() => {
        if (currentUser) {
            updateDashboardStats();
            const notifBadge = document.getElementById('notifBadge');
            const current = parseInt(notifBadge.textContent) || 0;
            // محاكاة إشعارات جديدة عشوائية
            if (Math.random() > 0.7) {
                notifBadge.textContent = current + 1;
                showNotification('📢 لديك إشعار جديد', 'info');
            }
        }
    }, 30000);

    // ===== تهيئة الرسم البياني =====
    setTimeout(() => {
        const ctx = document.getElementById('taskPieChart')?.getContext('2d');
        if (ctx) {
            new Chart(ctx, {
                type: 'pie',
                data: {
                    labels: ['مكتملة', 'قيد التنفيذ', 'متأخرة'],
                    datasets: [{
                        data: [14, 8, 2],
                        backgroundColor: ['#43A047', '#FB8C00', '#E53935'],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: { position: 'bottom', labels: { font: { size: 12 } } }
                    }
                }
            });
        }
    }, 100);

    console.log('🚀 نظام فريقي V2.0 جاهز للتشغيل!');
});
</script>
</body>
</html>
