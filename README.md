<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HR Pulse AI - Công cụ Phân tích Khảo sát Mức độ Hài lòng Nhân viên (Đề tài 35)</title>
    <!-- Tailwind CSS -->
    <script src="assets/tailwind.js"></script>
    <!-- Chart.js -->
    <script src="assets/chart.js"></script>
    <link rel="stylesheet" href="assets/site.css">
    <script defer src="assets/app.js"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc;
        }
        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(226, 232, 240, 0.8);
        }
        .step-active {
            border-color: #2563eb;
            background-color: #eff6ff;
            color: #1d4ed8;
        }
        .step-done {
            border-color: #10b981;
            background-color: #ecfdf5;
            color: #047857;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="text-slate-800 antialiased">

    <!-- Top Academic & Ethics Banner -->
    <div class="bg-slate-900 text-slate-200 text-xs py-2 px-4 border-b border-slate-800">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-2">
            <div class="flex items-center gap-2">
                <span class="bg-blue-600 text-white font-semibold px-2 py-0.5 rounded">HYUTE • Đề tài 35</span>
            <div class="flex items-center gap-4">
                <span id="aiModeBadge" class="inline-flex items-center gap-1.5 text-emerald-400 font-medium">
                    <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
                </span>
            </div>
        </div>
    </div>

    <!-- Main Navigation Header -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-blue-600 to-indigo-700 flex items-center justify-center text-white shadow-md">
                    <i class="fa-solid fa-heart-pulse text-lg"></i>
                </div>
                <div>
                    <h1 class="text-lg font-bold text-slate-900 leading-tight">HR Pulse <span class="text-blue-600">AI</span></h1>
                    <p class="text-xs text-slate-500">Phân tích Mức độ Hài lòng & Giữ chân Nhân sự</p>
                </div>
            </div>

            <!-- Navigation Tabs -->
            <nav class="hidden md:flex items-center gap-1 bg-slate-100 p-1 rounded-xl">
                <button onclick="switchTab('rating')" id="tab-rating" class="nav-btn">Đánh giá</button>
                <button onclick="switchTab('dashboard')" id="tab-dashboard" class="nav-btn px-4 py-2 rounded-lg text-sm font-semibold bg-white text-blue-600 shadow-sm transition">
                    <i class="fa-solid fa-chart-pie mr-1.5"></i>Tổng quan
                </button>
                <button onclick="switchTab('hitl')" id="tab-hitl" class="nav-btn px-4 py-2 rounded-lg text-sm font-medium text-slate-600 hover:text-slate-900 transition flex items-center gap-1.5">
                    <i class="fa-solid fa-user-check"></i>Kiểm duyệt AI
                    <span id="pendingCountBadge" class="bg-amber-100 text-amber-800 text-xs font-bold px-2 py-0.5 rounded-full">0</span>
                </button>
                <button onclick="switchTab('input')" id="tab-input" class="nav-btn px-4 py-2 rounded-lg text-sm font-medium text-slate-600 hover:text-slate-900 transition">
                    <i class="fa-solid fa-file-import mr-1.5"></i>Nhập phiếu
                </button>
            </nav>

            <!-- Action Buttons -->
            <div class="flex items-center gap-2">
                <button onclick="runFullAIAnalysis()" id="btnRunAI" class="bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold px-4 py-2 rounded-xl shadow-sm transition flex items-center gap-2">
                    <i class="fa-solid fa-wand-magic-sparkles"></i>
                    <span class="hidden sm:inline">Chạy Phân tích AI</span>
                </button>
                <button onclick="exportSummaryReport()" class="bg-slate-800 hover:bg-slate-900 text-white text-sm font-medium px-3.5 py-2 rounded-xl shadow-sm transition flex items-center gap-1.5" title="Xuất báo cáo tóm tắt">
                    <i class="fa-solid fa-file-export"></i>
                    <span class="hidden lg:inline">Xuất Báo cáo</span>
                </button>
            </div>
        </div>

        <!-- Mobile Nav -->
        <div class="flex md:hidden border-t border-slate-100 bg-slate-50 px-2 py-1.5 justify-around">
            <button onclick="switchTab('rating')" class="mobile-nav text-xs px-2">Đánh giá</button>
            <button onclick="switchTab('dashboard')" class="mobile-nav text-xs font-semibold text-blue-600 py-1 px-2 rounded">
                <i class="fa-solid fa-chart-pie block text-center mb-0.5"></i>Dashboard
            </button>
            <button onclick="switchTab('hitl')" class="mobile-nav text-xs font-medium text-slate-600 py-1 px-2 rounded">
                <i class="fa-solid fa-user-check block text-center mb-0.5"></i>Kiểm duyệt AI
            </button>
            <button onclick="switchTab('input')" class="mobile-nav text-xs font-medium text-slate-600 py-1 px-2 rounded">
                <i class="fa-solid fa-plus-circle block text-center mb-0.5"></i>Thêm Khảo sát
            </button>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">
        <section id="view-rating">
            <div class="rating-path">Không gian nhân sự <span>/</span> Đánh giá nhân viên</div>
            <div class="rating-banner">
                <div class="signal" aria-hidden="true"></div>
                <small>HR PULSE AI &nbsp; / &nbsp; EMPLOYEE EXPERIENCE</small>
                <h2>Đánh giá mức độ hài lòng</h2>
                <p>Chia sẻ cảm nhận của bạn về trải nghiệm làm việc tại cửa hàng.</p>
            </div>
            <div class="rating-layout">
                <form id="ratingForm">
                    <div class="form-head"><small>PHIẾU ĐÁNH GIÁ</small><span>Không yêu cầu họ tên</span></div>
                    <fieldset><legend><b>01</b> Bạn hài lòng với công việc ở mức nào?</legend>
                        <div id="ratingChoices"></div>
                    </fieldset>
                    <label for="ratingNote" class="note-title"><b>02</b> Góp ý của bạn <em>Không bắt buộc</em></label>
                    <textarea id="ratingNote" maxlength="500" rows="3" placeholder="Điều gì đang làm tốt hoặc cần thay đổi?"></textarea>
                    <div class="rating-counter"><span id="noteLength">0</span>/500 ký tự</div>
                    <div class="form-foot"><span>Phản hồi không hiển thị công khai.</span><button type="submit">Gửi đánh giá &nbsp; →</button></div>
                    <p id="ratingStatus" role="status"></p>
                </form>
                <aside class="rating-side"><small>THÔNG TIN PHIẾU</small><h3>Đánh giá tổng thể</h3>
                    <dl><dt>Đơn vị</dt><dd>Công ty</dd><dt>Đối tượng</dt><dd>Nhân viên</dd><dt>Thang điểm</dt><dd>1 đến 5 điểm</dd></dl>
                    <div class="side-note"><strong>Quyền riêng tư</strong><p>Vui lòng không ghi tên, số điện thoại hoặc email cá nhân trong góp ý.</p></div>
                </aside>
            </div>
        </section>

        <!-- Privacy & AI Limitations Notice Banner -->
        <div class="bg-amber-50 border border-amber-200 rounded-2xl p-4 flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
            <div class="flex items-start gap-3">
                <div class="w-9 h-9 rounded-xl bg-amber-100 text-amber-700 flex items-center justify-center shrink-0 mt-0.5">
                    <i class="fa-solid fa-shield-halved text-lg"></i>
                </div>
                <div class="text-xs sm:text-sm text-amber-900 space-y-1">
                    <p class="font-bold">Lưu ý Bảo mật Dữ liệu Cá nhân (PII) & Giới hạn Trách nhiệm AI:</p>
                    <p class="text-amber-800 leading-relaxed">
                        1. <strong>Bảo mật PII:</strong> Hệ thống tự động ẩn danh hóa dữ liệu, tuyệt đối <strong>không nhập họ tên thật, số CCCD, số điện thoại hoặc email cá nhân</strong> vào nội dung khảo sát.<br>
                        2. <strong>Giới hạn AI:</strong> Kết quả phân tích cảm xúc và dự báo rủi ro nghỉ việc chỉ mang tính chất <strong>tham khảo hỗ trợ</strong>; chuyên gia nhân sự cần kiểm duyệt (Human-in-the-loop) trước khi ra quyết định.
                    </p>
                </div>
            </div>
            <div class="shrink-0 flex items-center gap-2 self-end md:self-center">
                <button onclick="toggleApiKeyModal()" class="text-xs bg-white border border-amber-300 hover:bg-amber-100 text-amber-900 font-semibold px-3 py-2 rounded-xl transition shadow-xs">
                    <i class="fa-solid fa-key mr-1"></i>Cấu hình Gemini API
                </button>
            </div>
        </div>

        <!-- 4-Step Transparent AI Pipeline Bar -->
        <section class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 mb-4">
                <div>
                    <h2 class="text-sm font-bold uppercase tracking-wider text-slate-500 flex items-center gap-2">
                        <i class="fa-solid fa-diagram-project text-blue-600"></i>
                        Luồng Xử Lý AI Minh Bạch (4 Giai Đoạn)
                    </h2>
                </div>
                <span id="pipelineStatusText" class="text-xs font-medium text-emerald-700 bg-emerald-50 px-3 py-1 rounded-full border border-emerald-200">
                    <i class="fa-solid fa-circle-check mr-1"></i>Đã hoàn tất phân tích 18 bản ghi khảo sát
                </span>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-3">
                <!-- Step 1 -->
                <div id="step-1" class="step-done border rounded-xl p-3.5 transition-all">
                    <div class="flex items-center justify-between mb-1.5">
                        <span class="text-xs font-bold uppercase">Bước 1 • Tiền xử lý</span>
                        <i class="fa-solid fa-user-secret text-base"></i>
                    </div>
                    <p class="text-sm font-semibold mb-1">Ẩn danh hóa PII & Làm sạch</p>
                    <p class="text-xs opacity-85" id="step1-desc">Tự động che email, SĐT, mã định danh; chuẩn hóa thang Likert 1–5.</p>
                </div>

                <!-- Step 2 -->
                <div id="step-2" class="step-done border rounded-xl p-3.5 transition-all">
                    <div class="flex items-center justify-between mb-1.5">
                        <span class="text-xs font-bold uppercase">Bước 2 • Định lượng</span>
                        <i class="fa-solid fa-calculator text-base"></i>
                    </div>
                    <p class="text-sm font-semibold mb-1">Thống kê & Chỉ số eNPS</p>
                    <p class="text-xs opacity-85" id="step2-desc">Tính điểm trung bình 5 trụ cột, phân nhóm Promoter / Passive / Detractor.</p>
                </div>

                <!-- Step 3 -->
                <div id="step-3" class="step-done border rounded-xl p-3.5 transition-all">
                    <div class="flex items-center justify-between mb-1.5">
                        <span class="text-xs font-bold uppercase">Bước 3 • NLP Cảm xúc</span>
                        <i class="fa-solid fa-brain text-base"></i>
                    </div>
                    <p class="text-sm font-semibold mb-1">Phân tích Ngữ nghĩa & Chủ đề</p>
                    <p class="text-xs opacity-85" id="step3-desc">Phân loại Tích cực/Trung lập/Tiêu cực, trích xuất từ khóa & rủi ro nghỉ việc.</p>
                </div>

                <!-- Step 4 -->
                <div id="step-4" class="step-done border rounded-xl p-3.5 transition-all">
                    <div class="flex items-center justify-between mb-1.5">
                        <span class="text-xs font-bold uppercase">Bước 4 • Khuyến nghị</span>
                        <i class="fa-solid fa-lightbulb text-base"></i>
                    </div>
                    <p class="text-sm font-semibold mb-1">Đề xuất Hành động Nhân sự</p>
                    <p class="text-xs opacity-85" id="step4-desc">Sinh kế hoạch cải thiện theo từng phòng ban & chuyển sang HITL kiểm duyệt.</p>
                </div>
            </div>
        </section>

        <!-- ==================== TAB 1: DASHBOARD ==================== -->
        <div id="view-dashboard" class="space-y-6">
            <!-- Filter Bar -->
            <div class="bg-white rounded-2xl border border-slate-200 p-4 flex flex-wrap items-center justify-between gap-4 shadow-xs">
                <div class="flex flex-wrap items-center gap-3">
                    <div class="flex items-center gap-2 text-sm font-semibold text-slate-700">
                        <i class="fa-solid fa-filter text-blue-600"></i>
                        <span>Bộ lọc dữ liệu:</span>
                    </div>
                    <select id="filterDept" onchange="applyFilters()" class="bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl px-3 py-2 focus:ring-2 focus:ring-blue-500 outline-none">
                        <option value="ALL">Tất cả phòng ban</option>
                        <option value="Công nghệ">Phòng Công nghệ (IT)</option>
                        <option value="Kinh doanh">Phòng Kinh doanh (Sales)</option>
                        <option value="Vận hành">Phòng Vận hành (Ops)</option>
                        <option value="Nhân sự">Phòng Nhân sự (HR)</option>
                        <option value="Kế toán">Phòng Kế toán (Finance)</option>
                    </select>
                    <select id="filterSentiment" onchange="applyFilters()" class="bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl px-3 py-2 focus:ring-2 focus:ring-blue-500 outline-none">
                        <option value="ALL">Mọi sắc thái cảm xúc</option>
                        <option value="Tích cực">Tích cực (Positive)</option>
                        <option value="Trung lập">Trung lập (Neutral)</option>
                        <option value="Tiêu cực">Tiêu cực (Negative)</option>
                    </select>
                    <select id="filterStatus" onchange="applyFilters()" class="bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-xl px-3 py-2 focus:ring-2 focus:ring-blue-500 outline-none">
                        <option value="ALL">Tất cả trạng thái duyệt</option>
                        <option value="verified">Đã kiểm duyệt (HITL)</option>
                        <option value="pending">Chờ kiểm duyệt</option>
                    </select>
                </div>
                <div class="text-xs text-slate-500">
                    Đang hiển thị <strong id="filteredCount" class="text-slate-900">18</strong> / <span id="totalCount">18</span> phiếu khảo sát
                </div>
            </div>

            <!-- KPI Cards -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <!-- KPI 1 -->
                <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
                    <div class="flex items-center justify-between">
                        <span class="text-xs font-semibold uppercase tracking-wider text-slate-500">Điểm Hài Lòng Chung</span>
                        <div class="w-9 h-9 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center">
                            <i class="fa-solid fa-star"></i>
                        </div>
                    </div>
                    <div class="mt-3 flex items-baseline gap-2">
                        <span id="kpiAvgScore" class="text-3xl font-extrabold text-slate-900">3.72</span>
                        <span class="text-sm font-medium text-slate-400">/ 5.0</span>
                    </div>
                    <p id="kpiScoreTrend" class="mt-2 text-xs text-emerald-600 font-medium">
                        <i class="fa-solid fa-arrow-trend-up mr-1"></i>Mức hài lòng Khá (74.4%)
                    </p>
                </div>

                <!-- KPI 2: eNPS -->
                <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
                    <div class="flex items-center justify-between">
                        <span class="text-xs font-semibold uppercase tracking-wider text-slate-500">Chỉ số Gắn kết (eNPS)</span>
                        <div class="w-9 h-9 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center">
                            <i class="fa-solid fa-gauge-high"></i>
                        </div>
                    </div>
                    <div class="mt-3 flex items-baseline gap-2">
                        <span id="kpiEnps" class="text-3xl font-extrabold text-indigo-600">+17</span>
                        <span class="text-xs font-medium text-slate-500">(Thang -100 đến +100)</span>
                    </div>
                    <p id="kpiEnpsDetail" class="mt-2 text-xs text-slate-600">
                        Ủng hộ: <strong>39%</strong> • Trung lập: <strong>39%</strong> • Phản đối: <strong>22%</strong>
                    </p>
                </div>

                <!-- KPI 3: Positive Rate -->
                <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
                    <div class="flex items-center justify-between">
                        <span class="text-xs font-semibold uppercase tracking-wider text-slate-500">Cảm xúc Tích cực (NLP)</span>
                        <div class="w-9 h-9 rounded-xl bg-emerald-50 text-emerald-600 flex items-center justify-center">
                            <i class="fa-solid fa-face-smile"></i>
                        </div>
                    </div>
                    <div class="mt-3 flex items-baseline gap-2">
                        <span id="kpiPositiveRate" class="text-3xl font-extrabold text-emerald-600">55.6%</span>
                        <span id="kpiPositiveCount" class="text-xs font-medium text-slate-500">(10 phản hồi)</span>
                    </div>
                    <p class="mt-2 text-xs text-slate-600">
                        Tiêu cực cần chú ý: <strong id="kpiNegativeCount" class="text-rose-600">4 phản hồi</strong>
                    </p>
                </div>

                <!-- KPI 4: Attrition Risk -->
                <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
                    <div class="flex items-center justify-between">
                        <span class="text-xs font-semibold uppercase tracking-wider text-slate-500">Cảnh báo Rủi ro Nghỉ việc</span>
                        <div class="w-9 h-9 rounded-xl bg-rose-50 text-rose-600 flex items-center justify-center">
                            <i class="fa-solid fa-triangle-exclamation"></i>
                        </div>
                    </div>
                    <div class="mt-3 flex items-baseline gap-2">
                        <span id="kpiHighRisk" class="text-3xl font-extrabold text-rose-600">4</span>
                        <span class="text-sm font-medium text-slate-500">nhân sự mức Cao</span>
                    </div>
                    <p class="mt-2 text-xs text-slate-600">
                        Đã kiểm duyệt HITL: <strong id="kpiVerifiedRate" class="text-blue-600">12/18</strong> phiếu
                    </p>
                </div>
            </div>

            <!-- Charts Grid Row 1 -->
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Radar Chart: 5 Pillars -->
                <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
                    <div class="flex items-center justify-between mb-4">
                        <div>
                            <h3 class="text-base font-bold text-slate-900">Điểm Hài Lòng Trung Bình Theo 5 Tiêu Chí</h3>
                            <p class="text-xs text-slate-500">Đánh giá trên thang Likert 1.0 – 5.0</p>
                        </div>
                        <span class="text-xs font-semibold bg-blue-50 text-blue-700 px-2.5 py-1 rounded-lg">Biểu đồ Radar</span>
                    </div>
                    <div class="relative h-72 w-full">
                        <template data-chart="criteriaChart"></template>
                    </div>
                </div>

                <!-- Bar Chart: Department Satisfaction -->
                <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
                    <div class="flex items-center justify-between mb-4">
                        <div>
                            <h3 class="text-base font-bold text-slate-900">Mức Độ Hài Lòng Theo Phòng Ban</h3>
                            <p class="text-xs text-slate-500">So sánh điểm trung bình & tỷ lệ hài lòng (%)</p>
                        </div>
                        <span class="text-xs font-semibold bg-indigo-50 text-indigo-700 px-2.5 py-1 rounded-lg">Theo Đơn Vị</span>
                    </div>
                    <div class="relative h-72 w-full">
                        <template data-chart="deptChart"></template>
                    </div>
                </div>
            </div>

            <!-- Charts Grid Row 2 + AI Recommendations -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Doughnut Chart: Sentiment Distribution -->
                <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-xs">
                    <div class="mb-4">
                        <h3 class="text-base font-bold text-slate-900">Phân Bố Cảm Xúc Phản Hồi Mở</h3>
                        <p class="text-xs text-slate-500">Kết quả phân tích NLP & hiệu chỉnh HITL</p>
                    </div>
                    <div class="relative h-60 w-full flex items-center justify-center">
                        <template data-chart="sentimentChart"></template>
                    </div>
                </div>

                <!-- AI Strategic Insights & Action Plan -->
                <div class="lg:col-span-2 bg-gradient-to-br from-slate-900 to-slate-800 text-white rounded-2xl p-6 shadow-md flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between gap-2 mb-4">
                            <div class="flex items-center gap-2.5">
                                <div class="w-8 h-8 rounded-lg bg-blue-500/20 border border-blue-400/30 flex items-center justify-center text-blue-300">
                                    <i class="fa-solid fa-robot"></i>
                                </div>
                                <div>
                                    <h3 class="text-base font-bold">Tổng Hợp Phát Hiện & Đề Xuất Hành Động Nhân Sự (AI)</h3>
                                    <p class="text-xs text-slate-400">Tự động tổng hợp từ dữ liệu định lượng và phản hồi mở</p>
                                </div>
                            </div>
                            <span class="text-xs bg-blue-600/30 border border-blue-400/30 text-blue-200 px-2.5 py-1 rounded-full">Bước 4 AI</span>
                        </div>

                        <div id="aiExecutiveSummary" class="space-y-3 text-sm text-slate-200 leading-relaxed">
                            <!-- Populated dynamically by JS -->
                        </div>
                    </div>

                    <div class="mt-5 pt-4 border-t border-slate-700/80 flex flex-wrap items-center justify-between gap-2 text-xs text-slate-400">
                        <span><i class="fa-solid fa-tags mr-1 text-blue-400"></i>Chủ đề nổi bật: <strong id="topKeywordsText" class="text-slate-200">Lương thưởng, Áp lực KPI, Quy trình, Quản lý hỗ trợ</strong></span>
                        <button onclick="switchTab('hitl')" class="text-blue-300 hover:text-white font-semibold underline">
                            Đi tới kiểm duyệt chi tiết →
                        </button>
                    </div>
                </div>
            </div>

            <!-- Detailed Survey Responses Table -->
            <div class="bg-white rounded-2xl border border-slate-200 shadow-xs overflow-hidden">
                <div class="p-5 border-b border-slate-200 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
                    <div>
                        <h3 class="text-base font-bold text-slate-900">Danh Sách Phản Hồi Khảo Sát Chi Tiết</h3>
                        <p class="text-xs text-slate-500">Dữ liệu đã được ẩn danh hóa PII; hiển thị điểm Likert, nhãn cảm xúc AI và trạng thái kiểm duyệt</p>
                    </div>
                    <div class="relative">
                        <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 text-xs"></i>
                        <input type="text" id="searchInput" oninput="applyFilters()" placeholder="Tìm từ khóa, chủ đề..." class="pl-8 pr-3 py-1.5 text-sm bg-slate-50 border border-slate-300 rounded-xl outline-none focus:ring-2 focus:ring-blue-500 w-full sm:w-64">
                    </div>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 border-b border-slate-200 text-xs font-bold uppercase text-slate-500">
                                <th class="py-3 px-4">Mã ẩn danh</th>
                                <th class="py-3 px-4">Phòng ban</th>
                                <th class="py-3 px-4 text-center">Điểm TB</th>
                                <th class="py-3 px-4">Nội dung phản hồi mở (Đã lọc PII)</th>
                                <th class="py-3 px-4">Chủ đề AI trích xuất</th>
                                <th class="py-3 px-4">Cảm xúc & Rủi ro</th>
                                <th class="py-3 px-4 text-center">Kiểm duyệt</th>
                            </tr>
                        </thead>
                        <tbody id="surveyTableBody" class="divide-y divide-slate-100 text-sm">
                            <!-- Populated by JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- ==================== TAB 2: HUMAN-IN-THE-LOOP (HITL) ==================== -->
        <div id="view-hitl" class="hidden space-y-6">
            <div class="bg-white rounded-2xl border border-slate-200 p-6 shadow-xs">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 pb-5 border-b border-slate-200">
                    <div>
                        <div class="inline-flex items-center gap-2 bg-blue-50 text-blue-700 text-xs font-bold px-3 py-1 rounded-full mb-2">
                            <i class="fa-solid fa-user-shield"></i> Human-in-the-Loop (HITL) Governance
                        </div>
                        <h2 class="text-xl font-bold text-slate-900">Kiểm Duyệt & Hiệu Chỉnh Kết Quả Phân Tích AI</h2>
                        <p class="text-sm text-slate-600 mt-1">
                            Cán bộ nhân sự đối chiếu phản hồi thực tế với nhận định của AI (đặc biệt các câu mỉa mai, ẩn ý) để xác nhận hoặc chỉnh sửa trước khi chốt báo cáo.
                        </p>
                    </div>
                    <div class="flex items-center gap-2 shrink-0">
                        <button onclick="verifyAllPending()" class="bg-emerald-600 hover:bg-emerald-700 text-white text-sm font-semibold px-4 py-2.5 rounded-xl shadow-xs transition">
                            <i class="fa-solid fa-check-double mr-1.5"></i>Xác nhận tất cả phiếu chờ
                        </button>
                    </div>
                </div>

                <!-- HITL Cards List -->
                <div id="hitlCardsContainer" class="mt-6 grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Populated dynamically -->
                </div>
            </div>
        </div>

        <!-- ==================== TAB 3: DATA INPUT & IMPORT ==================== -->
        <div id="view-input" class="hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Single Survey Entry Form -->
                <div class="bg-white rounded-2xl border border-slate-200 p-6 shadow-xs">
                    <h2 class="text-lg font-bold text-slate-900 mb-1 flex items-center gap-2">
                        <i class="fa-solid fa-pen-to-square text-blue-600"></i>
                        Nhập Phiếu Khảo Sát Mới
                    </h2>
                    <p class="text-xs text-slate-500 mb-5">Hệ thống tự động chạy bộ lọc ẩn danh hóa PII và phân tích AI ngay khi thêm.</p>

                    <form id="newSurveyForm" onsubmit="handleAddSurvey(event)" class="space-y-4">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Phòng ban công tác *</label>
                                <select id="inDept" required class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="Công nghệ">Công nghệ (IT)</option>
                                    <option value="Kinh doanh">Kinh doanh (Sales)</option>
                                    <option value="Vận hành">Vận hành (Ops)</option>
                                    <option value="Nhân sự">Nhân sự (HR)</option>
                                    <option value="Kế toán">Kế toán (Finance)</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Thâm niên (Tùy chọn)</label>
                                <select id="inTenure" class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="Dưới 1 năm">Dưới 1 năm</option>
                                    <option value="1 - 3 năm" selected>1 - 3 năm</option>
                                    <option value="Trên 3 năm">Trên 3 năm</option>
                                </select>
                            </div>
                        </div>

                        <!-- 5 Likert Sliders -->
                        <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                            <p class="text-xs font-bold uppercase text-slate-600">Đánh giá mức độ hài lòng (Thang điểm 1 = Rất không hài lòng → 5 = Rất hài lòng)</p>
                            
                            <div class="space-y-2.5 text-sm">
                                <div class="flex items-center justify-between gap-4">
                                    <label class="text-slate-700 font-medium w-44">1. Lương & Phúc lợi:</label>
                                    <input type="range" id="inComp" min="1" max="5" value="4" oninput="this.nextElementSibling.value = this.value" class="flex-1 accent-blue-600">
                                    <output class="font-bold text-blue-600 w-6 text-center">4</output>
                                </div>
                                <div class="flex items-center justify-between gap-4">
                                    <label class="text-slate-700 font-medium w-44">2. Môi trường làm việc:</label>
                                    <input type="range" id="inEnv" min="1" max="5" value="4" oninput="this.nextElementSibling.value = this.value" class="flex-1 accent-blue-600">
                                    <output class="font-bold text-blue-600 w-6 text-center">4</output>
                                </div>
                                <div class="flex items-center justify-between gap-4">
                                    <label class="text-slate-700 font-medium w-44">3. Quản lý trực tiếp:</label>
                                    <input type="range" id="inMgr" min="1" max="5" value="4" oninput="this.nextElementSibling.value = this.value" class="flex-1 accent-blue-600">
                                    <output class="font-bold text-blue-600 w-6 text-center">4</output>
                                </div>
                                <div class="flex items-center justify-between gap-4">
                                    <label class="text-slate-700 font-medium w-44">4. Cơ hội thăng tiến:</label>
                                    <input type="range" id="inGrowth" min="1" max="5" value="3" oninput="this.nextElementSibling.value = this.value" class="flex-1 accent-blue-600">
                                    <output class="font-bold text-blue-600 w-6 text-center">3</output>
                                </div>
                                <div class="flex items-center justify-between gap-4">
                                    <label class="text-slate-700 font-medium w-44">5. Cân bằng cuộc sống:</label>
                                    <input type="range" id="inWlb" min="1" max="5" value="4" oninput="this.nextElementSibling.value = this.value" class="flex-1 accent-blue-600">
                                    <output class="font-bold text-blue-600 w-6 text-center">4</output>
                                </div>
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-600 mb-1">
                                Phản hồi mở bằng văn bản *
                                <span class="text-amber-600 normal-case font-normal">(Không nhập họ tên, SĐT, email cá nhân)</span>
                            </label>
                            <textarea id="inComment" rows="3" required placeholder="Ví dụ: Đồng nghiệp thân thiện, quản lý hỗ trợ tốt nhưng quy trình duyệt chi phí còn chậm và mức thưởng chưa tương xứng..." class="w-full bg-slate-50 border border-slate-300 rounded-xl p-3 text-sm outline-none focus:ring-2 focus:ring-blue-500"></textarea>
                        </div>

                        <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2.5 rounded-xl shadow-sm transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-plus"></i>Thêm Phiếu & Phân Tích AI Ngay
                        </button>
                    </form>
                </div>

                <!-- Batch Import CSV / JSON -->
                <div class="bg-white rounded-2xl border border-slate-200 p-6 shadow-xs flex flex-col justify-between">
                    <div>
                        <h2 class="text-lg font-bold text-slate-900 mb-1 flex items-center gap-2">
                            <i class="fa-solid fa-file-csv text-emerald-600"></i>
                            Nhập Hàng Loạt (CSV / JSON) & Quản Lý Dữ Liệu
                        </h2>
                        <p class="text-xs text-slate-500 mb-4">Tải lên tệp CSV/JSON hoặc dán trực tiếp dữ liệu khảo sát để phân tích đồng loạt.</p>

                        <div class="mb-4">
                            <label class="flex flex-col items-center justify-center w-full h-28 border-2 border-dashed border-slate-300 rounded-xl cursor-pointer bg-slate-50 hover:bg-slate-100 transition">
                                <div class="flex flex-col items-center justify-center pt-4 pb-5">
                                    <i class="fa-solid fa-cloud-arrow-up text-2xl text-slate-400 mb-1.5"></i>
                                    <p class="text-xs text-slate-600 font-medium">Nhấn để chọn tệp <span class="text-blue-600 font-bold">.CSV</span> hoặc <span class="text-blue-600 font-bold">.JSON</span></p>
                                </div>
                                <input id="fileUploadInput" type="file" accept=".csv,.json" class="hidden" onchange="handleFileUpload(event)" />
                            </label>
                        </div>

                        <div class="mb-3">
                            <div class="flex items-center justify-between mb-1">
                                <label class="text-xs font-bold uppercase text-slate-600">Hoặc dán dữ liệu CSV mẫu:</label>
                                <button type="button" onclick="fillSampleCsvText()" class="text-xs text-blue-600 hover:underline font-medium">Chèn định dạng CSV mẫu</button>
                            </div>
                            <textarea id="batchRawInput" rows="5" placeholder="dept,comp,env,mgr,growth,wlb,comment&#10;Công nghệ,4,5,5,4,4,Môi trường làm việc cởi mở và nhiều công nghệ mới" class="w-full font-mono text-xs bg-slate-50 border border-slate-300 rounded-xl p-3 outline-none focus:ring-2 focus:ring-blue-500"></textarea>
                        </div>
                    </div>

                    <div class="space-y-2.5">
                        <button type="button" onclick="processBatchText()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-semibold py-2.5 rounded-xl shadow-sm transition">
                            <i class="fa-solid fa-bolt mr-1.5"></i>Nạp & Phân Tích Dữ Liệu Hàng Loạt
                        </button>
                        <button type="button" onclick="resetToDefaultSampleData()" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold py-2 rounded-xl transition">
                            <i class="fa-solid fa-rotate-left mr-1"></i>Khôi phục 18 bản ghi dữ liệu mẫu ban đầu
                        </button>
                    </div>
                </div>
            </div>
        </div>

    </main>

    <!-- Edit HITL Modal -->
    <div id="editModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-xs z-50 hidden items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full p-6 shadow-2xl border border-slate-200 space-y-4">
            <div class="flex items-center justify-between border-b border-slate-100 pb-3">
                <h3 class="text-lg font-bold text-slate-900">Hiệu Chỉnh Kết Quả AI (Human-in-the-Loop)</h3>
                <button onclick="closeEditModal()" class="text-slate-400 hover:text-slate-700"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>

            <div class="bg-slate-50 p-3 rounded-xl border border-slate-200 text-xs space-y-1">
                <p class="font-bold text-slate-700">Mã phiếu: <span id="modalSurveyId" class="text-blue-600"></span> • Phòng ban: <span id="modalSurveyDept"></span></p>
                <p id="modalSurveyComment" class="text-slate-600 italic mt-1"></p>
            </div>

            <div class="space-y-3">
                <div>
                    <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Nhãn cảm xúc sau kiểm duyệt</label>
                    <select id="modalSentiment" class="w-full bg-white border border-slate-300 rounded-xl px-3 py-2 text-sm">
                        <option value="Tích cực">Tích cực (Positive)</option>
                        <option value="Trung lập">Trung lập (Neutral)</option>
                        <option value="Tiêu cực">Tiêu cực (Negative)</option>
                    </select>
                </div>

                <div>
                    <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Mức độ rủi ro nghỉ việc</label>
                    <select id="modalRisk" class="w-full bg-white border border-slate-300 rounded-xl px-3 py-2 text-sm">
                        <option value="Thấp">Thấp (An toàn)</option>
                        <option value="Trung bình">Trung bình (Cần theo dõi)</option>
                        <option value="Cao">Cao (Cần can thiệp sớm)</option>
                    </select>
                </div>

                <div>
                    <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Chủ đề chính (phân cách bằng dấu phẩy)</label>
                    <input type="text" id="modalTopics" class="w-full bg-white border border-slate-300 rounded-xl px-3 py-2 text-sm">
                </div>

                <div>
                    <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Ghi chú của Chuyên gia Nhân sự (HR)</label>
                    <input type="text" id="modalHrNote" placeholder="Ví dụ: Đã hiệu chỉnh từ Tích cực sang Tiêu cực do nhân viên dùng câu mỉa mai..." class="w-full bg-white border border-slate-300 rounded-xl px-3 py-2 text-sm">
                </div>
            </div>

            <div class="flex justify-end gap-2 pt-3 border-t border-slate-100">
                <button onclick="closeEditModal()" class="px-4 py-2 text-sm font-medium text-slate-600 hover:bg-slate-100 rounded-xl">Hủy</button>
                <button onclick="saveHitlEdit()" class="px-4 py-2 text-sm font-semibold bg-blue-600 hover:bg-blue-700 text-white rounded-xl shadow-xs">
                    <i class="fa-solid fa-check mr-1"></i>Lưu & Xác Nhận Duyệt
                </button>
            </div>
        </div>
    </div>

    <!-- Gemini API Key Config Modal -->
    <div id="apiKeyModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-xs z-50 hidden items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-md w-full p-6 shadow-2xl border border-slate-200 space-y-4">
            <div class="flex items-center justify-between">
                <h3 class="text-lg font-bold text-slate-900">Cấu Hình Gemini API (Tùy Chọn)</h3>
                <button onclick="toggleApiKeyModal()" class="text-slate-400 hover:text-slate-700"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <p class="text-xs text-slate-600 leading-relaxed">
                Ứng dụng hỗ trợ gọi trực tiếp <strong>Google Gemini API</strong> để phân tích ngữ nghĩa sâu và sinh báo cáo khuyến nghị. Nếu để trống hoặc chạy offline, hệ thống tự động sử dụng <strong>Bộ máy NLP Cục bộ thông minh</strong> để đảm bảo hoạt động 100%.
            </p>
            <div>
                <label class="block text-xs font-bold uppercase text-slate-600 mb-1">Gemini API Key</label>
                <input type="password" id="geminiApiKeyInput" placeholder="AIzaSy..." class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm outline-none focus:ring-2 focus:ring-blue-500">
            </div>
            <div class="flex justify-end gap-2">
                <button onclick="toggleApiKeyModal()" class="px-4 py-2 text-sm text-slate-600 hover:bg-slate-100 rounded-xl">Đóng</button>
                <button onclick="saveApiKey()" class="px-4 py-2 text-sm font-semibold bg-blue-600 text-white rounded-xl">Lưu Cấu Hình</button>
            </div>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toastBox" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 bg-slate-900 text-white text-sm px-4 py-3 rounded-xl shadow-lg flex items-center gap-2.5 max-w-sm">
        <i id="toastIcon" class="fa-solid fa-circle-check text-emerald-400"></i>
        <span id="toastMessage">Thao tác thành công!</span>
    </div>

    <script>
        // =========================================================================
        // 1. SAMPLE DATASET (18 Realistic Employee Survey Records across 5 Depts)
        // =========================================================================
        const DEFAULT_SURVEY_DATA = [
            {
                id: "NV-101", dept: "Công nghệ", tenure: "1 - 3 năm",
                scores: { comp: 4, env: 5, mgr: 5, growth: 4, wlb: 4 },
                rawComment: "Môi trường làm việc cởi mở, Tech Lead hỗ trợ rất nhiệt tình. Mong công ty mở thêm ngân sách học chứng chỉ AWS.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Quản lý hỗ trợ", "Đào tạo & Phát triển", "Môi trường"],
                aiReason: "Điểm trung bình cao (4.4/5), từ khóa tích cực về quản lý và văn hóa học tập.",
                verified: true, hrNote: "Đã xác nhận đúng thực tế."
            },
            {
                id: "NV-102", dept: "Kinh doanh", tenure: "1 - 3 năm",
                scores: { comp: 2, env: 3, mgr: 3, growth: 2, wlb: 2 },
                rawComment: "Chỉ tiêu KPI quý này tăng đột biến nhưng cơ chế hoa hồng lại giảm, anh em áp lực và OT liên tục cuối tháng.",
                sentiment: "Tiêu cực", risk: "Cao", topics: ["Lương thưởng & KPI", "Cân bằng cuộc sống"],
                aiReason: "Phản ánh trực tiếp việc giảm hoa hồng và tăng áp lực OT; điểm Lương & WLB thấp (2.0).",
                verified: true, hrNote: "Chuyển Trưởng phòng Kinh doanh xem xét lại định mức KPI."
            },
            {
                id: "NV-103", dept: "Vận hành", tenure: "Trên 3 năm",
                scores: { comp: 3, env: 4, mgr: 4, growth: 3, wlb: 3 },
                rawComment: "Đồng nghiệp hòa đồng nhưng quy trình phê duyệt chứng từ giữa các bộ phận còn rườm rà, mất nhiều thời gian chờ đợi.",
                sentiment: "Trung lập", risk: "Trung bình", topics: ["Quy trình nội bộ", "Đồng nghiệp"],
                aiReason: "Ghi nhận điểm mạnh về đồng nghiệp nhưng góp ý cải tiến quy trình vận hành.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-104", dept: "Công nghệ", tenure: "Dưới 1 năm",
                scores: { comp: 5, env: 5, mgr: 4, growth: 5, wlb: 5 },
                rawComment: "Quy trình onboarding rõ ràng, trang thiết bị làm việc mạnh, chế độ hybrid linh hoạt giúp mình cân bằng cuộc sống rất tốt.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Chế độ Hybrid", "Cơ sở vật chất", "Onboarding"],
                aiReason: "Mức độ hài lòng rất cao (4.8/5) ở nhân sự mới gia nhập.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-105", dept: "Kế toán", tenure: "Trên 3 năm",
                scores: { comp: 3, env: 4, mgr: 4, growth: 2, wlb: 3 },
                rawComment: "Công việc ổn định, sếp tâm lý nhưng lộ trình thăng tiến cho khối back-office chưa thực sự rõ ràng sau 3 năm.",
                sentiment: "Trung lập", risk: "Trung bình", topics: ["Lộ trình thăng tiến", "Quản lý hỗ trợ"],
                aiReason: "Hài lòng với quản lý nhưng băn khoăn về cơ hội phát triển nghề nghiệp (2/5).",
                verified: false, hrNote: ""
            },
            {
                id: "NV-106", dept: "Kinh doanh", tenure: "Dưới 1 năm",
                scores: { comp: 2, env: 2, mgr: 2, growth: 2, wlb: 2 },
                rawComment: "Tuyệt vời quá, ngày nào cũng họp tới 8 giờ tối không lương ngoài giờ, chắc mình sớm tìm bến đỗ mới thôi!",
                sentiment: "Tiêu cực", risk: "Cao", topics: ["OT & Giờ làm việc", "Ý định nghỉ việc", "Quản lý"],
                aiReason: "Phát hiện cụm từ mỉa mai ('Tuyệt vời quá...') kèm dấu hiệu rời bỏ ('tìm bến đỗ mới').",
                verified: false, hrNote: ""
            },
            {
                id: "NV-107", dept: "Nhân sự", tenure: "1 - 3 năm",
                scores: { comp: 4, env: 5, mgr: 5, growth: 4, wlb: 4 },
                rawComment: "Văn hóa gắn kết nội bộ rất tốt, ban lãnh đạo lắng nghe ý kiến nhân viên qua các buổi townhall định kỳ.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Văn hóa doanh nghiệp", "Ban lãnh đạo"],
                aiReason: "Đánh giá tích cực về truyền thông nội bộ và sự lắng nghe của lãnh đạo.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-108", dept: "Vận hành", tenure: "1 - 3 năm",
                scores: { comp: 2, env: 3, mgr: 2, growth: 2, wlb: 2 },
                rawComment: "Khối lượng đơn hàng tăng gấp đôi nhưng nhân sự không được bổ sung, quản lý phân công thiếu công bằng giữa các ca.",
                sentiment: "Tiêu cực", risk: "Cao", topics: ["Quá tải công việc", "Quản lý trực tiếp", "Định biên nhân sự"],
                aiReason: "Bức xúc về quá tải công việc và phân công ca trực thiếu công bằng.",
                verified: false, hrNote: ""
            },
            {
                id: "NV-109", dept: "Công nghệ", tenure: "Trên 3 năm",
                scores: { comp: 4, env: 4, mgr: 5, growth: 4, wlb: 4 },
                rawComment: "Dự án có nhiều bài toán kỹ thuật thú vị, mức thưởng cuối năm xứng đáng với nỗ lực của team.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Lương thưởng", "Tính chất công việc"],
                aiReason: "Hài lòng đồng đều ở tất cả các tiêu chí (4.2/5).",
                verified: true, hrNote: ""
            },
            {
                id: "NV-110", dept: "Kế toán", tenure: "1 - 3 năm",
                scores: { comp: 4, env: 4, mgr: 4, growth: 3, wlb: 4 },
                rawComment: "Phần mềm kế toán mới giúp giảm bớt nhập liệu thủ công. Môi trường làm việc yên tĩnh, chuyên nghiệp.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Công cụ làm việc", "Môi trường"],
                aiReason: "Phản hồi tích cực về chuyển đổi số công cụ làm việc.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-111", dept: "Kinh doanh", tenure: "Trên 3 năm",
                scores: { comp: 4, env: 4, mgr: 4, growth: 4, wlb: 3 },
                rawComment: "Sản phẩm dễ bán nhờ uy tín thương hiệu, thu nhập tốt nếu đạt doanh số nhưng mùa cao điểm hơi ít thời gian cho gia đình.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Thu nhập", "Cân bằng cuộc sống"],
                aiReason: "Tổng thể tích cực (3.8/5), ghi nhận đánh đổi nhẹ về thời gian mùa cao điểm.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-112", dept: "Vận hành", tenure: "Dưới 1 năm",
                scores: { comp: 3, env: 3, mgr: 3, growth: 3, wlb: 3 },
                rawComment: "Mọi thứ ở mức tạm ổn, chưa có điểm gì quá nổi bật nhưng cũng không có vấn đề nghiêm trọng.",
                sentiment: "Trung lập", risk: "Trung bình", topics: ["Đánh giá chung"],
                aiReason: "Tất cả điểm số ở mức 3/5, nội dung trung tính.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-113", dept: "Công nghệ", tenure: "1 - 3 năm",
                scores: { comp: 3, env: 4, mgr: 4, growth: 3, wlb: 2 },
                rawComment: "Team code vui nhưng việc trực hệ thống (on-call) cuối tuần chưa có chế độ nghỉ bù rõ ràng.",
                sentiment: "Trung lập", risk: "Trung bình", topics: ["Chế độ On-call", "Cân bằng cuộc sống"],
                aiReason: "Hài lòng về đồng đội nhưng điểm WLB thấp (2/5) do trực cuối tuần.",
                verified: false, hrNote: ""
            },
            {
                id: "NV-114", dept: "Nhân sự", tenure: "Trên 3 năm",
                scores: { comp: 4, env: 5, mgr: 4, growth: 4, wlb: 5 },
                rawComment: "Chế độ bảo hiểm sức khỏe cho người thân là điểm cộng rất lớn khiến mình gắn bó lâu dài với công ty.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Phúc lợi & Bảo hiểm", "Gắn kết"],
                aiReason: "Đánh giá cao phúc lợi gia đình, mức độ gắn kết bền vững.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-115", dept: "Kinh doanh", tenure: "1 - 3 năm",
                scores: { comp: 2, env: 3, mgr: 2, growth: 2, wlb: 3 },
                rawComment: "Chính sách thanh toán hoa hồng chậm trễ gần 2 tháng làm ảnh hưởng tâm lý anh em đi thị trường.",
                sentiment: "Tiêu cực", risk: "Cao", topics: ["Chậm thanh toán thưởng", "Chính sách đãi ngộ"],
                aiReason: "Phản ánh bức xúc cụ thể về tiến độ chi trả hoa hồng.",
                verified: false, hrNote: ""
            },
            {
                id: "NV-116", dept: "Vận hành", tenure: "Trên 3 năm",
                scores: { comp: 4, env: 4, mgr: 5, growth: 4, wlb: 4 },
                rawComment: "Trưởng bộ phận luôn đứng ra bảo vệ quyền lợi anh em và lắng nghe đề xuất cải tiến kho bãi.",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Quản lý trực tiếp", "Cải tiến vận hành"],
                aiReason: "Điểm quản lý đạt tối đa (5/5), tinh thần gắn kết cao.",
                verified: true, hrNote: ""
            },
            {
                id: "NV-117", dept: "Kế toán", tenure: "Dưới 1 năm",
                scores: { comp: 3, env: 4, mgr: 3, growth: 3, wlb: 4 },
                rawComment: "Tài liệu hướng dẫn nghiệp vụ nội bộ hơi thiếu, người mới phải tự mày mò hỏi từng người khá mất thời gian.",
                sentiment: "Trung lập", risk: "Trung bình", topics: ["Tài liệu đào tạo", "Onboarding"],
                aiReason: "Góp ý mang tính xây dựng về hệ thống tài liệu hướng dẫn.",
                verified: false, hrNote: ""
            },
            {
                id: "NV-118", dept: "Công nghệ", tenure: "1 - 3 năm",
                scores: { comp: 5, env: 4, mgr: 5, growth: 5, wlb: 4 },
                rawComment: "Kỳ review lương vừa rồi rất minh bạch và công bằng dựa trên năng lực thực tế. Rất hài lòng!",
                sentiment: "Tích cực", risk: "Thấp", topics: ["Review lương minh bạch", "Cơ hội thăng tiến"],
                aiReason: "Điểm trung bình 4.6/5, khen ngợi tính minh bạch trong đánh giá năng lực.",
                verified: true, hrNote: ""
            }
        ];

        // Global Application State
        let surveyList = JSON.parse(localStorage.getItem('utehy_hr_surveys_v1')) || JSON.parse(JSON.stringify(DEFAULT_SURVEY_DATA));
        let userApiKey = localStorage.getItem('utehy_gemini_api_key') || "";
        let currentEditingId = null;

        // Chart Instances
        let criteriaChartInstance = null;
        let deptChartInstance = null;
        let sentimentChartInstance = null;

        // =========================================================================
        // 2. PII ANONYMIZATION & HYBRID NLP ENGINE (STEPS 1 -> 4)
        // =========================================================================
        function sanitizePII(text) {
            if (!text) return "";
            let cleaned = text;
            // Mask Email addresses
            cleaned = cleaned.replace(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g, "[EMAIL_ĐÃ_ẨN]");
            // Mask Vietnamese phone numbers (03x, 05x, 07x, 08x, 09x, +84)
            cleaned = cleaned.replace(/(\+84|0)(3|5|7|8|9)[0-9]{8}\b/g, "[SĐT_ĐÃ_ẨN]");
            // Mask 9-digit or 12-digit ID / CCCD numbers
            cleaned = cleaned.replace(/\b\d{9}\b|\b\d{12}\b/g, "[CCCD_ĐÃ_ẨN]");
            return cleaned.trim();
        }

        function localSmartNLPAnalyzer(scores, comment) {
            const avg = (scores.comp + scores.env + scores.mgr + scores.growth + scores.wlb) / 5;
            const lower = comment.toLowerCase();

            // Sarcasm & critical negative indicators
            const negativeKeywords = ["áp lực", "ot", "chậm", "bức xúc", "nghỉ việc", "bến đỗ mới", "không công bằng", "quá tải", "rườm rà", "giảm", "thiếu", "mệt mỏi", "tệ", "kém"];
            const positiveKeywords = ["hài lòng", "nhiệt tình", "tốt", "cởi mở", "minh bạch", "tuyệt", "xứng đáng", "gắn kết", "hỗ trợ", "thân thiện", "chuyên nghiệp", "linh hoạt"];
            const sarcasmPatterns = ["tuyệt vời quá", "quá tuyệt", "vui ghê", "sướng quá"];

            let negHits = negativeKeywords.filter(k => lower.includes(k)).length;
            let posHits = positiveKeywords.filter(k => lower.includes(k)).length;
            let hasSarcasm = sarcasmPatterns.some(s => lower.includes(s)) && (negHits > 0 || avg < 3.0);

            let sentiment = "Trung lập";
            let risk = "Trung bình";
            let aiReason = "";

            if (hasSarcasm || avg <= 2.6 || (negHits >= 2 && avg < 3.4)) {
                sentiment = "Tiêu cực";
                risk = avg <= 2.5 || lower.includes("bến đỗ") || lower.includes("nghỉ") ? "Cao" : "Trung bình";
                aiReason = hasSarcasm
                    ? "Phát hiện cấu trúc câu cảm thán mỉa mai kết hợp điểm đánh giá thấp."
                    : `Điểm trung bình thấp (${avg.toFixed(1)}/5) kèm các từ khóa phản ánh bất cập.`;
            } else if (avg >= 3.8 && negHits === 0) {
                sentiment = "Tích cực";
                risk = "Thấp";
                aiReason = `Điểm trung bình cao (${avg.toFixed(1)}/5) và ngữ nghĩa phản hồi tích cực.`;
            } else if (avg >= 3.6 && posHits > negHits) {
                sentiment = "Tích cực";
                risk = "Thấp";
                aiReason = `Tổng thể hài lòng (${avg.toFixed(1)}/5), ghi nhận phản hồi xây dựng.`;
            } else {
                sentiment = "Trung lập";
                risk = avg < 3.0 ? "Cao" : "Trung bình";
                aiReason = `Điểm số ở mức trung bình (${avg.toFixed(1)}/5) hoặc có cả điểm khen và điểm góp ý.`;
            }

            // Topic extraction
            const topics = [];
            if (lower.includes("lương") || lower.includes("thưởng") || lower.includes("hoa hồng") || lower.includes("thu nhập")) topics.push("Lương thưởng & Phúc lợi");
            if (lower.includes("sếp") || lower.includes("quản lý") || lower.includes("lead") || lower.includes("lãnh đạo")) topics.push("Quản lý trực tiếp");
            if (lower.includes("ot") || lower.includes("giờ") || lower.includes("cân bằng") || lower.includes("gia đình") || lower.includes("hybrid")) topics.push("Cân bằng cuộc sống");
            if (lower.includes("thăng tiến") || lower.includes("học") || lower.includes("đào tạo") || lower.includes("lộ trình")) topics.push("Đào tạo & Thăng tiến");
            if (lower.includes("quy trình") || lower.includes("môi trường") || lower.includes("đồng nghiệp") || lower.includes("công cụ")) topics.push("Môi trường & Quy trình");
            if (topics.length === 0) topics.push("Đánh giá chung");

            return { sentiment, risk, topics, aiReason };
        }

        async function analyzeWithGeminiOrFallback(scores, cleanComment) {
            // Always compute local fallback first for instant reliability
            const localResult = localSmartNLPAnalyzer(scores, cleanComment);
            if (!userApiKey) {
                return localResult;
            }

            try {
                const prompt = `Phân tích phiếu khảo sát nhân viên sau (thang Likert 1-5: Lương=${scores.comp}, Môi trường=${scores.env}, Quản lý=${scores.mgr}, Thăng tiến=${scores.growth}, Cân bằng=${scores.wlb}). Phản hồi mở: "${cleanComment}". Trả về JSON chính xác gồm: {"sentiment": "Tích cực"|"Trung lập"|"Tiêu cực", "risk": "Thấp"|"Trung bình"|"Cao", "topics": ["chủ đề 1", "chủ đề 2"], "aiReason": "giải thích ngắn gọn 1 câu tiếng Việt"}.`;

                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${userApiKey}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: prompt }] }],
                        generationConfig: { responseMimeType: "application/json" }
                    })
                });

                if (!response.ok) throw new Error("API Error");
                const data = await response.json();
                const text = data?.candidates?.[0]?.content?.parts?.[0]?.text;
                if (text) {
                    const parsed = JSON.parse(text);
                    return {
                        sentiment: ["Tích cực", "Trung lập", "Tiêu cực"].includes(parsed.sentiment) ? parsed.sentiment : localResult.sentiment,
                        risk: ["Thấp", "Trung bình", "Cao"].includes(parsed.risk) ? parsed.risk : localResult.risk,
                        topics: Array.isArray(parsed.topics) && parsed.topics.length ? parsed.topics.slice(0, 3) : localResult.topics,
                        aiReason: parsed.aiReason || localResult.aiReason
                    };
                }
            } catch (e) {
                // Silent fallback to local NLP engine
            }
            return localResult;
        }

        // =========================================================================
        // 3. DASHBOARD CALCULATIONS, CHARTS & EXECUTIVE SUMMARY
        // =========================================================================
        function getFilteredSurveys() {
            const dept = document.getElementById('filterDept').value;
            const sentiment = document.getElementById('filterSentiment').value;
            const status = document.getElementById('filterStatus').value;
            const keyword = document.getElementById('searchInput').value.toLowerCase().trim();

            return surveyList.filter(item => {
                if (dept !== "ALL" && item.dept !== dept) return false;
                if (sentiment !== "ALL" && item.sentiment !== sentiment) return false;
                if (status === "verified" && !item.verified) return false;
                if (status === "pending" && item.verified) return false;
                if (keyword) {
                    const matchText = item.rawComment.toLowerCase().includes(keyword) ||
                                      item.id.toLowerCase().includes(keyword) ||
                                      item.topics.some(t => t.toLowerCase().includes(keyword));
                    if (!matchText) return false;
                }
                return true;
            });
        }

        function updateAllVisuals() {
            saveToStorage();
            const filtered = getFilteredSurveys();
            document.getElementById('filteredCount').textContent = filtered.length;
            document.getElementById('totalCount').textContent = surveyList.length;

            const pendingCount = surveyList.filter(s => !s.verified).length;
            document.getElementById('pendingCountBadge').textContent = pendingCount;

            // Calculate KPIs on filtered dataset
            const n = filtered.length;
            if (n === 0) {
                document.getElementById('kpiAvgScore').textContent = "0.00";
                document.getElementById('kpiEnps').textContent = "0";
                document.getElementById('kpiPositiveRate').textContent = "0%";
                document.getElementById('kpiHighRisk').textContent = "0";
                renderSurveyTable([]);
                renderHitlCards();
                return;
            }

            let sumComp = 0, sumEnv = 0, sumMgr = 0, sumGrowth = 0, sumWlb = 0;
            let promoters = 0, passives = 0, detractors = 0;
            let posCount = 0, neuCount = 0, negCount = 0;
            let highRiskCount = 0;
            let verifiedCount = 0;

            filtered.forEach(item => {
                const s = item.scores;
                sumComp += s.comp;
                sumEnv += s.env;
                sumMgr += s.mgr;
                sumGrowth += s.growth;
                sumWlb += s.wlb;

                const avg = (s.comp + s.env + s.mgr + s.growth + s.wlb) / 5;
                if (avg >= 4.2) promoters++;
                else if (avg >= 3.2) passives++;
                else detractors++;

                if (item.sentiment === "Tích cực") posCount++;
                else if (item.sentiment === "Trung lập") neuCount++;
                else negCount++;

                if (item.risk === "Cao") highRiskCount++;
                if (item.verified) verifiedCount++;
            });

            const avgComp = sumComp / n;
            const avgEnv = sumEnv / n;
            const avgMgr = sumMgr / n;
            const avgGrowth = sumGrowth / n;
            const avgWlb = sumWlb / n;
            const overallAvg = (avgComp + avgEnv + avgMgr + avgGrowth + avgWlb) / 5;

            const pPct = Math.round((promoters / n) * 100);
            const paPct = Math.round((passives / n) * 100);
            const dPct = Math.round((detractors / n) * 100);
            const enps = pPct - dPct;

            // Update KPI Cards
            document.getElementById('kpiAvgScore').textContent = overallAvg.toFixed(2);
            const pctScore = ((overallAvg / 5) * 100).toFixed(1);
            const scoreTrendEl = document.getElementById('kpiScoreTrend');
            if (overallAvg >= 3.8) {
                scoreTrendEl.className = "mt-2 text-xs text-emerald-600 font-medium";
                scoreTrendEl.innerHTML = `<i class="fa-solid fa-arrow-trend-up mr-1"></i>Mức hài lòng Tốt (${pctScore}%)`;
            } else if (overallAvg >= 3.2) {
                scoreTrendEl.className = "mt-2 text-xs text-blue-600 font-medium";
                scoreTrendEl.innerHTML = `<i class="fa-solid fa-minus mr-1"></i>Mức hài lòng Khá (${pctScore}%)`;
            } else {
                scoreTrendEl.className = "mt-2 text-xs text-rose-600 font-medium";
                scoreTrendEl.innerHTML = `<i class="fa-solid fa-arrow-trend-down mr-1"></i>Cần cải thiện gấp (${pctScore}%)`;
            }

            const enpsEl = document.getElementById('kpiEnps');
            enpsEl.textContent = (enps > 0 ? "+" : "") + enps;
            enpsEl.className = `text-3xl font-extrabold ${enps >= 20 ? 'text-emerald-600' : enps >= 0 ? 'text-indigo-600' : 'text-rose-600'}`;
            document.getElementById('kpiEnpsDetail').innerHTML = `Ủng hộ: <strong>${pPct}%</strong> • Trung lập: <strong>${paPct}%</strong> • Phản đối: <strong>${dPct}%</strong>`;

            const posRate = ((posCount / n) * 100).toFixed(1);
            document.getElementById('kpiPositiveRate').textContent = `${posRate}%`;
            document.getElementById('kpiPositiveCount').textContent = `(${posCount} phản hồi)`;
            document.getElementById('kpiNegativeCount').textContent = `${negCount} phản hồi`;

            document.getElementById('kpiHighRisk').textContent = highRiskCount;
            document.getElementById('kpiVerifiedRate').textContent = `${verifiedCount}/${n}`;

            // Render Charts
            renderCharts(
                [avgComp.toFixed(2), avgEnv.toFixed(2), avgMgr.toFixed(2), avgGrowth.toFixed(2), avgWlb.toFixed(2)],
                [posCount, neuCount, negCount]
            );

            // Generate Executive Summary
            generateExecutiveSummary(filtered, {
                "Lương & Phúc lợi": avgComp,
                "Môi trường làm việc": avgEnv,
                "Quản lý trực tiếp": avgMgr,
                "Cơ hội thăng tiến": avgGrowth,
                "Cân bằng cuộc sống": avgWlb
            }, highRiskCount);

            // Render Table & HITL
            renderSurveyTable(filtered);
            renderHitlCards();
        }

        function renderCharts(criteriaAverages, sentimentCounts) {
            const dashboard = document.getElementById('view-dashboard');
            if (!dashboard.getClientRects().length) return;
            dashboard.querySelectorAll('template[data-chart]').forEach(slot => {
                if (document.getElementById(slot.dataset.chart)) return;
                const canvas = document.createElement('canvas');
                canvas.id = slot.dataset.chart;
                slot.after(canvas);
            });
            // 1. Radar Chart: 5 Criteria
            const ctx1 = document.getElementById('criteriaChart').getContext('2d');
            if (criteriaChartInstance) criteriaChartInstance.destroy();
            criteriaChartInstance = new Chart(ctx1, {
                type: 'radar',
                data: {
                    labels: ['Lương & Phúc lợi', 'Môi trường làm việc', 'Quản lý trực tiếp', 'Cơ hội thăng tiến', 'Cân bằng cuộc sống'],
                    datasets: [{
                        label: 'Điểm trung bình (1-5)',
                        data: criteriaAverages,
                        backgroundColor: 'rgba(37, 99, 235, 0.18)',
                        borderColor: '#2563eb',
                        pointBackgroundColor: '#2563eb',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: '#2563eb',
                        borderWidth: 2
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            min: 0,
                            max: 5,
                            ticks: { stepSize: 1, font: { size: 11 } },
                            pointLabels: { font: { size: 12, weight: '600' }, color: '#334155' }
                        }
                    },
                    plugins: {
                        legend: { display: false }
                    }
                }
            });

            // 2. Bar Chart: Department Satisfaction
            const depts = ["Công nghệ", "Nhân sự", "Kế toán", "Vận hành", "Kinh doanh"];
            const deptScores = depts.map(d => {
                const items = surveyList.filter(s => s.dept === d);
                if (!items.length) return 0;
                const total = items.reduce((acc, cur) => {
                    const s = cur.scores;
                    return acc + (s.comp + s.env + s.mgr + s.growth + s.wlb) / 5;
                }, 0);
                return (total / items.length).toFixed(2);
            });

            const ctx2 = document.getElementById('deptChart').getContext('2d');
            if (deptChartInstance) deptChartInstance.destroy();
            deptChartInstance = new Chart(ctx2, {
                type: 'bar',
                data: {
                    labels: depts,
                    datasets: [{
                        label: 'Điểm hài lòng TB',
                        data: deptScores,
                        backgroundColor: deptScores.map(val => val >= 4.0 ? '#10b981' : val >= 3.2 ? '#3b82f6' : '#f43f5e'),
                        borderRadius: 8,
                        barThickness: 32
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: { min: 0, max: 5, ticks: { stepSize: 1 } },
                        x: { grid: { display: false } }
                    },
                    plugins: {
                        legend: { display: false }
                    }
                }
            });

            // 3. Doughnut Chart: Sentiment Distribution
            const ctx3 = document.getElementById('sentimentChart').getContext('2d');
            if (sentimentChartInstance) sentimentChartInstance.destroy();
            sentimentChartInstance = new Chart(ctx3, {
                type: 'doughnut',
                data: {
                    labels: ['Tích cực', 'Trung lập', 'Tiêu cực'],
                    datasets: [{
                        data: sentimentCounts,
                        backgroundColor: ['#10b981', '#f59e0b', '#f43f5e'],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '65%',
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: { padding: 16, usePointStyle: true, font: { size: 12, weight: '600' } }
                        }
                    }
                }
            });
        }

        function generateExecutiveSummary(data, criteriaMap, highRiskCount) {
            const entries = Object.entries(criteriaMap).sort((a, b) => b[1] - a[1]);
            const strongest = entries[0];
            const weakest = entries[entries.length - 1];

            // Find dept with lowest score
            const deptGroups = {};
            data.forEach(item => {
                if (!deptGroups[item.dept]) deptGroups[item.dept] = [];
                const avg = (item.scores.comp + item.scores.env + item.scores.mgr + item.scores.growth + item.scores.wlb) / 5;
                deptGroups[item.dept].push(avg);
            });

            let lowestDept = "Chưa xác định";
            let lowestDeptScore = 5;
            Object.entries(deptGroups).forEach(([d, arr]) => {
                const mean = arr.reduce((a, b) => a + b, 0) / arr.length;
                if (mean < lowestDeptScore) {
                    lowestDeptScore = mean;
                    lowestDept = d;
                }
            });

            // Count topic frequencies
            const topicFreq = {};
            data.forEach(item => {
                item.topics.forEach(t => {
                    topicFreq[t] = (topicFreq[t] || 0) + 1;
                });
            });
            const topTopics = Object.entries(topicFreq).sort((a, b) => b[1] - a[1]).slice(0, 4).map(x => x[0]);
            document.getElementById('topKeywordsText').textContent = topTopics.join(", ") || "Chưa có dữ liệu";

            const summaryHTML = `
                <div class="bg-white/5 border border-white/10 rounded-xl p-3.5">
                    <p class="font-semibold text-emerald-300 mb-1"><i class="fa-solid fa-circle-check mr-1.5"></i>Điểm sáng nổi bật:</p>
                    <p class="text-xs sm:text-sm text-slate-200">Tiêu chí <strong>${strongest[0]}</strong> đạt điểm cao nhất (<strong>${strongest[1].toFixed(2)}/5.0</strong>), tạo nền tảng giữ chân nhân sự tích cực.</p>
                </div>
                <div class="bg-white/5 border border-white/10 rounded-xl p-3.5">
                    <p class="font-semibold text-amber-300 mb-1"><i class="fa-solid fa-triangle-exclamation mr-1.5"></i>Điểm nghẽn cần ưu tiên xử lý:</p>
                    <p class="text-xs sm:text-sm text-slate-200">Tiêu chí <strong>${weakest[0]}</strong> có điểm thấp nhất (<strong>${weakest[1].toFixed(2)}/5.0</strong>). Bộ phận <strong>${lowestDept}</strong> đang có mức hài lòng thấp nhất (<strong>${lowestDeptScore.toFixed(2)}/5.0</strong>) với <strong>${highRiskCount}</strong> nhân sự có nguy cơ nghỉ việc cao.</p>
                </div>
                <div class="bg-blue-500/15 border border-blue-400/25 rounded-xl p-3.5">
                    <p class="font-semibold text-blue-300 mb-1"><i class="fa-solid fa-list-check mr-1.5"></i>Đề xuất hành động cho HR & Quản lý:</p>
                    <ul class="list-disc list-inside text-xs sm:text-sm text-slate-200 space-y-1">
                        <li>Tổ chức đối thoại 1-1 ẩn danh tại phòng <strong>${lowestDept}</strong> để tháo gỡ áp lực KPI và tiến độ chi trả phúc lợi.</li>
                        <li>Chuẩn hóa quy trình đánh giá/lộ trình thăng tiến định kỳ 6 tháng cho khối hỗ trợ & vận hành.</li>
                    </ul>
                </div>
            `;
            document.getElementById('aiExecutiveSummary').innerHTML = summaryHTML;
        }

        // =========================================================================
        // 4. TABLE & HUMAN-IN-THE-LOOP (HITL) RENDERING
        // =========================================================================
        function renderSurveyTable(list) {
            const tbody = document.getElementById('surveyTableBody');
            if (!list.length) {
                tbody.innerHTML = `<tr><td colspan="7" class="py-8 text-center text-slate-400">Không tìm thấy bản ghi khảo sát nào phù hợp với bộ lọc.</td></tr>`;
                return;
            }

            tbody.innerHTML = list.map(item => {
                const s = item.scores;
                const avg = ((s.comp + s.env + s.mgr + s.growth + s.wlb) / 5).toFixed(1);

                const sentimentBadge = item.sentiment === "Tích cực"
                    ? `<span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-50 text-emerald-700 border border-emerald-200">Tích cực</span>`
                    : item.sentiment === "Tiêu cực"
                    ? `<span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-rose-50 text-rose-700 border border-rose-200">Tiêu cực</span>`
                    : `<span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-amber-50 text-amber-700 border border-amber-200">Trung lập</span>`;

                const riskBadge = item.risk === "Cao"
                    ? `<span class="text-xs font-bold text-rose-600 block mt-1"><i class="fa-solid fa-fire mr-1"></i>Rủi ro Cao</span>`
                    : item.risk === "Trung bình"
                    ? `<span class="text-xs font-medium text-amber-600 block mt-1">Rủi ro TB</span>`
                    : `<span class="text-xs text-slate-400 block mt-1">An toàn</span>`;

                const statusBadge = item.verified
                    ? `<span class="inline-flex items-center gap-1 text-xs font-semibold text-emerald-700 bg-emerald-50 px-2.5 py-1 rounded-lg border border-emerald-200"><i class="fa-solid fa-circle-check"></i>Đã duyệt</span>`
                    : `<button onclick="openEditModal('${item.id}')" class="inline-flex items-center gap-1 text-xs font-semibold text-amber-800 bg-amber-100 hover:bg-amber-200 px-2.5 py-1 rounded-lg border border-amber-300 transition"><i class="fa-solid fa-clock"></i>Cần duyệt</button>`;

                return `
                    <tr class="hover:bg-slate-50/80 transition">
                        <td class="py-3.5 px-4 font-mono text-xs font-bold text-slate-700">${item.id}</td>
                        <td class="py-3.5 px-4 font-medium text-slate-800">${item.dept}<span class="block text-xs text-slate-400 font-normal">${item.tenure || '1-3 năm'}</span></td>
                        <td class="py-3.5 px-4 text-center">
                            <span class="font-bold text-slate-900">${avg}</span><span class="text-xs text-slate-400">/5</span>
                        </td>
                        <td class="py-3.5 px-4 max-w-md">
                            <p class="text-slate-700 leading-snug">${item.rawComment}</p>
                            ${item.hrNote ? `<p class="text-xs text-blue-600 mt-1"><i class="fa-solid fa-comment-dots mr-1"></i>HR: ${item.hrNote}</p>` : ''}
                        </td>
                        <td class="py-3.5 px-4">
                            <div class="flex flex-wrap gap-1">
                                ${item.topics.map(t => `<span class="text-xs bg-slate-100 text-slate-700 px-2 py-0.5 rounded-md border border-slate-200">${t}</span>`).join('')}
                            </div>
                        </td>
                        <td class="py-3.5 px-4">${sentimentBadge}${riskBadge}</td>
                        <td class="py-3.5 px-4 text-center">${statusBadge}</td>
                    </tr>
                `;
            }).join('');
        }

        function renderHitlCards() {
            const container = document.getElementById('hitlCardsContainer');
            // Sort pending items first
            const sorted = [...surveyList].sort((a, b) => Number(a.verified) - Number(b.verified));

            container.innerHTML = sorted.map(item => {
                const s = item.scores;
                const avg = ((s.comp + s.env + s.mgr + s.growth + s.wlb) / 5).toFixed(1);

                const borderClass = item.verified ? 'border-slate-200 bg-slate-50/50' : 'border-amber-300 bg-amber-50/20 shadow-xs';
                const sentimentColor = item.sentiment === "Tích cực" ? 'text-emerald-700 bg-emerald-100' : item.sentiment === "Tiêu cực" ? 'text-rose-700 bg-rose-100' : 'text-amber-700 bg-amber-100';

                return `
                    <div class="border ${borderClass} rounded-2xl p-4 flex flex-col justify-between gap-3 transition">
                        <div>
                            <div class="flex items-center justify-between gap-2 mb-2">
                                <div class="flex items-center gap-2">
                                    <span class="font-mono text-xs font-bold bg-slate-800 text-white px-2 py-0.5 rounded">${item.id}</span>
                                    <span class="text-xs font-bold text-slate-700">${item.dept}</span>
                                    <span class="text-xs text-slate-500">• Điểm TB: <strong>${avg}/5</strong></span>
                                </div>
                                ${item.verified
                                    ? `<span class="text-xs font-bold text-emerald-700 bg-emerald-100 px-2.5 py-0.5 rounded-full"><i class="fa-solid fa-check mr-1"></i>Đã kiểm duyệt</span>`
                                    : `<span class="text-xs font-bold text-amber-800 bg-amber-200 px-2.5 py-0.5 rounded-full animate-pulse">Chờ HR xác nhận</span>`
                                }
                            </div>

                            <p class="text-sm text-slate-800 bg-white p-3 rounded-xl border border-slate-200/80 italic mb-2.5">
                                "${item.rawComment}"
                            </p>

                            <div class="text-xs space-y-1.5 text-slate-600">
                                <div class="flex flex-wrap items-center gap-2">
                                    <span>Nhận định AI:</span>
                                    <span class="font-bold px-2 py-0.5 rounded ${sentimentColor}">${item.sentiment}</span>
                                    <span>• Rủi ro nghỉ việc: <strong class="${item.risk === 'Cao' ? 'text-rose-600' : 'text-slate-800'}">${item.risk}</strong></span>
                                </div>
                                <p class="text-slate-500"><i class="fa-solid fa-robot text-blue-500 mr-1"></i>Lý giải AI: ${item.aiReason}</p>
                                ${item.hrNote ? `<p class="text-blue-700 font-medium"><i class="fa-solid fa-user-check mr-1"></i>Ghi chú HR: ${item.hrNote}</p>` : ''}
                            </div>
                        </div>

                        <div class="pt-3 border-t border-slate-200/80 flex items-center justify-end gap-2">
                            <button onclick="openEditModal('${item.id}')" class="px-3 py-1.5 text-xs font-semibold text-slate-700 bg-white hover:bg-slate-100 border border-slate-300 rounded-xl transition">
                                <i class="fa-solid fa-pen mr-1"></i>Chỉnh sửa kết quả AI
                            </button>
                            ${!item.verified ? `
                                <button onclick="confirmSingleHitl('${item.id}')" class="px-3 py-1.5 text-xs font-semibold text-white bg-emerald-600 hover:bg-emerald-700 rounded-xl shadow-xs transition">
                                    <i class="fa-solid fa-check mr-1"></i>Xác nhận đúng
                                </button>
                            ` : ''}
                        </div>
                    </div>
                `;
            }).join('');
        }

        // =========================================================================
        // 5. INTERACTIVE ACTIONS, PIPELINE ANIMATION & EXPORT
        // =========================================================================
        function switchTab(tabName) {
            document.body.classList.toggle('rating-mode', tabName === 'rating');
            document.querySelectorAll('.mobile-nav').forEach((btn, i) => btn.setAttribute('aria-current', ['rating','dashboard','hitl','input'][i] === tabName ? 'page' : 'false'));
            ['rating', 'dashboard', 'hitl', 'input'].forEach(t => {
                document.getElementById(`view-${t}`).classList.add('hidden');
                const btn = document.getElementById(`tab-${t}`);
                if (btn) {
                    btn.className = "nav-btn px-4 py-2 rounded-lg text-sm font-medium text-slate-600 hover:text-slate-900 transition flex items-center gap-1.5";
                }
            });

            document.getElementById(`view-${tabName}`).classList.remove('hidden');
            if (tabName === 'dashboard') {
                updateAllVisuals();
            } else {
                [criteriaChartInstance, deptChartInstance, sentimentChartInstance].forEach(chart => {
                    if (!chart) return;
                    const canvas = chart.canvas;
                    chart.destroy();
                    canvas.remove();
                });
                criteriaChartInstance = deptChartInstance = sentimentChartInstance = null;
            }
            const activeBtn = document.getElementById(`tab-${tabName}`);
            if (activeBtn) {
                activeBtn.className = "nav-btn px-4 py-2 rounded-lg text-sm font-semibold bg-white text-blue-600 shadow-sm transition flex items-center gap-1.5";
            }
        }

        function applyFilters() {
            updateAllVisuals();
        }

        async function runFullAIAnalysis() {
            const btn = document.getElementById('btnRunAI');
            const statusText = document.getElementById('pipelineStatusText');
            btn.disabled = true;
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin"></i><span>Đang xử lý 4 bước...</span>`;

            // Visual step-by-step progression
            const steps = ['step-1', 'step-2', 'step-3', 'step-4'];
            steps.forEach(s => {
                document.getElementById(s).className = "border border-slate-200 bg-slate-50 text-slate-400 rounded-xl p-3.5 transition-all";
            });

            for (let i = 0; i < steps.length; i++) {
                const el = document.getElementById(steps[i]);
                el.className = "step-active border rounded-xl p-3.5 transition-all shadow-xs";
                statusText.textContent = `Đang chạy Bước ${i + 1}/4...`;
                await new Promise(r => setTimeout(r, 320));
                el.className = "step-done border rounded-xl p-3.5 transition-all";
            }

            // Re-run sanitization & NLP on unverified items
            for (let item of surveyList) {
                item.rawComment = sanitizePII(item.rawComment);
                if (!item.verified) {
                    const res = await analyzeWithGeminiOrFallback(item.scores, item.rawComment);
                    item.sentiment = res.sentiment;
                    item.risk = res.risk;
                    item.topics = res.topics;
                    item.aiReason = res.aiReason;
                }
            }

            updateAllVisuals();
            statusText.innerHTML = `<i class="fa-solid fa-circle-check mr-1"></i>Đã hoàn tất phân tích ${surveyList.length} bản ghi khảo sát`;
            btn.disabled = false;
            btn.innerHTML = `<i class="fa-solid fa-wand-magic-sparkles"></i><span class="hidden sm:inline">Chạy Phân tích AI</span>`;
            showToast("Đã hoàn tất chu trình phân tích AI 4 bước!");
        }

        async function handleAddSurvey(e) {
            e.preventDefault();
            const dept = document.getElementById('inDept').value;
            const tenure = document.getElementById('inTenure').value;
            const scores = {
                comp: parseInt(document.getElementById('inComp').value, 10),
                env: parseInt(document.getElementById('inEnv').value, 10),
                mgr: parseInt(document.getElementById('inMgr').value, 10),
                growth: parseInt(document.getElementById('inGrowth').value, 10),
                wlb: parseInt(document.getElementById('inWlb').value, 10)
            };
            const rawInput = document.getElementById('inComment').value;
            const cleanComment = sanitizePII(rawInput);

            const analysis = await analyzeWithGeminiOrFallback(scores, cleanComment);
            const newId = `NV-${101 + surveyList.length}`;

            surveyList.unshift({
                id: newId,
                dept,
                tenure,
                scores,
                rawComment: cleanComment,
                sentiment: analysis.sentiment,
                risk: analysis.risk,
                topics: analysis.topics,
                aiReason: analysis.aiReason,
                verified: false,
                hrNote: ""
            });

            document.getElementById('newSurveyForm').reset();
            updateAllVisuals();
            switchTab('dashboard');
            showToast(`Đã thêm phiếu ${newId} (Đã lọc PII & phân tích AI)!`);
        }

        function fillSampleCsvText() {
            document.getElementById('batchRawInput').value =
`dept,comp,env,mgr,growth,wlb,comment
Công nghệ,4,5,4,4,4,Team hỗ trợ nhiệt tình nhưng cần nâng cấp server test
Kinh doanh,2,3,2,2,2,Áp lực doanh số cao và thưởng quý thanh toán chậm
Nhân sự,5,5,5,4,5,Môi trường gắn kết và phúc lợi chăm sóc sức khỏe rất tốt`;
        }

        async function processBatchText() {
            const raw = document.getElementById('batchRawInput').value.trim();
            if (!raw) {
                showToast("Vui lòng nhập hoặc tải lên dữ liệu CSV/JSON!", true);
                return;
            }

            let added = 0;
            try {
                if (raw.startsWith('[')) {
                    const parsed = JSON.parse(raw);
                    for (const row of parsed) {
                        const scores = {
                            comp: Number(row.comp) || 3,
                            env: Number(row.env) || 3,
                            mgr: Number(row.mgr) || 3,
                            growth: Number(row.growth) || 3,
                            wlb: Number(row.wlb) || 3
                        };
                        const cleanComment = sanitizePII(row.comment || "Không có bình luận");
                        const ai = await analyzeWithGeminiOrFallback(scores, cleanComment);
                        surveyList.unshift({
                            id: `NV-${101 + surveyList.length}`,
                            dept: row.dept || "Vận hành",
                            tenure: row.tenure || "1 - 3 năm",
                            scores,
                            rawComment: cleanComment,
                            sentiment: ai.sentiment,
                            risk: ai.risk,
                            topics: ai.topics,
                            aiReason: ai.aiReason,
                            verified: false,
                            hrNote: ""
                        });
                        added++;
                    }
                } else {
                    const lines = raw.split(/\r?\n/).filter(l => l.trim());
                    for (let i = 0; i < lines.length; i++) {
                        if (i === 0 && lines[i].toLowerCase().includes('dept')) continue;
                        const cols = lines[i].split(',');
                        if (cols.length >= 7) {
                            const scores = {
                                comp: Math.min(5, Math.max(1, parseInt(cols[1]) || 3)),
                                env: Math.min(5, Math.max(1, parseInt(cols[2]) || 3)),
                                mgr: Math.min(5, Math.max(1, parseInt(cols[3]) || 3)),
                                growth: Math.min(5, Math.max(1, parseInt(cols[4]) || 3)),
                                wlb: Math.min(5, Math.max(1, parseInt(cols[5]) || 3))
                            };
                            const cleanComment = sanitizePII(cols.slice(6).join(','));
                            const ai = await analyzeWithGeminiOrFallback(scores, cleanComment);
                            surveyList.unshift({
                                id: `NV-${101 + surveyList.length}`,
                                dept: cols[0].trim() || "Công nghệ",
                                tenure: "1 - 3 năm",
                                scores,
                                rawComment: cleanComment,
                                sentiment: ai.sentiment,
                                risk: ai.risk,
                                topics: ai.topics,
                                aiReason: ai.aiReason,
                                verified: false,
                                hrNote: ""
                            });
                            added++;
                        }
                    }
                }
            } catch (err) {
                showToast("Định dạng dữ liệu chưa hợp lệ, vui lòng kiểm tra lại!", true);
                return;
            }

            if (added > 0) {
                document.getElementById('batchRawInput').value = "";
                updateAllVisuals();
                switchTab('dashboard');
                showToast(`Đã nạp và phân tích thành công ${added} phiếu khảo sát!`);
            } else {
                showToast("Không đọc được bản ghi hợp lệ nào.", true);
            }
        }

        function handleFileUpload(e) {
            const file = e.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(evt) {
                document.getElementById('batchRawInput').value = evt.target.result;
                showToast(`Đã đọc tệp ${file.name}. Nhấn "Nạp & Phân tích" để xử lý.`);
            };
            reader.readAsText(file);
        }

        function resetToDefaultSampleData() {
            surveyList = JSON.parse(JSON.stringify(DEFAULT_SURVEY_DATA));
            updateAllVisuals();
            showToast("Đã khôi phục 18 bản ghi khảo sát mẫu chuẩn!");
        }

        // HITL Modal Handlers
        function confirmSingleHitl(id) {
            const item = surveyList.find(s => s.id === id);
            if (item) {
                item.verified = true;
                updateAllVisuals();
                showToast(`Đã xác nhận kết quả AI cho phiếu ${id}!`);
            }
        }

        function verifyAllPending() {
            surveyList.forEach(s => s.verified = true);
            updateAllVisuals();
            showToast("Đã xác nhận kiểm duyệt toàn bộ các phiếu khảo sát!");
        }

        function openEditModal(id) {
            const item = surveyList.find(s => s.id === id);
            if (!item) return;
            currentEditingId = id;
            document.getElementById('modalSurveyId').textContent = item.id;
            document.getElementById('modalSurveyDept').textContent = item.dept;
            document.getElementById('modalSurveyComment').textContent = `"${item.rawComment}"`;
            document.getElementById('modalSentiment').value = item.sentiment;
            document.getElementById('modalRisk').value = item.risk;
            document.getElementById('modalTopics').value = item.topics.join(', ');
            document.getElementById('modalHrNote').value = item.hrNote || '';

            const modal = document.getElementById('editModal');
            modal.classList.remove('hidden');
            modal.classList.add('flex');
        }

        function closeEditModal() {
            const modal = document.getElementById('editModal');
            modal.classList.add('hidden');
            modal.classList.remove('flex');
            currentEditingId = null;
        }

        function saveHitlEdit() {
            if (!currentEditingId) return;
            const item = surveyList.find(s => s.id === currentEditingId);
            if (item) {
                item.sentiment = document.getElementById('modalSentiment').value;
                item.risk = document.getElementById('modalRisk').value;
                const rawTopics = document.getElementById('modalTopics').value.split(',').map(t => t.trim()).filter(Boolean);
                item.topics = rawTopics.length ? rawTopics : ["Đánh giá chung"];
                item.hrNote = document.getElementById('modalHrNote').value.trim();
                item.verified = true;
            }
            closeEditModal();
            updateAllVisuals();
            showToast("Đã lưu hiệu chỉnh của chuyên gia nhân sự!");
        }

        // API Key Modal
        function toggleApiKeyModal() {
            const modal = document.getElementById('apiKeyModal');
            if (modal.classList.contains('hidden')) {
                document.getElementById('geminiApiKeyInput').value = userApiKey;
                modal.classList.remove('hidden');
                modal.classList.add('flex');
            } else {
                modal.classList.add('hidden');
                modal.classList.remove('flex');
            }
        }

        function saveApiKey() {
            userApiKey = document.getElementById('geminiApiKeyInput').value.trim();
            localStorage.setItem('utehy_gemini_api_key', userApiKey);
            toggleApiKeyModal();
            showToast(userApiKey ? "Đã lưu Gemini API Key!" : "Đã chuyển sang chế độ NLP Cục bộ!");
        }

        // Export Summary Report (Text/Markdown Download)
        function exportSummaryReport() {
            const total = surveyList.length;
            const verified = surveyList.filter(s => s.verified).length;
            const avgScore = document.getElementById('kpiAvgScore').textContent;
            const enps = document.getElementById('kpiEnps').textContent;
            const posRate = document.getElementById('kpiPositiveRate').textContent;
            const highRisk = document.getElementById('kpiHighRisk').textContent;

            let report = `BÁO CÁO PHÂN TÍCH MỨC ĐỘ HÀI LÒNG NHÂN VIÊN (HR PULSE AI)\n`;
            report += `Dự án: Năng lực số và Ứng dụng AI - Đề tài 35 (UTEHY)\n`;
            report += `Ngày xuất báo cáo: ${new Date().toLocaleString('vi-VN')}\n`;
            report += `====================================================================\n\n`;
            report += `1. TỔNG QUAN CHỈ SỐ CHÍNH:\n`;
            report += `- Tổng số phiếu khảo sát đã ẩn danh hóa PII: ${total} phiếu\n`;
            report += `- Tỷ lệ đã được cán bộ nhân sự kiểm duyệt (HITL): ${verified}/${total} phiếu\n`;
            report += `- Điểm hài lòng trung bình chung: ${avgScore} / 5.0\n`;
            report += `- Chỉ số gắn kết nhân viên (eNPS): ${enps}\n`;
            report += `- Tỷ lệ phản hồi cảm xúc tích cực: ${posRate}\n`;
            report += `- Số lượng nhân sự có cảnh báo rủi ro nghỉ việc mức Cao: ${highRisk}\n\n`;
            report += `2. CHI TIẾT PHẢN HỒI CẦN LƯU Ý (RỦI RO CAO / TIÊU CỰC):\n`;

            surveyList.filter(s => s.sentiment === "Tiêu cực" || s.risk === "Cao").forEach(item => {
                report += `- [${item.id} | ${item.dept}] Cảm xúc: ${item.sentiment} | Rủi ro: ${item.risk} | Trạng thái: ${item.verified ? 'Đã duyệt' : 'Chờ duyệt'}\n`;
                report += `  Nội dung: "${item.rawComment}"\n`;
                if (item.hrNote) report += `  Ghi chú HR: ${item.hrNote}\n`;
            });

            report += `\n3. LƯU Ý QUẢN TRỊ:\n`;
            report += `Báo cáo được tổng hợp với sự hỗ trợ của AI và đã trải qua bước kiểm duyệt Human-in-the-Loop. Dữ liệu cá nhân (PII) đã được ẩn danh hóa theo quy định bảo mật.\n`;

            const blob = new Blob([report], { type: 'text/plain;charset=utf-8' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `Bao_Cao_Hai_Long_Nhan_Vien_DeTai35.txt`;
            a.click();
            URL.revokeObjectURL(url);
            showToast("Đã xuất tệp báo cáo tóm tắt thành công!");
        }

        function saveToStorage() {
            localStorage.setItem('utehy_hr_surveys_v1', JSON.stringify(surveyList));
        }

        function showToast(msg, isError = false) {
            const box = document.getElementById('toastBox');
            const icon = document.getElementById('toastIcon');
            document.getElementById('toastMessage').textContent = msg;
            icon.className = isError ? "fa-solid fa-circle-exclamation text-rose-400" : "fa-solid fa-circle-check text-emerald-400";
            box.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => {
                box.classList.add('translate-y-20', 'opacity-0');
            }, 3200);
        }

        // Initialize on Window Load
        window.onload = function() {
            updateAllVisuals();
        };
    </script>
</body>
</html>
