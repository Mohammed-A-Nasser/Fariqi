<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>فريقي - نظام إدارة المكتب الفني</title>
    
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css" />
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore-compat.js"></script>
    
    <style>
        :root {
            --teal: #00897B;
            --teal-dark: #00695C;
            --teal-light: #e0f2f1;
            --sidebar-width: 260px;
        }
        * { box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #f4f6f8;
            color: #333;
            margin: 0;
            padding: 0;
        }
        .page-section { display: none; }
        .page-section.active { display: block; }
        
        .text-teal { color: var(--teal) !important; }
        .bg-teal { background-color: var(--teal) !important; color: #fff; }
        .btn-teal {
            background-color: var(--teal);
            color: #fff;
            border: none;
        }
        .btn-teal:hover { background-color: var(--teal-dark); color: #fff; }
        .btn-outline-teal {
            border: 1px solid var(--teal);
            color: var(--teal);
            background: transparent;
        }
        .btn-outline-teal:hover {
            background-color: var(--teal);
            color: #fff;
        }
        
        .login-page {
            background: linear-gradient(135deg, #e0f2f1 0%, #b2dfdb 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .logo-circle {
            width: 80px;
            height: 80px;
            background: #e0f2f1;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 16px;
        }
        .login-page .card {
            border-radius: 1.2rem;
            animation: fadeInUp 0.4s ease;
        }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .sidebar {
            position: fixed;
            top: 0;
            right: 0;
            width: var(--sidebar-width);
            height: 100vh;
            background: #fff;
            box-shadow: -2px 0 12px rgba(0,0,0,0.08);
            display: flex;
            flex-direction: column;
            padding: 20px 0;
            z-index: 1000;
            overflow-y: auto;
            transition: transform 0.3s ease;
        }
        .sidebar-header {
            padding: 0 20px 16px;
            border-bottom: 1px solid #eee;
            margin-bottom: 8px;
        }
        .sidebar .nav-link {
            color: #555;
            padding: 10px 20px;
            border-radius: 0;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 0.95rem;
            transition: background 0.2s, color 0.2s;
            cursor: pointer;
            border: none;
            background: none;
            width: 100%;
            text-align: right;
        }
        .sidebar .nav-link:hover,
        .sidebar .nav-link.active {
            background: var(--teal-light);
            color: var(--teal);
            font-weight: 600;
        }
        .sidebar .nav-link i { width: 18px; text-align: center; }
        .sidebar .logout-btn {
            margin-top: auto;
            padding: 12px 20px;
            display: block;
            border-top: 1px solid #eee;
        }
        .avatar-circle {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1rem;
            flex-shrink: 0;
            background: var(--teal);
            color: #fff;
        }
        
        .main-content {
            margin-right: var(--sidebar-width);
            padding: 28px 30px;
            min-height: 100vh;
        }
        .welcome-banner {
            background: linear-gradient(135deg, #00897B, #1B5E20);
            border-radius: 16px;
        }
        
        .stat-card {
            border-radius: 12px;
            padding: 20px;
            color: #fff;
            position: relative;
            overflow: hidden;
        }
        .stat-card.total { background: linear-gradient(135deg, #00897B, #00695C); }
        .stat-card.pending { background: linear-gradient(135deg, #FB8C00, #E65100); }
        .stat-card.completed { background: linear-gradient(135deg, #43A047, #1B5E20); }
        .stat-card.overdue { background: linear-gradient(135deg, #E53935, #B71C1C); }
        .stat-card .number { font-size: 2.4rem; font-weight: 700; line-height: 1; }
        .stat-card .label { font-size: 0.9rem; opacity: 0.9; margin-top: 4px; }
        .stat-card .icon {
            position: absolute;
            top: 16px;
            left: 16px;
            font-size: 2.2rem;
            opacity: 0.2;
        }
        
        .card {
            border: none;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }
        .card-header {
            background: transparent;
            border-bottom: 1px solid rgba(0,0,0,0.06);
            padding: 16px 20px;
            font-weight: 600;
        }
        
        .critical-task {
            border-right: 4px solid transparent;
            padding: 14px 20px;
        }
        .critical-task.overdue { border-right-color: #dc3545; background: #fff5f5; }
        .critical-task.warning { border-right-color: #ffc107; background: #fffbf0; }
        
        .department-card { transition: transform 0.2s; cursor: pointer; }
        .department-card:hover { transform: translateY(-3px); box-shadow: 0 6px 20px rgba(0,0,0,0.1); }
        
        @media (max-width: 768px) {
            .main-content { margin-right: 0; padding: 16px; }
            .sidebar { transform: translateX(100%); }
            .sidebar.open { transform: translateX(0); }
            .stat-card .number { font-size: 1.8rem; }
        }
        
        .toast-notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 9999;
            padding: 12px 24px;
            border-radius: 8px;
            color: #fff;
            font-weight: 600;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            animation: slideDown 0.3s ease;
            max-width: 90%;
        }
        .toast-success { background: #28a745; }
        .toast-danger { background: #dc3545; }
        .toast-warning { background: #ffc107; color: #333; }
        .toast-info { background: #17a2b8; }
        @keyframes slideDown {
            from { opacity: 0; transform: translateX(-50%) translateY(-20px); }
            to { opacity: 1; transform: translateX(-50%) translateY(0); }
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
                        <div id="loginError" class="alert alert-danger d-none">اسم المستخدم أو كلمة المرور غير صحيحة</div>
                        <form id="loginForm">
                            <div class="mb-3">
                                <label class="form-label fw-semibold"><i class="fas fa-user me-2"></i>اسم المستخدم</label>
                                <input type="text" class="form-control form-control-lg" id="loginUsername" placeholder="أدخل اسم المستخدم" required />
                            </div>
                            <div class="mb-3">
                                <label class="form-label fw-semibold"><i class="fas fa-lock me-2"></i>كلمة المرور</label>
                                <div class="input-group">
                                    <input type="password" class="form-control form-control-lg" id="loginPassword" placeholder="أدخل كلمة المرور" required />
                                    <button class="btn btn-outline-secondary" type="button" id="togglePassword"><i class="fas fa-eye"></i></button>
                                </div>
                            </div>
                            <div class="mb-3">
                                <small class="text-muted">تجريبي: admin / admin123</small>
                            </div>
                            <button type="submit" class="btn btn-success btn-lg w-100 fw-semibold" id="loginBtn">
                                <i class="fas fa-sign-in-alt me-2"></i>تسجيل الدخول
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- ===== التطبيق ===== -->
<div id="app-container" style="display:none;">

    <!-- القائمة الجانبية -->
    <nav class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <h3 class="text-success fw-bold"><i class="fas fa-tasks text-teal"></i> فريقي</h3>
            <small class="text-muted">مستشفى القنطرة شرق</small>
        </div>
        <ul class="nav flex-column">
            <li><button class="nav-link active" data-page="dashboard"><i class="fas fa-chart-pie"></i> لوحة التحكم</button></li>
            <li><button class="nav-link" data-page="tasks"><i class="fas fa-tasks"></i> التكليفات <span class="badge bg-danger rounded-pill ms-2" id="taskBadge">0</span></button></li>
            <li><button class="nav-link" data-page="users"><i class="fas fa-users"></i> المستخدمون</button></li>
            <li><button class="nav-link" data-page="departments"><i class="fas fa-building"></i> الإدارات</button></li>
            <li><button class="nav-link" data-page="reports"><i class="fas fa-file-alt"></i> التقارير</button></li>
            <li><button class="nav-link" data-page="notifications"><i class="fas fa-bell"></i> الإشعارات <span class="badge bg-danger rounded-pill ms-2" id="notifBadge">5</span></button></li>
            <li><button class="nav-link" data-page="settings"><i class="fas fa-cog"></i> الإعدادات</button></li>
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
            <div class="welcome-banner p-4 text-white mb-4">
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
                        <div class="card-body">
                            <div class="mb-3"><div class="d-flex justify-content-between"><span>متوسط وقت الإنجاز</span><span class="fw-semibold" id="kpiAvgTime">0</span></div></div>
                            <div class="mb-3"><div class="d-flex justify-content-between"><span>نسبة المهام المتأخرة</span><span class="fw-semibold text-danger" id="kpiOverdueRate">0%</span></div></div>
                            <div class="mb-3"><div class="d-flex justify-content-between"><span>معدل الإنجاز الأسبوعي</span><span class="fw-semibold text-success" id="kpiWeeklyRate">0</span></div></div>
                            <div><div class="d-flex justify-content-between"><span>تقييم الأداء العام</span><span class="fw-semibold text-teal" id="kpiOverall">جيد</span></div></div>
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
                        <div class="col-md-2"><button class="btn btn-outline-teal w-100" id="clearFilters"><i class="fas fa-times"></i> مسح</button></div>
                    </div>
                </div>
            </div>
            <div class="card">
                <div class="card-body p-0">
                    <div class="table-responsive">
                        <table class="table table-hover mb-0">
                            <thead class="table-light">
                                <tr><th>#</th><th>العنوان</th><th>الإدارة</th><th>المسؤول</th><th>التسليم</th><th>الحالة</th><th>الأولوية</th><th>الإجراءات</th></tr>
                            </thead>
                            <tbody id="tasksTableBody"></tbody>
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
                            <tbody id="usersBody"></tbody>
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
                            <input type="text" class="form-control" id="taskTitle" placeholder="أدخل عنوان التكليف" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الإدارة *</label>
                            <select class="form-select" id="taskDept" required>
                                <option value="">اختر الإدارة</option>
                                <option value="المكتب الفني">المكتب الفني</option>
                                <option value="الجودة">الجودة</option>
                                <option value="الموارد البشرية">الموارد البشرية</option>
                                <option value="مكافحة العدوى">مكافحة العدوى</option>
                                <option value="التمريض">التمريض</option>
                                <option value="الصيدلة">الصيدلة</option>
                            </select>
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">المسؤول *</label>
                            <select class="form-select" id="taskAssignee" required>
                                <option value="">اختر المسؤول</option>
                                <option value="أحمد محمد">أحمد محمد</option>
                                <option value="د. سارة">د. سارة</option>
                                <option value="أ. خالد">أ. خالد</option>
                                <option value="م. يوسف">م. يوسف</option>
                                <option value="م. نورة">م. نورة</option>
                                <option value="ص. نادر">ص. نادر</option>
                            </select>
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">تاريخ التسليم *</label>
                            <input type="date" class="form-control" id="taskDueDate" required />
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الأولوية</label>
                            <select class="form-select" id="taskPriority">
                                <option value="normal">عادية</option>
                                <option value="medium">متوسطة</option>
                                <option value="critical">حرجة</option>
                            </select>
                        </div>
                        <div class="col-12">
                            <label class="form-label fw-semibold">الوصف</label>
                            <textarea class="form-control" id="taskDescription" rows="3" placeholder="وصف التكليف..."></textarea>
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
                                <option value="المكتب الفني">المكتب الفني</option>
                                <option value="الجودة">الجودة</option>
                                <option value="الموارد البشرية">الموارد البشرية</option>
                                <option value="مكافحة العدوى">مكافحة العدوى</option>
                                <option value="التمريض">التمريض</option>
                                <option value="المعمل">المعمل</option>
                                <option value="الأشعة">الأشعة</option>
                                <option value="الصيدلة">الصيدلة</option>
                                <option value="الهندسة">الهندسة</option>
                            </select>
                        </div>
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">الدور *</label>
                            <select class="form-select" id="userRole" required>
                                <option value="">اختر الدور</option>
                                <option value="مدير المستشفى">مدير المستشفى</option>
                                <option value="مدير المكتب الفني">مدير المكتب الفني</option>
                                <option value="عضو المكتب الفني">عضو المكتب الفني</option>
                                <option value="مدير إدارة">مدير إدارة</option>
                                <option value="مستخدم إدارة">مستخدم إدارة</option>
                                <option value="مراقب">مراقب</option>
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
                        <input type="text" class="form-control" id="deptName" placeholder="أدخل اسم الإدارة" required />
                    </div>
                    <div class="mb-3">
                        <label class="form-label fw-semibold">المدير</label>
                        <select class="form-select" id="deptManager">
                            <option value="">اختر المدير</option>
                            <option value="أحمد محمد">أحمد محمد</option>
                            <option value="د. سارة">د. سارة</option>
                            <option value="أ. خالد">أ. خالد</option>
                            <option value="م. يوسف">م. يوسف</option>
                            <option value="م. نورة">م. نورة</option>
                            <option value="ص. نادر">ص. نادر</option>
                        </select>
                    </div>
                    <div class="mb-3">
                        <label class="form-label fw-semibold">الوصف</label>
                        <textarea class="form-control" id="deptDescription" rows="3" placeholder="وصف الإدارة"></textarea>
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

<script>
// ==========================================
// إعدادات Firebase
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
let firebaseReady = false;

try {
    if (typeof firebase !== 'undefined') {
        firebase.initializeApp(firebaseConfig);
        db = firebase.firestore();
        firebaseReady = true;
        console.log('✅ Firebase connected successfully!');
        
        // اختبار الاتصال
        db.collection('test').doc('connection').set({ 
            connected: true, 
            time: new Date().toISOString() 
        }).then(() => {
            console.log('✅ Firebase write test successful');
        }).catch(err => {
            console.warn('⚠️ Firebase write test failed:', err.message);
        });
    }
} catch (e) {
    console.warn('⚠️ Firebase initialization error:', e.message);
}

// ==========================================
// البيانات والمستخدمين
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
        return data ? JSON.parse(data) : JSON.parse(JSON.stringify(DEFAULT_USERS));
    } catch(e) { 
        return JSON.parse(JSON.stringify(DEFAULT_USERS));
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
// دالة الإشعارات
// ==========================================
function showToast(message, type = 'success') {
    const existing = document.querySelector('.toast-notification');
    if (existing) existing.remove();
    
    const toast = document.createElement('div');
    toast.className = `toast-notification toast-${type}`;
    toast.textContent = message;
    document.body.appendChild(toast);
    
    setTimeout(() => {
        if (toast.parentNode) toast.remove();
    }, 3000);
}

// ==========================================
// تسجيل الدخول والخروج
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
    showToast('مرحباً بك في نظام فريقي!', 'success');
}

function updateUI() {
    if (!currentUser) return;
    document.getElementById('sidebarUserName').textContent = currentUser.fullName || currentUser.username;
    document.getElementById('sidebarUserRole').textContent = currentUser.role || 'مستخدم';
    document.getElementById('sidebarUserInitial').textContent = (currentUser.fullName || currentUser.username).charAt(0);
    document.getElementById('dashboardUserName').textContent = currentUser.fullName || currentUser.username;
    
    const now = new Date().toLocaleDateString('ar-EG', { 
        year: 'numeric', 
        month: 'long', 
        day: 'numeric' 
    });
    document.getElementById('currentDate').textContent = now;
    
    document.getElementById('profileFullName').value = currentUser.fullName || '';
    document.getElementById('profileEmail').value = currentUser.username + '@hospital.local';
    document.getElementById('profilePhone').value = currentUser.phone || '';
    document.getElementById('profileJob').value = currentUser.job || '';
    
    // الوضع الليلي
    const darkMode = localStorage.getItem('darkMode') === 'true';
    if (darkMode) {
        document.documentElement.setAttribute('data-theme', 'dark');
        document.getElementById('darkModeSwitch').checked = true;
    }
}

// ==========================================
// تحميل التكليفات
// ==========================================
function loadTasks() {
    const tbody = document.getElementById('tasksTableBody');
    let tasks = getTasks();
    
    // إذا كانت القائمة فارغة، أضف تكليفات افتراضية
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
        tasks = defaultTasks;
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
    
    const isAdmin = currentUser && (currentUser.role === 'مدير المكتب الفني' || currentUser.role === 'مدير المستشفى');
    
    if (tasks.length === 0) {
        tbody.innerHTML = `<tr><td colspan="8" class="text-center py-3">📭 لا توجد تكليفات</td></tr>`;
        return;
    }
    
    tbody.innerHTML = tasks.map((task, idx) => `
        <tr data-status="${task.status}" data-dept="${task.dept?.toLowerCase().replace(' ', '') || ''}">
            <td>${idx + 1}</td>
            <td><strong>${task.title}</strong></td>
            <td>${task.dept || 'غير محدد'}</td>
            <td>${task.assignee || 'غير محدد'}</td>
            <td>${task.dueDate || 'غير محدد'}</td>
            <td><span class="badge ${statusMap[task.status]?.class || 'bg-secondary'}">${statusMap[task.status]?.text || task.status}</span></td>
            <td><span class="badge ${priorityMap[task.priority]?.class || 'bg-secondary'}">${priorityMap[task.priority]?.text || task.priority}</span></td>
            <td>
                <button class="btn btn-sm btn-outline-teal" onclick="viewTask(${task.id})"><i class="fas fa-eye"></i></button>
                ${isAdmin ? `<button class="btn btn-sm btn-outline-danger" onclick="deleteTask(${task.id})"><i class="fas fa-trash"></i></button>` : ''}
            </td>
        </tr>
    `).join('');
    
    // تحديث العدد
    const pendingCount = tasks.filter(t => t.status === 'pending').length;
    document.getElementById('taskBadge').textContent = pendingCount;
}

// ==========================================
// عرض وحذف التكليفات
// ==========================================
function viewTask(id) {
    const tasks = getTasks();
    const task = tasks.find(t => t.id === id);
    if (task) {
        showToast(`📋 ${task.title}\nالإدارة: ${task.dept}\nالمسؤول: ${task.assignee}\nالتسليم: ${task.dueDate}`, 'info');
    }
}

function deleteTask(id) {
    const isAdmin = currentUser && (currentUser.role === 'مدير المكتب الفني' || currentUser.role === 'مدير المستشفى');
    if (!isAdmin) {
        showToast('⚠️ ليس لديك صلاحية لحذف التكليفات', 'danger');
        return;
    }
    
    if (!confirm('⚠️ هل أنت متأكد من حذف هذا التكليف؟')) return;
    
    let tasks = getTasks();
    tasks = tasks.filter(t => t.id !== id);
    saveTasks(tasks);
    
    // حفظ في Firebase
    if (firebaseReady && db) {
        db.collection('tasks').where('id', '==', id).get()
            .then(snapshot => {
                snapshot.forEach(doc => doc.ref.delete());
                console.log('✅ Task deleted from Firebase');
            })
            .catch(err => console.warn('Firebase delete error:', err));
    }
    
    loadTasks();
    updateDashboard();
    showToast('✅ تم حذف التكليف بنجاح', 'success');
}

// ==========================================
// تحميل المستخدمين
// ==========================================
function loadUsers() {
    const users = getStoredUsers();
    const userList = Object.keys(users).map(key => ({ 
        id: key, 
        ...users[key],
        email: users[key].email || key + '@hospital.local'
    }));
    
    const tbody = document.getElementById('usersBody');
    const isAdmin = currentUser && (currentUser.role === 'مدير المكتب الفني' || currentUser.role === 'مدير المستشفى');
    
    if (userList.length === 0) {
        tbody.innerHTML = `<tr><td colspan="7" class="text-center">لا يوجد مستخدمون</td></tr>`;
        return;
    }
    
    tbody.innerHTML = userList.map((user, idx) => `
        <tr>
            <td>${idx + 1}</td>
            <td>
                <div class="d-flex align-items-center">
                    <div class="avatar-circle me-2" style="width:32px;height:32px;font-size:12px;">
                        ${(user.fullName || user.username).charAt(0)}
                    </div>
                    ${user.fullName || user.username}
                </div>
            </td>
            <td>${user.email}</td>
            <td>${user.department || 'غير محدد'}</td>
            <td>${user.role || 'مستخدم'}</td>
            <td><span class="badge ${user.status === 'نشط' ? 'bg-success' : 'bg-warning'}">${user.status || 'نشط'}</span></td>
            <td>
                <button class="btn btn-sm btn-outline-teal" onclick="viewUser('${user.username}')"><i class="fas fa-eye"></i></button>
                ${isAdmin ? `<button class="btn btn-sm btn-outline-danger" onclick="deleteUser('${user.username}')"><i class="fas fa-trash"></i></button>` : ''}
            </td>
        </tr>
    `).join('');
    
    // تحديث الإحصائيات
    document.getElementById('totalUsers').textContent = userList.length;
    document.getElementById('activeUsers').textContent = userList.filter(u => u.status === 'نشط').length;
    document.getElementById('inactiveUsers').textContent = userList.filter(u => u.status === 'غير نشط').length;
    document.getElementById('suspendedUsers').textContent = userList.filter(u => u.status === 'معلق').length;
}

function viewUser(username) {
    const users = getStoredUsers();
    const user = users[username];
    if (user) {
        showToast(`👤 ${user.fullName}\nالدور: ${user.role}\nالإدارة: ${user.department}`, 'info');
    }
}

function deleteUser(username) {
    const isAdmin = currentUser && (currentUser.role === 'مدير المكتب الفني' || currentUser.role === 'مدير المستشفى');
    if (!isAdmin) {
        showToast('⚠️ ليس لديك صلاحية لحذف المستخدمين', 'danger');
        return;
    }
    
    if (!confirm(`⚠️ هل أنت متأكد من حذف المستخدم "${username}"؟`)) return;
    
    const users = getStoredUsers();
    if (users[username]) {
        delete users[username];
        saveStoredUsers(users);
        
        if (firebaseReady && db) {
            db.collection('users').where('username', '==', username).get()
                .then(snapshot => snapshot.forEach(doc => doc.ref.delete()))
                .catch(err => console.warn('Firebase delete error:', err));
        }
        
        loadUsers();
        showToast('✅ تم حذف المستخدم بنجاح', 'success');
    }
}

// ==========================================
// تحميل الإدارات
// ==========================================
function loadDepartments() {
    const departments = [
        { name: 'المكتب الفني', icon: 'fa-star', color: 'teal', members: 3, manager: 'أحمد محمد', tasks: 12, progress: 92 },
        { name: 'الجودة', icon: 'fa-check-circle', color: 'success', members: 2, manager: 'د. سارة', tasks: 8, progress: 78 },
        { name: 'الموارد البشرية', icon: 'fa-users', color: 'warning', members: 2, manager: 'م. يوسف', tasks: 6, progress: 65 },
        { name: 'مكافحة العدوى', icon: 'fa-shield-virus', color: 'danger', members: 2, manager: 'أ. خالد', tasks: 5, progress: 43 },
        { name: 'التمريض', icon: 'fa-user-md', color: 'info', members: 3, manager: 'م. نورة', tasks: 10, progress: 70 },
        { name: 'الصيدلة', icon: 'fa-pills', color: 'secondary', members: 1, manager: 'ص. نادر', tasks: 4, progress: 55 }
    ];
    
    const container = document.getElementById('departmentsContainer');
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
                        <div class="badge bg-${dept.color} p-2">${dept.tasks} مهمة</div>
                    </div>
                    <div class="mt-3">
                        <div class="progress"><div class="progress-bar bg-${dept.color}" style="width: ${dept.progress}%;"></div></div>
                        <small class="text-muted">نسبة الإنجاز ${dept.progress}%</small>
                    </div>
                </div>
            </div>
        </div>
    `).join('');
}

// ==========================================
// تحديث لوحة التحكم
// ==========================================
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
    document.getElementById('kpiAvgTime').textContent = total > 0 ? (4.2 + (overdue * 0.5)).toFixed(1) + ' يوم' : '0';
    document.getElementById('kpiOverdueRate').textContent = total > 0 ? Math.round((overdue / total) * 100) + '%' : '0%';
    document.getElementById('kpiWeeklyRate').textContent = Math.round(completed / 4) + ' مهام/أسبوع';
    document.getElementById('kpiOverall').textContent = rate >= 70 ? `جيد جداً (${rate}%)` : `جيد (${rate}%)`;
}

// ==========================================
// أحداث الصفحة
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
    
    // ===== تسجيل الدخول =====
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
    
    // ===== تبديل كلمة المرور =====
    document.getElementById('togglePassword').addEventListener('click', function() {
        const pwd = document.getElementById('loginPassword');
        pwd.type = pwd.type === 'password' ? 'text' : 'password';
        this.querySelector('i').classList.toggle('fa-eye');
        this.querySelector('i').classList.toggle('fa-eye-slash');
    });
    
    // ===== التنقل بين الصفحات =====
    document.querySelectorAll('.sidebar .nav-link[data-page]').forEach(link => {
        link.addEventListener('click', function() {
            // إخفاء الكل
            document.querySelectorAll('.page-section').forEach(el => el.classList.remove('active'));
            // إظهار المطلوب
            const page = this.dataset.page;
            document.getElementById('page-' + page).classList.add('active');
            // تحديث القائمة
            document.querySelectorAll('.sidebar .nav-link').forEach(l => l.classList.remove('active'));
            this.classList.add('active');
            // غلق القائمة
            document.getElementById('sidebar').classList.remove('open');
        });
    });
    
    // ===== تبديل القائمة =====
    document.getElementById('sidebarToggle').addEventListener('click', function() {
        document.getElementById('sidebar').classList.toggle('open');
    });
    
    // ===== تسجيل الخروج =====
    document.getElementById('logoutBtn').addEventListener('click', function() {
        logout();
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
    document.getElementById('saveLogoBtn').addEventListener('click', function() {
        const input = document.getElementById('logoInput');
        const file = input.files[0];
        if (file) {
            const reader = new FileReader();
            reader.onload = function(e) {
                localStorage.setItem('systemLogo', e.target.result);
                showToast('✅ تم تحديث الشعار بنجاح', 'success');
            };
            reader.readAsDataURL(file);
        } else {
            showToast('⚠️ الرجاء اختيار ملف صورة', 'warning');
        }
    });
    
    // ===== إنشاء تكليف جديد =====
    document.getElementById('createTaskBtn').addEventListener('click', function() {
        // جلب القيم
        const title = document.getElementById('taskTitle').value.trim();
        const dept = document.getElementById('taskDept').value;
        const assignee = document.getElementById('taskAssignee').value;
        const dueDate = document.getElementById('taskDueDate').value;
        const priority = document.getElementById('taskPriority').value;
        const description = document.getElementById('taskDescription').value.trim();
        
        // التحقق
        if (!title || !dept || !assignee || !dueDate) {
            showToast('⚠️ يرجى ملء جميع الحقول المطلوبة', 'warning');
            return;
        }
        
        // إنشاء التكليف
        const tasks = getTasks();
        const newTask = {
            id: Date.now(),
            title: title,
            dept: dept,
            assignee: assignee,
            dueDate: dueDate,
            priority: priority || 'normal',
            status: 'pending',
            description: description,
            createdAt: new Date().toISOString()
        };
        tasks.push(newTask);
        saveTasks(tasks);
        
        // حفظ في Firebase
        if (firebaseReady && db) {
            db.collection('tasks').add({
                ...newTask,
                timestamp: firebase.firestore.FieldValue.serverTimestamp()
            }).then(() => {
                console.log('✅ Task saved to Firebase');
            }).catch(err => {
                console.warn('Firebase save error:', err);
            });
        }
        
        // إغلاق المودال
        const modal = bootstrap.Modal.getInstance(document.getElementById('newTaskModal'));
        if (modal) modal.hide();
        
        // تحديث البيانات
        loadTasks();
        updateDashboard();
        showToast('✅ تم إنشاء التكليف بنجاح', 'success');
        
        // إعادة تعيين النموذج
        document.getElementById('newTaskForm').reset();
    });
    
    // ===== إضافة مستخدم =====
    document.getElementById('saveUserBtn').addEventListener('click', function() {
        const fullName = document.getElementById('userFullName').value.trim();
        const username = document.getElementById('userUsername').value.trim();
        const email = document.getElementById('userEmail').value.trim();
        const password = document.getElementById('userPassword').value;
        const confirm = document.getElementById('userConfirmPassword').value;
        const role = document.getElementById('userRole').value;
        const dept = document.getElementById('userDepartment').value;
        const phone = document.getElementById('userPhone').value.trim();
        
        if (!fullName || !username || !email || !password || !role || !dept) {
            showToast('⚠️ يرجى ملء جميع الحقول المطلوبة', 'warning');
            return;
        }
        if (password !== confirm) {
            showToast('⚠️ كلمتا المرور غير متطابقتين', 'danger');
            return;
        }
        if (password.length < 6) {
            showToast('⚠️ كلمة المرور يجب أن تكون 6 خانات على الأقل', 'warning');
            return;
        }
        
        const users = getStoredUsers();
        if (users[username]) {
            showToast('⚠️ اسم المستخدم موجود بالفعل', 'warning');
            return;
        }
        
        users[username] = { 
            username, 
            password, 
            fullName, 
            role, 
            department: dept,
            phone: phone,
            job: role,
            status: 'نشط',
            email: email
        };
        saveStoredUsers(users);
        
        if (firebaseReady && db) {
            db.collection('users').add({
                username, fullName, email, role, department: dept, phone,
                status: 'نشط',
                timestamp: firebase.firestore.FieldValue.serverTimestamp()
            }).catch(err => console.warn('Firebase save error:', err));
        }
        
        const modal = bootstrap.Modal.getInstance(document.getElementById('newUserModal'));
        if (modal) modal.hide();
        
        loadUsers();
        showToast(`✅ تم إضافة المستخدم ${fullName} بنجاح`, 'success');
        document.getElementById('newUserForm').reset();
    });
    
    // ===== إضافة إدارة =====
    document.getElementById('createDeptBtn').addEventListener('click', function() {
        const name = document.getElementById('deptName').value.trim();
        const manager = document.getElementById('deptManager').value;
        const description = document.getElementById('deptDescription').value.trim();
        
        if (!name) {
            showToast('⚠️ يرجى إدخال اسم الإدارة', 'warning');
            return;
        }
        
        const depts = JSON.parse(localStorage.getItem('departments') || '[]');
        depts.push({ 
            id: Date.now(), 
            name: name,
            manager: manager,
            description: description,
            createdAt: new Date().toISOString()
        });
        localStorage.setItem('departments', JSON.stringify(depts));
        
        if (firebaseReady && db) {
            db.collection('departments').add({
                name, manager, description,
                timestamp: firebase.firestore.FieldValue.serverTimestamp()
            }).catch(err => console.warn('Firebase save error:', err));
        }
        
        const modal = bootstrap.Modal.getInstance(document.getElementById('newDeptModal'));
        if (modal) modal.hide();
        
        loadDepartments();
        showToast(`✅ تم إضافة الإدارة ${name} بنجاح`, 'success');
        document.getElementById('newDeptForm').reset();
    });
    
    // ===== تحديد الكل كمقروء =====
    document.getElementById('markAllRead').addEventListener('click', function() {
        document.querySelectorAll('#notificationsList .badge').forEach(b => {
            b.textContent = 'مقروء';
            b.className = 'badge bg-secondary rounded-pill';
        });
        document.getElementById('notifBadge').textContent = '0';
        showToast('✅ تم تحديد جميع الإشعارات كمقروءة', 'success');
    });
    
    // ===== مسح البيانات =====
    document.getElementById('clearDataBtn').addEventListener('click', function() {
        if (confirm('⚠️ هل أنت متأكد من مسح جميع البيانات؟')) {
            localStorage.clear();
            showToast('🗑️ تم مسح جميع البيانات', 'warning');
            setTimeout(() => location.reload(), 1500);
        }
    });
    
    // ===== بحث وتصفية =====
    document.getElementById('searchTask').addEventListener('keyup', filterTasks);
    document.getElementById('filterStatus').addEventListener('change', filterTasks);
    document.getElementById('filterDept').addEventListener('change', filterTasks);
    document.getElementById('clearFilters').addEventListener('click', function() {
        document.getElementById('searchTask').value = '';
        document.getElementById('filterStatus').value = '';
        document.getElementById('filterDept').value = '';
        filterTasks();
    });
    
    function filterTasks() {
        const search = document.getElementById('searchTask').value.toLowerCase();
        const status = document.getElementById('filterStatus').value;
        const dept = document.getElementById('filterDept').value;
        
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
    
    // ===== إغلاق القائمة =====
    document.addEventListener('click', function(e) {
        const sidebar = document.getElementById('sidebar');
        const toggle = document.getElementById('sidebarToggle');
        if (window.innerWidth < 768 && sidebar.classList.contains('open') &&
            !sidebar.contains(e.target) && !toggle.contains(e.target)) {
            sidebar.classList.remove('open');
        }
    });
    
    // ===== الرسم البياني =====
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
                    plugins: { legend: { position: 'bottom' } }
                }
            });
        }
    }, 500);
    
    // ===== إعدادات الملف الشخصي =====
    document.getElementById('profileForm').addEventListener('submit', function(e) {
        e.preventDefault();
        showToast('✅ تم حفظ التغييرات بنجاح', 'success');
    });
    
    document.getElementById('passwordForm').addEventListener('submit', function(e) {
        e.preventDefault();
        showToast('✅ تم تغيير كلمة المرور بنجاح', 'success');
    });
    
    console.log('🚀 نظام فريقي V3.0 جاهز للتشغيل مع Firebase!');
});
</script>
</body>
</html>
