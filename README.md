<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes" />
    <meta name="theme-color" content="#00897B" />
    <title>فريقي - نظام إدارة المكتب الفني</title>
    
    <!-- Bootstrap RTL -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css" />
    <!-- Font Awesome 6 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-storage-compat.js"></script>
    
    <style>
        /* ===== متغيرات ===== */
        :root {
            --teal: #00897B;
            --teal-dark: #00695C;
            --teal-light: #e0f2f1;
            --sidebar-width: 260px;
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
        [data-theme="dark"] .list-group-item {
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

        /* ===== عام ===== */
        * { box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', -apple-system, sans-serif;
            background: #f4f6f8;
            color: #333;
            margin: 0;
            padding: 0;
            transition: var(--transition);
        }
        .page-section { display: none; animation: fadeSlide 0.4s ease; }
        .page-section.active { display: block; }
        @keyframes fadeSlide {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ===== ألوان ===== */
        .text-teal { color: var(--teal) !important; }
        .bg-teal { background: var(--teal) !important; color: #fff; }
        .btn-teal {
            background: var(--teal);
            color: #fff;
            border: none;
            transition: var(--transition);
        }
        .btn-teal:hover {
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

        /* ===== تسجيل الدخول ===== */
        .login-page {
            background: linear-gradient(135deg, #00897B 0%, #004D40 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        .login-page .card {
            border-radius: var(--radius);
            animation: fadeUp 0.6s ease;
            background: rgba(255,255,255,0.95);
        }
        [data-theme="dark"] .login-page .card {
            background: rgba(30,30,30,0.95);
        }
        @keyframes fadeUp {
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
        }

        /* ===== القائمة الجانبية ===== */
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
            transition: transform 0.3s ease;
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
        .sidebar .nav-link i { width: 20px; text-align: center; }
        .sidebar .badge { margin-right: auto; }
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
            background: var(--teal);
            color: #fff;
        }

        /* ===== المحتوى ===== */
        .main-content {
            margin-right: var(--sidebar-width);
            padding: 28px 30px;
            min-height: 100vh;
            transition: var(--transition);
        }
        .welcome-banner {
            background: linear-gradient(135deg, #00897B, #004D40);
            border-radius: var(--radius);
            padding: 30px;
            color: #fff;
            position: relative;
            overflow: hidden;
        }

        /* ===== بطاقات ===== */
        .stat-card {
            border-radius: var(--radius);
            padding: 24px;
            color: #fff;
            position: relative;
            overflow: hidden;
            transition: var(--transition);
            cursor: pointer;
        }
        .stat-card:hover { transform: translateY(-4px); box-shadow: var(--shadow-lg); }
        .stat-card .number { font-size: 2.6rem; font-weight: 700; line-height: 1; }
        .stat-card .label { font-size: 0.9rem; opacity: 0.9; margin-top: 4px; }
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

        .card {
            border: none;
            border-radius: var(--radius);
            box-shadow: var(--shadow-sm);
            transition: var(--transition);
        }
        .card:hover { box-shadow: var(--shadow-md); }
        .card-header {
            background: transparent;
            border-bottom: 1px solid rgba(0,0,0,0.06);
            padding: 16px 20px;
            font-weight: 600;
        }

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

        .department-card { transition: var(--transition); cursor: pointer; }
        .department-card:hover {
            transform: translateY(-6px);
            box-shadow: var(--shadow-lg) !important;
        }

        .report-card {
            cursor: pointer;
            transition: var(--transition);
            padding: 24px;
        }
        .report-card:hover {
            transform: translateY(-6px);
            box-shadow: var(--shadow-lg) !important;
        }

        /* ===== استجابة ===== */
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
        @media print {
            .no-print { display: none !important; }
            .sidebar { display: none !important; }
            .main-content { margin: 0 !important; padding: 20px !important; }
            .page-section { display: block !important; }
            .page-section:not(.active) { display: none !important; }
            #page-department-report { display: block !important; }
            body { background: white !important; }
        }

        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(0,0,0,0.1);
            border-radius: 50%;
            border-top-color: var(--teal);
            animation: spin 0.6s ease-in-out infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
    </style>
</head>
<body>

<!-- ===== صفحة تسجيل الدخول ===== -->
<div id="page-login" class="login-page">
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-5 col-lg-4">
                <div class="card shadow-lg border-0 rounded-4 p-4">
                    <div class="card-body">
                        <div class="text-center mb-4">
                            <div class="logo-circle"><i class="fas fa-tasks fa-3x text-teal"></i></div>
                            <h1 class="fw-bold text-success">فريقي</h1>
                            <p class="text-muted">نظام إدارة المكتب الفني</p>
                            <p class="text-muted small">مستشفى القنطرة شرق</p>
                        </div>
                        <div id="loginError" class="alert alert-danger d-none" role="alert">
                            <i class="fas fa-exclamation-circle me-2"></i> اسم المستخدم أو كلمة المرور غير صحيحة
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

<!-- ===== التطبيق الرئيسي ===== -->
<div id="app-container" style="display:none;">

    <!-- القائمة الجانبية -->
    <nav class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <h3 class="text-success fw-bold"><i class="fas fa-tasks text-teal"></i> فريقي</h3>
            <small class="text-muted">مستشفى القنطرة شرق</small>
        </div>
        <ul class="nav flex-column">
            <li class="nav-item"><button class="nav-link active" data-page="dashboard"><i class="fas fa-chart-pie"></i> لوحة التحكم</button></li>
            <li class="nav-item"><button class="nav-link" data-page="tasks"><i class="fas fa-tasks"></i> التكليفات <span class="badge bg-danger rounded-pill ms-2" id="taskBadge">0</span></button></li>
            <li class="nav-item"><button class="nav-link" data-page="users"><i class="fas fa-users"></i> المستخدمون</button></li>
            <li class="nav-item"><button class="nav-link" data-page="departments"><i class="fas fa-building"></i> الإدارات</button></li>
            <li class="nav-item"><button class="nav-link" data-page="reports"><i class="fas fa-file-alt"></i> التقارير</button></li>
            <li class="nav-item"><button class="nav-link" data-page="notifications"><i class="fas fa-bell"></i> الإشعارات <span class="badge bg-danger rounded-pill ms-2" id="notifBadge">5</span></button></li>
            <li class="nav-item"><button class="nav-link" data-page="settings"><i class="fas fa-cog"></i> الإعدادات</button></li>
        </ul>
        <hr />
        <div class="user-info p-3">
            <div class="d-flex align-items-center">
                <div class="avatar-circle"><span id="sidebarUserInitial">أ</span></div>
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

    <!-- المحتوى -->
    <div class="main-content">
        <button class="btn btn-teal d-md-none mb-3" id="sidebarToggle"><i class="fas fa-bars"></i></button>

        <!-- ===== لوحة التحكم ===== -->
        <div class="page-section active" id="page-dashboard">
            <div class="welcome-banner mb-4">
                <div class="row align-items-center">
                    <div class="col-md-8">
                        <h4><i class="fas fa-user-circle me-2"></i> مرحباً، <span id="dashboardUserName">أحمد محمد</span>!</h4>
                        <p class="mb-0">هذه هي لوحة التحكم الرئيسية لنظام فريقي.</p>
                    </div>
                    <div class="col-md-4 text-md-end mt-3 mt-md-0">
                        <span class="badge bg-light text-dark p-2"><i class="fas fa-calendar-alt me-1"></i><span id="currentDate"></span></span>
                    </div>
                </div>
            </div>

            <!-- الإحصائيات -->
            <div class="row g-4 mb-4">
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card total">
                        <div><div class="number" id="statTotalTasks">0</div><div class="label">إجمالي التكليفات</div></div>
                        <div class="icon"><i class="fas fa-tasks"></i></div>
                    </div>
                </div>
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card pending">
                        <div><div class="number" id="statPendingTasks">0</div><div class="label">قيد التنفيذ</div></div>
                        <div class="icon"><i class="fas fa-clock"></i></div>
                    </div>
                </div>
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card completed">
                        <div><div class="number" id="statCompletedTasks">0</div><div class="label">مكتملة</div></div>
                        <div class="icon"><i class="fas fa-check-circle"></i></div>
                    </div>
                </div>
                <div class="col-xl-3 col-lg-6 col-md-6">
                    <div class="stat-card overdue">
                        <div><div class="number" id="statOverdueTasks">0</div><div class="label">متأخرة</div></div>
                        <div class="icon"><i class="fas fa-exclamation-triangle"></i></div>
                    </div>
                </div>
            </div>

            <!-- المخططات -->
            <div class="row g-4">
                <div class="col-lg-6">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-chart-pie text-teal me-2"></i> توزيع المهام</div>
                        <div class="card-body"><canvas id="taskPieChart" height="250"></canvas></div>
                    </div>
                </div>
                <div class="col-lg-6">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-chart-line text-teal me-2"></i> مؤشرات الأداء</div>
                        <div class="card-body" id="kpiContainer">
                            <div class="mb-3"><div class="d-flex justify-content-between"><span>متوسط وقت الإنجاز</span><span class="fw-semibold" id="kpiAvgTime">4.2 يوم</span></div></div>
                            <div class="mb-3"><div class="d-flex justify-content-between"><span>نسبة المهام المتأخرة</span><span class="fw-semibold text-danger" id="kpiOverdueRate">0%</span></div></div>
                            <div class="mb-3"><div class="d-flex justify-content-between"><span>معدل الإنجاز الأسبوعي</span><span class="fw-semibold text-success" id="kpiWeeklyRate">0 مهام/أسبوع</span></div></div>
                            <div><div class="d-flex justify-content-between"><span>تقييم الأداء العام</span><span class="fw-semibold text-teal" id="kpiOverall">جيد (0%)</span></div></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ===== التكليفات ===== -->
        <div class="page-section" id="page-tasks">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-tasks text-teal me-2"></i> التكليفات</h4>
                <button class="btn btn-success" data-bs-toggle="modal" data-bs-target="#newTaskModal"><i class="fas fa-plus me-2"></i> تكليف جديد</button>
            </div>
            <div class="card mb-4">
                <div class="card-body">
                    <div class="row g-3">
                        <div class="col-md-4"><input type="text" class="form-control" id="searchTask" placeholder="بحث..." /></div>
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
                        <div class="col-md-2"><button class="btn btn-outline-teal w-100" id="clearFilters"><i class="fas fa-times me-1"></i> مسح</button></div>
                    </div>
                </div>
            </div>
            <div class="card">
                <div class="card-body p-0">
                    <div class="table-responsive">
                        <table class="table table-hover mb-0">
                            <thead class="table-light"><tr><th>#</th><th>العنوان</th><th>الإدارة</th><th>المسؤول</th><th>التسليم</th><th>الحالة</th><th>الأولوية</th><th>الإجراءات</th></tr></thead>
                            <tbody id="tasksTableBody"><tr><td colspan="8" class="text-center">⏳ جاري التحميل...</td></tr></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- ===== المستخدمون ===== -->
        <div class="page-section" id="page-users">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-users text-teal me-2"></i> المستخدمون</h4>
                <button class="btn btn-success" data-bs-toggle="modal" data-bs-target="#newUserModal"><i class="fas fa-user-plus me-2"></i> مستخدم جديد</button>
            </div>
            <div class="row g-4 mb-4">
                <div class="col-md-3"><div class="stat-card total p-3"><div class="number" id="totalUsers">0</div><div class="label">الإجمالي</div></div></div>
                <div class="col-md-3"><div class="stat-card pending p-3"><div class="number" id="activeUsers">0</div><div class="label">نشط</div></div></div>
                <div class="col-md-3"><div class="stat-card completed p-3"><div class="number" id="inactiveUsers">0</div><div class="label">غير نشط</div></div></div>
                <div class="col-md-3"><div class="stat-card overdue p-3"><div class="number" id="suspendedUsers">0</div><div class="label">معلق</div></div></div>
            </div>
            <div class="card">
                <div class="card-body p-0">
                    <div class="table-responsive">
                        <table class="table table-hover mb-0">
                            <thead class="table-light"><tr><th>#</th><th>الاسم</th><th>البريد</th><th>الإدارة</th><th>الدور</th><th>الحالة</th><th>الإجراءات</th></tr></thead>
                            <tbody id="usersBody"><tr><td colspan="7" class="text-center">⏳ جاري التحميل...</td></tr></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- ===== الإدارات ===== -->
        <div class="page-section" id="page-departments">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-building text-teal me-2"></i> الإدارات</h4>
                <button class="btn btn-success" data-bs-toggle="modal" data-bs-target="#newDeptModal"><i class="fas fa-plus me-2"></i> إدارة جديدة</button>
            </div>
            <div class="row g-4" id="departmentsContainer"></div>
        </div>

        <!-- ===== التقارير ===== -->
        <div class="page-section" id="page-reports">
            <h4 class="mb-4"><i class="fas fa-file-alt text-teal me-2"></i> التقارير</h4>
            <div class="row g-4 mb-4">
                <div class="col-md-6 col-lg-3"><div class="card report-card text-center p-3"><i class="fas fa-file-pdf fa-3x text-danger mb-3"></i><h6>تقرير التكليفات</h6><button class="btn btn-outline-teal btn-sm" onclick="window.print()">تحميل PDF</button></div></div>
                <div class="col-md-6 col-lg-3"><div class="card report-card text-center p-3"><i class="fas fa-file-excel fa-3x text-success mb-3"></i><h6>تقرير الأداء</h6><button class="btn btn-outline-teal btn-sm" onclick="window.print()">تحميل PDF</button></div></div>
                <div class="col-md-6 col-lg-3"><div class="card report-card text-center p-3"><i class="fas fa-file-alt fa-3x text-primary mb-3"></i><h6>المهام المتأخرة</h6><button class="btn btn-outline-teal btn-sm" onclick="window.print()">تحميل PDF</button></div></div>
                <div class="col-md-6 col-lg-3"><div class="card report-card text-center p-3"><i class="fas fa-users-cog fa-3x text-teal mb-3"></i><h6>تقرير المستخدمين</h6><button class="btn btn-outline-teal btn-sm" onclick="window.print()">تحميل PDF</button></div></div>
            </div>
        </div>

        <!-- ===== الإشعارات ===== -->
        <div class="page-section" id="page-notifications">
            <div class="d-flex justify-content-between align-items-center flex-wrap gap-3 mb-4">
                <h4><i class="fas fa-bell text-teal me-2"></i> الإشعارات</h4>
                <button class="btn btn-outline-teal btn-sm" id="markAllRead"><i class="fas fa-check-double me-1"></i> تحديد الكل كمقروء</button>
            </div>
            <div class="card"><div class="card-body p-0" id="notificationsList"></div></div>
        </div>

        <!-- ===== الإعدادات ===== -->
        <div class="page-section" id="page-settings">
            <h4 class="mb-4"><i class="fas fa-cog text-teal me-2"></i> الإعدادات</h4>
            <div class="row g-4">
                <div class="col-lg-8">
                    <div class="card">
                        <div class="card-header"><i class="fas fa-user-edit me-2"></i> الملف الشخصي</div>
                        <div class="card-body">
                            <form id="profileForm">
                                <div class="row g-3">
                                    <div class="col-md-6"><label class="form-label">الاسم الكامل</label><input type="text" class="form-control" id="profileFullName" /></div>
                                    <div class="col-md-6"><label class="form-label">البريد الإلكتروني</label><input type="email" class="form-control" id="profileEmail" /></div>
                                    <div class="col-md-6"><label class="form-label">رقم الهاتف</label><input type="tel" class="form-control" id="profilePhone" /></div>
                                    <div class="col-md-6"><label class="form-label">الوظيفة</label><input type="text" class="form-control" id="profileJob" /></div>
                                    <div class="col-12"><button type="submit" class="btn btn-teal"><i class="fas fa-save me-2"></i>حفظ</button></div>
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
                                <div class="mb-3"><label>كلمة المرور الحالية</label><input type="password" class="form-control" required /></div>
                                <div class="mb-3"><label>كلمة المرور الجديدة</label><input type="password" class="form-control" required /></div>
                                <div class="mb-3"><label>تأكيد كلمة المرور</label><input type="password" class="form-control" required /></div>
                                <button type="submit" class="btn btn-teal w-100"><i class="fas fa-key me-2"></i>تغيير</button>
                            </form>
                        </div>
                    </div>
                    <div class="card mt-4">
                        <div class="card-header"><i class="fas fa-sliders-h me-2"></i> النظام</div>
                        <div class="card-body">
                            <div class="form-check form-switch mb-2"><input class="form-check-input" type="checkbox" id="darkModeSwitch" /><label class="form-check-label" for="darkModeSwitch">الوضع الليلي</label></div>
                            <hr />
                            <div class="mb-3"><label>رفع الشعار</label><input type="file" class="form-control" id="logoInput" accept="image/*" /><button class="btn btn-teal mt-2" id="saveLogoBtn"><i class="fas fa-upload me-1"></i> رفع</button></div>
                            <hr />
                            <button class="btn btn-outline-danger btn-sm w-100" id="clearDataBtn"><i class="fas fa-trash me-1"></i> مسح البيانات</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
</div>

<!-- ===== مودالات ===== -->
<div class="modal fade" id="newTaskModal" tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header"><h5 class="modal-title"><i class="fas fa-plus-circle text-teal me-2"></i> تكليف جديد</h5><button type="button" class="btn-close" data-bs-dismiss="modal"></button></div>
            <div class="modal-body">
                <form id="newTaskForm">
                    <div class="row g-3">
                        <div class="col-12"><label class="form-label">العنوان *</label><input type="text" class="form-control" required /></div>
                        <div class="col-md-6"><label class="form-label">الإدارة *</label><select class="form-select" required><option value="">اختر</option><option>المكتب الفني</option><option>الجودة</option><option>الموارد البشرية</option><option>مكافحة العدوى</option><option>التمريض</option><option>الصيدلة</option></select></div>
                        <div class="col-md-6"><label class="form-label">المسؤول *</label><select class="form-select" required><option value="">اختر</option><option>أحمد محمد</option><option>د. سارة</option><option>أ. خالد</option><option>م. يوسف</option><option>م. نورة</option><option>ص. نادر</option></select></div>
                        <div class="col-md-6"><label class="form-label">تاريخ التسليم *</label><input type="date" class="form-control" required /></div>
                        <div class="col-md-6"><label class="form-label">الأولوية</label><select class="form-select"><option value="normal">عادية</option><option value="medium">متوسطة</option><option value="critical">حرجة</option></select></div>
                        <div class="col-12"><label class="form-label">الوصف</label><textarea class="form-control" rows="3"></textarea></div>
                    </div>
                </form>
            </div>
            <div class="modal-footer"><button type="button" class="btn btn-secondary" data-bs-dismiss="modal">إلغاء</button><button type="button" class="btn btn-success" id="createTaskBtn">إنشاء</button></div>
        </div>
    </div>
</div>

<div class="modal fade" id="newUserModal" tabindex="-1">
    <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header"><h5 class="modal-title"><i class="fas fa-user-plus text-teal me-2"></i> مستخدم جديد</h5><button type="button" class="btn-close" data-bs-dismiss="modal"></button></div>
            <div class="modal-body">
                <form id="newUserForm">
                    <div class="row g-3">
                        <div class="col-md-6"><label class="form-label">الاسم *</label><input type="text" class="form-control" id="userFullName" required /></div>
                        <div class="col-md-6"><label class="form-label">اسم المستخدم *</label><input type="text" class="form-control" id="userUsername" required /></div>
                        <div class="col-md-6"><label class="form-label">البريد *</label><input type="email" class="form-control" id="userEmail" required /></div>
                        <div class="col-md-6"><label class="form-label">الهاتف</label><input type="tel" class="form-control" id="userPhone" /></div>
                        <div class="col-md-6"><label class="form-label">الإدارة *</label><select class="form-select" id="userDepartment" required><option value="">اختر</option><option>المكتب الفني</option><option>الجودة</option><option>الموارد البشرية</option><option>مكافحة العدوى</option><option>التمريض</option><option>المعمل</option><option>الأشعة</option><option>الصيدلة</option><option>الهندسة</option></select></div>
                        <div class="col-md-6"><label class="form-label">الدور *</label><select class="form-select" id="userRole" required><option value="">اختر</option><option>مدير المستشفى</option><option>مدير المكتب الفني</option><option>عضو المكتب الفني</option><option>مدير إدارة</option><option>مستخدم إدارة</option><option>مراقب</option></select></div>
                        <div class="col-md-6"><label class="form-label">كلمة المرور *</label><input type="password" class="form-control" id="userPassword" required /></div>
                        <div class="col-md-6"><label class="form-label">تأكيد كلمة المرور *</label><input type="password" class="form-control" id="userConfirmPassword" required /></div>
                    </div>
                </form>
            </div>
            <div class="modal-footer"><button type="button" class="btn btn-secondary" data-bs-dismiss="modal">إلغاء</button><button type="button" class="btn btn-success" id="saveUserBtn">إضافة</button></div>
        </div>
    </div>
</div>

<div class="modal fade" id="newDeptModal" tabindex="-1">
    <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
            <div class="modal-header"><h5 class="modal-title"><i class="fas fa-plus-circle text-teal me-2"></i> إدارة جديدة</h5><button type="button" class="btn-close" data-bs-dismiss="modal"></button></div>
            <div class="modal-body">
                <form id="newDeptForm">
                    <div class="mb-3"><label class="form-label">اسم الإدارة *</label><input type="text" class="form-control" required /></div>
                    <div class="mb-3"><label class="form-label">المدير</label><select class="form-select"><option value="">اختر</option><option>أحمد محمد</option><option>د. سارة</option><option>أ. خالد</option><option>م. يوسف</option><option>م. نورة</option><option>ص. نادر</option></select></div>
                    <div class="mb-3"><label class="form-label">الوصف</label><textarea class="form-control" rows="3"></textarea></div>
                </form>
            </div>
            <div class="modal-footer"><button type="button" class="btn btn-secondary" data-bs-dismiss="modal">إلغاء</button><button type="button" class="btn btn-success" id="createDeptBtn">إضافة</button></div>
        </div>
    </div>
</div>

<script>
// ==========================================
// 1. إعدادات Firebase - تم تحديثها بإعداداتك
// ==========================================
const firebaseConfig = {
    apiKey: "AIzaSyBN9WE0RPuvjFAkLQtmhg39OcFPz3nBEcA",
    authDomain: "fariqi-system.firebaseapp.com",
    projectId: "fariqi-system",
    storageBucket: "fariqi-system.firebasestorage.app",
    messagingSenderId: "248925303859",
    appId: "1:248925303859:web:03c4f67a58a42ecd559f46"
};

// تهيئة Firebase
let db = null;
let storage = null;

try {
    if (typeof firebase !== 'undefined') {
        firebase.initializeApp(firebaseConfig);
        db = firebase.firestore();
        storage = firebase.storage();
        console.log('✅ Firebase initialized successfully with your config!');
        
        // اختبار الاتصال
        db.collection('test').doc('connection').set({ connected: true, time: new Date().toISOString() })
            .then(() => console.log('✅ Firebase connection test successful'))
            .catch(err => console.warn('⚠️ Firebase connection test failed:', err.message));
    }
} catch (e) {
    console.warn('⚠️ Firebase initialization error:', e.message);
}

// ==========================================
// 2. المستخدمون والبيانات
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
        status: 'نشط'
    }
};

function getStoredUsers() {
    try {
        const data = localStorage.getItem('hospitalUsers');
        return data ? JSON.parse(data) : DEFAULT_USERS;
    } catch(e) { return DEFAULT_USERS; }
}

function saveStoredUsers(users) {
    localStorage.setItem('hospitalUsers', JSON.stringify(users));
}

function getTasks() {
    try {
        const data = localStorage.getItem('hospitalTasks');
        return data ? JSON.parse(data) : [];
    } catch(e) { return []; }
}

function saveTasks(tasks) {
    localStorage.setItem('hospitalTasks', JSON.stringify(tasks));
}

// ==========================================
// 3. وظائف تسجيل الدخول
// ==========================================
function login(username, password) {
    const users = getStoredUsers();
    const user = users[username];
    if (user && user.password === password) {
        currentUser = { username, ...user };
        delete currentUser.password;
        localStorage.setItem('currentUser', JSON.stringify(currentUser));
        return true;
    }
    return false;
}

function logout() {
    currentUser = null;
    localStorage.removeItem('currentUser');
    document.getElementById('page-login').style.display = 'flex';
    document.getElementById('app-container').style.display = 'none';
}

function showApp() {
    document.getElementById('page-login').style.display = 'none';
    document.getElementById('app-container').style.display = 'block';
    updateUI();
    loadTasks();
    loadUsers();
    loadDepartments();
    updateDashboard();
}

function updateUI() {
    if (!currentUser) return;
    document.getElementById('sidebarUserName').textContent = currentUser.fullName || currentUser.username;
    document.getElementById('sidebarUserRole').textContent = currentUser.role || 'مستخدم';
    document.getElementById('sidebarUserInitial').textContent = (currentUser.fullName || currentUser.username).charAt(0);
    document.getElementById('dashboardUserName').textContent = currentUser.fullName || currentUser.username;
    
    const now = new Date().toLocaleDateString('ar-EG', { year: 'numeric', month: 'long', day: 'numeric' });
    document.getElementById('currentDate').textContent = now;
    
    document.getElementById('profileFullName').value = currentUser.fullName || '';
    document.getElementById('profileEmail').value = currentUser.username + '@hospital.local';
    document.getElementById('profilePhone').value = currentUser.phone || '';
    document.getElementById('profileJob').value = currentUser.job || '';
    
    applySavedLogo();
    const darkMode = localStorage.getItem('darkMode') === 'true';
    if (darkMode) {
        document.documentElement.setAttribute('data-theme', 'dark');
        document.getElementById('darkModeSwitch').checked = true;
    }
}

// ==========================================
// 4. تحميل البيانات
// ==========================================
function loadTasks() {
    const tbody = document.getElementById('tasksTableBody');
    const tasks = getTasks();
    
    if (tasks.length === 0) {
        const defaultTasks = [
            { id: 1, title: 'تقرير المكافحة الشهري', dept: 'مكافحة العدوى', assignee: 'أ. خالد', dueDate: '2026-06-18', status: 'overdue', priority: 'critical' },
            { id: 2, title: 'تحديث سياسات الجودة', dept: 'الجودة', assignee: 'د. سارة', dueDate: '2026-06-21', status: 'pending', priority: 'critical' },
            { id: 3, title: 'جرد الأدوية الفصلية', dept: 'الصيدلة', assignee: 'ص. نادر', dueDate: '2026-06-23', status: 'pending', priority: 'medium' },
            { id: 4, title: 'اجتماع المكتب الفني', dept: 'المكتب الفني', assignee: 'أحمد محمد', dueDate: '2026-06-15', status: 'completed', priority: 'normal' },
            { id: 5, title: 'تقييم أداء الموظفين', dept: 'الموارد البشرية', assignee: 'م. يوسف', dueDate: '2026-06-10', status: 'completed', priority: 'medium' },
            { id: 6, title: 'برنامج تدريب التمريض', dept: 'التمريض', assignee: 'م. نورة', dueDate: '2026-06-28', status: 'pending', priority: 'medium' }
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
                <button class="btn btn-sm btn-outline-teal" onclick="viewTask(${task.id})"><i class="fas fa-eye"></i></button>
                <button class="btn btn-sm btn-outline-danger" onclick="deleteTask(${task.id})"><i class="fas fa-trash"></i></button>
            </td>
        </tr>
    `).join('');
    
    document.getElementById('taskBadge').textContent = tasks.filter(t => t.status === 'pending').length;
    updateDashboard();
}

function viewTask(id) {
    const tasks = getTasks();
    const task = tasks.find(t => t.id === id);
    if (task) {
        alert(`📋 تفاصيل التكليف:\n\nالعنوان: ${task.title}\nالإدارة: ${task.dept}\nالمسؤول: ${task.assignee}\nالتسليم: ${task.dueDate}\nالحالة: ${task.status}\nالأولوية: ${task.priority}`);
    }
}

function deleteTask(id) {
    if (!confirm('⚠️ هل أنت متأكد من حذف هذا التكليف؟')) return;
    let tasks = getTasks();
    tasks = tasks.filter(t => t.id !== id);
    saveTasks(tasks);
    
    // حذف من Firebase
    if (db) {
        db.collection('tasks').where('id', '==', id).get()
            .then(snapshot => snapshot.forEach(doc => doc.ref.delete()))
            .catch(e => console.warn('Firebase delete error:', e));
    }
    
    loadTasks();
    showNotification('✅ تم حذف التكليف بنجاح', 'success');
}

function loadUsers() {
    const users = getStoredUsers();
    const userList = Object.keys(users).map(key => ({ id: key, ...users[key] }));
    
    const tbody = document.getElementById('usersBody');
    tbody.innerHTML = userList.map((user, idx) => `
        <tr>
            <td>${idx + 1}</td>
            <td><div class="d-flex align-items-center"><div class="avatar-circle me-2" style="width:32px;height:32px;font-size:12px;">${(user.fullName || user.username).charAt(0)}</div>${user.fullName || user.username}</div></td>
            <td>${user.username}@hospital.local</td>
            <td>${user.department || 'غير محدد'}</td>
            <td>${user.role || 'مستخدم'}</td>
            <td><span class="badge ${user.status === 'نشط' ? 'bg-success' : 'bg-warning'}">${user.status}</span></td>
            <td>
                <button class="btn btn-sm btn-outline-teal"><i class="fas fa-eye"></i></button>
                <button class="btn btn-sm btn-outline-danger" onclick="deleteUser('${user.username}')"><i class="fas fa-trash"></i></button>
            </td>
        </tr>
    `).join('');
    
    document.getElementById('totalUsers').textContent = userList.length;
    document.getElementById('activeUsers').textContent = userList.filter(u => u.status === 'نشط').length;
    document.getElementById('inactiveUsers').textContent = userList.filter(u => u.status === 'غير نشط').length;
    document.getElementById('suspendedUsers').textContent = userList.filter(u => u.status === 'معلق').length;
}

function deleteUser(username) {
    if (!confirm(`⚠️ هل أنت متأكد من حذف المستخدم "${username}"؟`)) return;
    const users = getStoredUsers();
    delete users[username];
    saveStoredUsers(users);
    
    if (db) {
        db.collection('users').where('username', '==', username).get()
            .then(snapshot => snapshot.forEach(doc => doc.ref.delete()))
            .catch(e => console.warn('Firebase delete error:', e));
    }
    
    loadUsers();
    showNotification('✅ تم حذف المستخدم بنجاح', 'success');
}

function loadDepartments() {
    const departments = [
        { name: 'المكتب الفني', icon: 'fa-star', color: 'teal', members: 3, manager: 'أحمد محمد', tasks: 12, progress: 92 },
        { name: 'الجودة', icon: 'fa-check-circle', color: 'success', members: 2, manager: 'د. سارة', tasks: 8, progress: 78 },
        { name: 'الموارد البشرية', icon: 'fa-users', color: 'warning', members: 2, manager: 'م. يوسف', tasks: 6, progress: 65 },
        { name: 'مكافحة العدوى', icon: 'fa-shield-virus', color: 'danger', members: 2, manager: 'أ. خالد', tasks: 5, progress: 43 },
        { name: 'التمريض', icon: 'fa-user-md', color: 'info', members: 3, manager: 'م. نورة', tasks: 10, progress: 70 },
        { name: 'الصيدلة', icon: 'fa-pills', color: 'secondary', members: 1, manager: 'ص. نادر', tasks: 4, progress: 55 }
    ];
    
    document.getElementById('departmentsContainer').innerHTML = departments.map(dept => `
        <div class="col-lg-4 col-md-6">
            <div class="card department-card">
                <div class="card-body">
                    <div class="d-flex justify-content-between align-items-start">
                        <div>
                            <h5 class="fw-bold"><i class="fas ${dept.icon} text-${dept.color} me-1"></i> ${dept.name}</h5>
                            <p class="text-muted mb-1">${dept.members} أعضاء</p>
                            <p class="text-muted small">المدير: ${dept.manager}</p>
                        </div>
                        <div class="badge bg-${dept.color} p-2">${dept.tasks} مهمة</div>
                    </div>
                    <div class="mt-3">
                        <div class="progress"><div class="progress-bar bg-${dept.color}" style="width: ${dept.progress}%;"></div></div>
                        <small class="text-muted">${dept.progress}%</small>
                    </div>
                </div>
            </div>
        </div>
    `).join('');
}

function updateDashboard() {
    const tasks = getTasks();
    const total = tasks.length;
    const pending = tasks.filter(t => t.status === 'pending').length;
    const completed = tasks.filter(t => t.status === 'completed').length;
    const overdue = tasks.filter(t => t.status === 'overdue').length;
    
    document.getElementById('statTotalTasks').textContent = total;
    document.getElementById('statPendingTasks').textContent = pending;
    document.getElementById('statCompletedTasks').textContent = completed;
    document.getElementById('statOverdueTasks').textContent = overdue;
    
    const rate = total > 0 ? Math.round((completed / total) * 100) : 0;
    document.getElementById('kpiAvgTime').textContent = total > 0 ? (4.2 + (overdue * 0.5)).toFixed(1) + ' يوم' : '0 يوم';
    document.getElementById('kpiOverdueRate').textContent = total > 0 ? Math.round((overdue / total) * 100) + '%' : '0%';
    document.getElementById('kpiWeeklyRate').textContent = Math.round(completed / 4) + ' مهام/أسبوع';
    document.getElementById('kpiOverall').textContent = rate >= 70 ? `جيد جداً (${rate}%)` : `جيد (${rate}%)`;
}

function showNotification(message, type = 'info') {
    const div = document.createElement('div');
    div.className = `alert alert-${type} alert-dismissible fade show position-fixed top-0 start-50 translate-middle-x mt-3`;
    div.style.zIndex = '9999';
    div.style.maxWidth = '500px';
    div.style.boxShadow = 'var(--shadow-lg)';
    div.innerHTML = `${message} <button type="button" class="btn-close" data-bs-dismiss="alert"></button>`;
    document.body.appendChild(div);
    setTimeout(() => div.remove(), 5000);
}

// ==========================================
// 5. الشعار والإعدادات
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
                showNotification('✅ تم تحديث الشعار بنجاح', 'success');
                applySavedLogo();
            };
            reader.readAsDataURL(file);
        } else {
            showNotification('⚠️ الرجاء اختيار ملف صورة', 'warning');
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
// 6. أحداث الصفحة
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
        } catch(e) {}
    }
    document.getElementById('page-login').style.display = 'flex';
    document.getElementById('app-container').style.display = 'none';
    
    // تسجيل الدخول
    document.getElementById('loginForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const username = document.getElementById('loginUsername').value.trim();
        const password = document.getElementById('loginPassword').value.trim();
        const errorEl = document.getElementById('loginError');
        
        if (login(username, password)) {
            errorEl.classList.add('d-none');
            showApp();
        } else {
            errorEl.classList.remove('d-none');
        }
    });
    
    // تبديل كلمة المرور
    document.getElementById('togglePassword').addEventListener('click', function() {
        const pwd = document.getElementById('loginPassword');
        pwd.type = pwd.type === 'password' ? 'text' : 'password';
        this.querySelector('i').classList.toggle('fa-eye');
        this.querySelector('i').classList.toggle('fa-eye-slash');
    });
    
    // التنقل
    document.querySelectorAll('.sidebar .nav-link[data-page]').forEach(link => {
        link.addEventListener('click', function() {
            document.querySelectorAll('.page-section').forEach(el => el.classList.remove('active'));
            document.getElementById('page-' + this.dataset.page).classList.add('active');
            document.querySelectorAll('.sidebar .nav-link').forEach(l => l.classList.remove('active'));
            this.classList.add('active');
            document.getElementById('sidebar').classList.remove('open');
        });
    });
    
    // تبديل القائمة
    document.getElementById('sidebarToggle').addEventListener('click', function() {
        document.getElementById('sidebar').classList.toggle('open');
    });
    
    // تسجيل الخروج
    document.getElementById('logoutBtn').addEventListener('click', logout);
    
    // الوضع الليلي
    document.getElementById('darkModeSwitch').addEventListener('change', function() {
        if (this.checked) {
            document.documentElement.setAttribute('data-theme', 'dark');
            localStorage.setItem('darkMode', 'true');
        } else {
            document.documentElement.removeAttribute('data-theme');
            localStorage.setItem('darkMode', 'false');
        }
    });
    
    // رفع الشعار
    handleLogoUpload();
    
    // إنشاء تكليف
    document.getElementById('createTaskBtn').addEventListener('click', function() {
        const form = document.getElementById('newTaskForm');
        const title = form.querySelector('input[type="text"]').value.trim();
        const dept = form.querySelectorAll('select')[0].value;
        const assignee = form.querySelectorAll('select')[1].value;
        const dueDate = form.querySelector('input[type="date"]').value;
        
        if (!title || !dept || !assignee || !dueDate) {
            showNotification('⚠️ يرجى ملء جميع الحقول المطلوبة', 'warning');
            return;
        }
        
        const tasks = getTasks();
        tasks.push({
            id: Date.now(),
            title, dept, assignee, dueDate,
            status: 'pending',
            priority: form.querySelectorAll('select')[2]?.value || 'normal'
        });
        saveTasks(tasks);
        
        if (db) {
            db.collection('tasks').add({ title, dept, assignee, dueDate, status: 'pending', createdAt: new Date().toISOString() })
                .catch(e => console.warn('Firebase save error:', e));
        }
        
        bootstrap.Modal.getInstance(document.getElementById('newTaskModal')).hide();
        loadTasks();
        showNotification('✅ تم إنشاء التكليف بنجاح', 'success');
        form.reset();
    });
    
    // إضافة مستخدم
    document.getElementById('saveUserBtn').addEventListener('click', function() {
        const fullName = document.getElementById('userFullName').value.trim();
        const username = document.getElementById('userUsername').value.trim();
        const email = document.getElementById('userEmail').value.trim();
        const password = document.getElementById('userPassword').value;
        const confirm = document.getElementById('userConfirmPassword').value;
        const role = document.getElementById('userRole').value;
        const dept = document.getElementById('userDepartment').value;
        
        if (!fullName || !username || !email || !password || !role || !dept) {
            showNotification('⚠️ يرجى ملء جميع الحقول', 'warning');
            return;
        }
        if (password !== confirm) {
            showNotification('⚠️ كلمتا المرور غير متطابقتين', 'danger');
            return;
        }
        if (password.length < 6) {
            showNotification('⚠️ كلمة المرور يجب أن تكون 6 خانات على الأقل', 'warning');
            return;
        }
        
        const users = getStoredUsers();
        if (users[username]) {
            showNotification('⚠️ اسم المستخدم موجود بالفعل', 'warning');
            return;
        }
        
        users[username] = { username, password, fullName, role, department: dept, status: 'نشط' };
        saveStoredUsers(users);
        
        if (db) {
            db.collection('users').add({ username, fullName, email, role, department: dept, status: 'نشط' })
                .catch(e => console.warn('Firebase save error:', e));
        }
        
        bootstrap.Modal.getInstance(document.getElementById('newUserModal')).hide();
        loadUsers();
        showNotification(`✅ تم إضافة المستخدم ${fullName}`, 'success');
        document.getElementById('newUserForm').reset();
    });
    
    // إضافة إدارة
    document.getElementById('createDeptBtn').addEventListener('click', function() {
        const name = document.getElementById('newDeptForm').querySelector('input').value.trim();
        if (!name) {
            showNotification('⚠️ يرجى إدخال اسم الإدارة', 'warning');
            return;
        }
        const depts = JSON.parse(localStorage.getItem('departments') || '[]');
        depts.push({ id: Date.now(), name });
        localStorage.setItem('departments', JSON.stringify(depts));
        bootstrap.Modal.getInstance(document.getElementById('newDeptModal')).hide();
        loadDepartments();
        showNotification(`✅ تم إضافة الإدارة ${name}`, 'success');
        document.getElementById('newDeptForm').reset();
    });
    
    // تحديد الكل كمقروء
    document.getElementById('markAllRead').addEventListener('click', function() {
        document.querySelectorAll('#notificationsList .badge').forEach(b => {
            b.textContent = 'مقروء';
            b.className = 'badge bg-secondary rounded-pill';
        });
        document.getElementById('notifBadge').textContent = '0';
        showNotification('✅ تم تحديد جميع الإشعارات كمقروءة', 'success');
    });
    
    // مسح البيانات
    document.getElementById('clearDataBtn').addEventListener('click', function() {
        if (confirm('⚠️ هل أنت متأكد من مسح جميع البيانات؟')) {
            localStorage.clear();
            showNotification('🗑️ تم مسح جميع البيانات', 'warning');
            setTimeout(() => location.reload(), 1500);
        }
    });
    
    // بحث وتصفية
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
        document.querySelectorAll('#tasksTableBody tr').forEach(row => {
            const text = row.textContent.toLowerCase();
            const rowStatus = row.dataset.status || '';
            const rowDept = row.dataset.dept || '';
            let show = true;
            if (search && !text.includes(search)) show = false;
            if (status && rowStatus !== status) show = false;
            if (dept && rowDept !== dept) show = false;
            row.style.display = show ? '' : 'none';
        });
    }
    
    // إغلاق القائمة
    document.addEventListener('click', function(e) {
        const sidebar = document.getElementById('sidebar');
        const toggle = document.getElementById('sidebarToggle');
        if (window.innerWidth < 768 && sidebar.classList.contains('open') &&
            !sidebar.contains(e.target) && !toggle.contains(e.target)) {
            sidebar.classList.remove('open');
        }
    });
    
    // المخطط
    setTimeout(() => {
        const ctx = document.getElementById('taskPieChart')?.getContext('2d');
        if (ctx) {
            new Chart(ctx, {
                type: 'pie',
                data: {
                    labels: ['مكتملة', 'قيد التنفيذ', 'متأخرة'],
                    datasets: [{ data: [14, 8, 2], backgroundColor: ['#43A047', '#FB8C00', '#E53935'], borderWidth: 0 }]
                },
                options: { responsive: true, plugins: { legend: { position: 'bottom' } } }
            });
        }
    }, 500);
    
    console.log('🚀 نظام فريقي V2.0 جاهز للتشغيل مع Firebase!');
});
</script>
</body>
</html>
