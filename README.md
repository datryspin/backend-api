<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EduPulse - University Student Portal</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Lucide Icons CDN -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#eff6ff',
                            100: '#dbeafe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                            800: '#1e40af',
                            900: '#1e3a8a',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        .dark .custom-scrollbar::-webkit-scrollbar-thumb { background: #334155; }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 dark:bg-gray-900 dark:text-gray-100 min-h-screen flex flex-col transition-colors duration-200">

    <!-- Top Navigation Bar -->
    <header class="bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700 sticky top-0 z-30 transition-colors duration-200">
        <div class="px-4 sm:px-6 lg:px-8 py-3 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <button id="mobile-menu-btn" class="lg:hidden p-2 rounded-lg text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700">
                    <i data-lucide="menu" class="w-6 h-6"></i>
                </button>
                <div class="flex items-center space-x-2">
                    <div class="bg-brand-600 text-white p-2 rounded-xl shadow-md">
                        <i data-lucide="graduation-cap" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <span class="font-bold text-xl text-gray-900 dark:text-white tracking-tight">Edu<span class="text-brand-600">Pulse</span></span>
                        <span class="hidden sm:inline-block text-xs text-gray-500 dark:text-gray-400 ml-2 border-l border-gray-300 dark:border-gray-600 pl-2">Student Portal</span>
                    </div>
                </div>
            </div>

            <!-- Search Bar -->
            <div class="hidden md:flex flex-1 max-w-md mx-8 relative">
                <i data-lucide="search" class="w-4 h-4 absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
                <input type="text" id="global-search" placeholder="Search courses, grades, notices..." class="w-full pl-9 pr-4 py-1.5 text-sm bg-gray-100 dark:bg-gray-700/50 border border-transparent dark:border-gray-600 rounded-full focus:outline-none focus:bg-white dark:focus:bg-gray-800 focus:border-brand-500 transition-all">
            </div>

            <div class="flex items-center space-x-2 sm:space-x-4">
                <!-- Dark Mode Toggle -->
                <button id="theme-toggle" class="p-2 text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition">
                    <i data-lucide="sun" class="w-5 h-5 hidden dark:block"></i>
                    <i data-lucide="moon" class="w-5 h-5 block dark:hidden"></i>
                </button>

                <!-- Notifications Dropdown -->
                <div class="relative">
                    <button id="notif-btn" class="p-2 text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-200 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition relative">
                        <i data-lucide="bell" class="w-5 h-5"></i>
                        <span id="notif-badge" class="absolute top-1.5 right-1.5 w-2 h-2 bg-red-500 rounded-full"></span>
                    </button>
                    <!-- Popover -->
                    <div id="notif-dropdown" class="hidden absolute right-0 mt-2 w-80 bg-white dark:bg-gray-800 rounded-xl shadow-xl border border-gray-200 dark:border-gray-700 py-2 z-50">
                        <div class="px-4 py-2 border-b border-gray-100 dark:border-gray-700 flex justify-between items-center">
                            <span class="font-semibold text-sm">Notifications</span>
                            <span class="text-xs text-brand-600 cursor-pointer hover:underline" onclick="markAllNotificationsRead()">Mark all read</span>
                        </div>
                        <div id="notif-list" class="max-h-64 overflow-y-auto custom-scrollbar divide-y divide-gray-100 dark:divide-gray-700/50">
                            <!-- Populated dynamically -->
                        </div>
                    </div>
                </div>

                <!-- Profile Dropdown -->
                <div class="flex items-center space-x-3 pl-2 border-l border-gray-200 dark:border-gray-700">
                    <img id="user-avatar" src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&q=80&w=150" alt="Avatar" class="w-9 h-9 rounded-full object-cover ring-2 ring-brand-500/20">
                    <div class="hidden sm:block text-left">
                        <p id="user-name" class="text-xs font-semibold leading-none text-gray-900 dark:text-white">Alex Morgan</p>
                        <p id="user-id" class="text-[10px] text-gray-500 dark:text-gray-400 leading-tight mt-1">ID: CS-2023-8849</p>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <div class="flex flex-1 overflow-hidden">
        <!-- Sidebar Navigation -->
        <aside id="sidebar" class="fixed inset-y-0 left-0 z-20 w-64 bg-white dark:bg-gray-800 border-r border-gray-200 dark:border-gray-700 transform -translate-x-full lg:translate-x-0 transition-transform duration-200 ease-in-out flex flex-col pt-16 lg:pt-0">
            <div class="p-4 flex-1 overflow-y-auto custom-scrollbar space-y-1">
                <p class="px-3 text-[11px] font-semibold text-gray-400 uppercase tracking-wider mb-2">Main Navigation</p>
                <button onclick="switchTab('dashboard')" data-tab="dashboard" class="nav-btn active flex items-center space-x-3 w-full px-3 py-2.5 rounded-lg text-sm font-medium transition text-brand-600 bg-brand-50 dark:bg-brand-900/30 dark:text-brand-400">
                    <i data-lucide="layout-dashboard" class="w-5 h-5"></i>
                    <span>Dashboard</span>
                </button>
                <button onclick="switchTab('courses')" data-tab="courses" class="nav-btn flex items-center space-x-3 w-full px-3 py-2.5 rounded-lg text-sm font-medium transition text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700/50">
                    <i data-lucide="book-open" class="w-5 h-5"></i>
                    <span>My Courses</span>
                </button>
                <button onclick="switchTab('grades')" data-tab="grades" class="nav-btn flex items-center space-x-3 w-full px-3 py-2.5 rounded-lg text-sm font-medium transition text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700/50">
                    <i data-lucide="award" class="w-5 h-5"></i>
                    <span>Grades & Transcript</span>
                </button>
                <button onclick="switchTab('schedule')" data-tab="schedule" class="nav-btn flex items-center space-x-3 w-full px-3 py-2.5 rounded-lg text-sm font-medium transition text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700/50">
                    <i data-lucide="calendar" class="w-5 h-5"></i>
                    <span>Schedule & Timetable</span>
                </button>

                <p class="px-3 text-[11px] font-semibold text-gray-400 uppercase tracking-wider mt-6 mb-2">Finance & Services</p>
                <button onclick="switchTab('finance')" data-tab="finance" class="nav-btn flex items-center space-x-3 w-full px-3 py-2.5 rounded-lg text-sm font-medium transition text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700/50">
                    <i data-lucide="wallet" class="w-5 h-5"></i>
                    <span>Fees & Payments</span>
                </button>
                <button onclick="switchTab('profile')" data-tab="profile" class="nav-btn flex items-center space-x-3 w-full px-3 py-2.5 rounded-lg text-sm font-medium transition text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700/50">
                    <i data-lucide="user" class="w-5 h-5"></i>
                    <span>Student Profile</span>
                </button>
            </div>

            <!-- Quick Status Box -->
            <div class="p-4 border-t border-gray-200 dark:border-gray-700 bg-gray-50 dark:bg-gray-800/50 m-3 rounded-xl">
                <div class="flex items-center justify-between text-xs mb-1">
                    <span class="text-gray-500 dark:text-gray-400">Academic Standing</span>
                    <span class="font-semibold text-emerald-600 dark:text-emerald-400">Good</span>
                </div>
                <div class="w-full bg-gray-200 dark:bg-gray-700 rounded-full h-1.5 overflow-hidden">
                    <div class="bg-emerald-500 h-1.5 rounded-full" style="width: 88%"></div>
                </div>
                <p class="text-[11px] text-gray-400 mt-2">B.Sc Computer Science • Sem 4</p>
            </div>
        </aside>

        <!-- Overlay for Mobile Sidebar -->
        <div id="sidebar-overlay" class="fixed inset-0 bg-gray-900/50 z-10 hidden lg:hidden"></div>

        <!-- Main Content Area -->
        <main class="flex-1 overflow-y-auto custom-scrollbar p-4 sm:p-6 lg:p-8">

            <!-- TAB 1: DASHBOARD -->
            <div id="tab-dashboard" class="tab-content space-y-6">
                <!-- Welcome Banner -->
                <div class="relative overflow-hidden bg-gradient-to-r from-brand-600 to-indigo-700 rounded-2xl p-6 text-white shadow-lg">
                    <div class="relative z-10 max-w-2xl">
                        <span class="bg-white/20 text-xs font-semibold px-2.5 py-1 rounded-full uppercase tracking-wider backdrop-blur-md">Fall Semester 2026</span>
                        <h1 class="text-2xl sm:text-3xl font-bold mt-2">Welcome back, Alex! 👋</h1>
                        <p class="text-brand-100 mt-1 text-sm sm:text-base">You have 2 upcoming assignments due this week and 1 lecture scheduled for today.</p>
                        <div class="mt-4 flex flex-wrap gap-3">
                            <button onclick="switchTab('schedule')" class="bg-white text-brand-700 hover:bg-brand-50 px-4 py-2 rounded-lg font-semibold text-xs sm:text-sm shadow transition">View Timetable</button>
                            <button onclick="switchTab('courses')" class="bg-brand-500/40 hover:bg-brand-500/60 backdrop-blur-md border border-white/20 text-white px-4 py-2 rounded-lg font-semibold text-xs sm:text-sm transition">Browse Courses</button>
                        </div>
                    </div>
                    <!-- Decorative Graphic -->
                    <div class="absolute -right-8 -bottom-8 w-64 h-64 bg-white/10 rounded-full blur-2xl pointer-events-none"></div>
                </div>

                <!-- Stats Overview Cards -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <div class="bg-white dark:bg-gray-800 p-5 rounded-xl border border-gray-100 dark:border-gray-700 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs text-gray-500 dark:text-gray-400 font-medium">Current GPA</p>
                                <h3 class="text-2xl font-bold mt-1 text-gray-900 dark:text-white" id="stat-gpa">3.82</h3>
                            </div>
                            <div class="p-2.5 bg-emerald-50 dark:bg-emerald-900/30 text-emerald-600 rounded-lg">
                                <i data-lucide="trending-up" class="w-5 h-5"></i>
                            </div>
                        </div>
                        <p class="text-xs text-emerald-600 dark:text-emerald-400 mt-2 flex items-center">
                            <span>+0.12 from last semester</span>
                        </p>
                    </div>

                    <div class="bg-white dark:bg-gray-800 p-5 rounded-xl border border-gray-100 dark:border-gray-700 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs text-gray-500 dark:text-gray-400 font-medium">Earned Credits</p>
                                <h3 class="text-2xl font-bold mt-1 text-gray-900 dark:text-white">64 / 120</h3>
                            </div>
                            <div class="p-2.5 bg-blue-50 dark:bg-blue-900/30 text-blue-600 rounded-lg">
                                <i data-lucide="award" class="w-5 h-5"></i>
                            </div>
                        </div>
                        <p class="text-xs text-gray-500 dark:text-gray-400 mt-2">53% of degree completed</p>
                    </div>

                    <div class="bg-white dark:bg-gray-800 p-5 rounded-xl border border-gray-100 dark:border-gray-700 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs text-gray-500 dark:text-gray-400 font-medium">Overall Attendance</p>
                                <h3 class="text-2xl font-bold mt-1 text-gray-900 dark:text-white">92.4%</h3>
                            </div>
                            <div class="p-2.5 bg-purple-50 dark:bg-purple-900/30 text-purple-600 rounded-lg">
                                <i data-lucide="check-circle-2" class="w-5 h-5"></i>
                            </div>
                        </div>
                        <p class="text-xs text-emerald-600 dark:text-emerald-400 mt-2">Above 85% requirement</p>
                    </div>

                    <div class="bg-white dark:bg-gray-800 p-5 rounded-xl border border-gray-100 dark:border-gray-700 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs text-gray-500 dark:text-gray-400 font-medium">Tuition Due</p>
                                <h3 class="text-2xl font-bold mt-1 text-gray-900 dark:text-white">$450.00</h3>
                            </div>
                            <div class="p-2.5 bg-amber-50 dark:bg-amber-900/30 text-amber-600 rounded-lg">
                                <i data-lucide="dollar-sign" class="w-5 h-5"></i>
                            </div>
                        </div>
                        <p class="text-xs text-amber-600 dark:text-amber-400 mt-2">Due in 12 days</p>
                    </div>
                </div>

                <!-- Main Content Grid -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Today's Schedule & Assignments (Left 2 Columns) -->
                    <div class="lg:col-span-2 space-y-6">
                        <!-- Upcoming Tasks -->
                        <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-100 dark:border-gray-700 p-5 shadow-sm">
                            <div class="flex justify-between items-center mb-4">
                                <h3 class="font-bold text-gray-900 dark:text-white flex items-center gap-2">
                                    <i data-lucide="clock" class="w-5 h-5 text-brand-600"></i> Upcoming Tasks & Assignments
                                </h3>
                                <span class="text-xs font-medium text-brand-600 dark:text-brand-400 cursor-pointer hover:underline" onclick="switchTab('courses')">View all</span>
                            </div>
                            <div class="space-y-3" id="dash-assignments-list">
                                <!-- Populated dynamically -->
                            </div>
                        </div>

                        <!-- Performance Chart -->
                        <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-100 dark:border-gray-700 p-5 shadow-sm">
                            <h3 class="font-bold text-gray-900 dark:text-white mb-4">Academic Progression (GPA Trend)</h3>
                            <div class="h-64 relative">
                                <canvas id="gpaChart"></canvas>
                            </div>
                        </div>
                    </div>

                    <!-- Right Column: Announcements & Quick Actions -->
                    <div class="space-y-6">
                        <!-- Campus Announcements -->
                        <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-100 dark:border-gray-700 p-5 shadow-sm">
                            <h3 class="font-bold text-gray-900 dark:text-white mb-4 flex items-center gap-2">
                                <i data-lucide="megaphone" class="w-5 h-5 text-amber-500"></i> Notice Board
                            </h3>
                            <div class="space-y-4" id="dash-notices">
                                <div class="border-l-2 border-brand-500 pl-3">
                                    <span class="text-[10px] uppercase font-bold text-brand-600 bg-brand-50 dark:bg-brand-900/40 px-2 py-0.5 rounded">Academic</span>
                                    <h4 class="text-xs font-semibold text-gray-900 dark:text-white mt-1">Mid-Term Exam Schedule Released</h4>
                                    <p class="text-xs text-gray-500 dark:text-gray-400 mt-0.5">Please check your schedule tab for updated examination room assignments.</p>
                                    <span class="text-[10px] text-gray-400 block mt-1">2 hours ago</span>
                                </div>
                                <div class="border-l-2 border-emerald-500 pl-3">
                                    <span class="text-[10px] uppercase font-bold text-emerald-600 bg-emerald-50 dark:bg-emerald-900/40 px-2 py-0.5 rounded">Event</span>
                                    <h4 class="text-xs font-semibold text-gray-900 dark:text-white mt-1">Annual Tech Hackathon 2026</h4>
                                    <p class="text-xs text-gray-500 dark:text-gray-400 mt-0.5">Registration is now open for teams up to 4 members. $5,000 cash prizes!</p>
                                    <span class="text-[10px] text-gray-400 block mt-1">1 day ago</span>
                                </div>
                            </div>
                        </div>

                        <!-- Advisor Quick Contact -->
                        <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-100 dark:border-gray-700 p-5 shadow-sm">
                            <h3 class="font-bold text-gray-900 dark:text-white mb-3">Academic Advisor</h3>
                            <div class="flex items-center space-x-3">
                                <img src="https://images.unsplash.com/photo-1573496359142-b8d87734a5a2?auto=format&fit=crop&q=80&w=150" class="w-12 h-12 rounded-full object-cover">
                                <div>
                                    <h4 class="text-sm font-semibold text-gray-900 dark:text-white">Dr. Sarah Jenkins</h4>
                                    <p class="text-xs text-gray-500">Dept. of Computer Science</p>
                                    <p class="text-xs text-brand-600 dark:text-brand-400 mt-0.5">s.jenkins@university.edu</p>
                                </div>
                            </div>
                            <button onclick="alert('Appointment request sent to Dr. Jenkins!')" class="w-full mt-4 bg-gray-100 dark:bg-gray-700 hover:bg-gray-200 dark:hover:bg-gray-600 text-gray-800 dark:text-gray-200 py-2 rounded-lg text-xs font-medium transition">Book Advisory Session</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB 2: MY COURSES -->
            <div id="tab-courses" class="tab-content hidden space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="text-xl font-bold text-gray-900 dark:text-white">Enrolled Courses</h2>
                        <p class="text-xs text-gray-500">Fall Semester 2026 • 4 Active Courses</p>
                    </div>
                    <div class="flex items-center gap-2">
                        <input type="text" id="course-filter" onkeyup="filterCourses()" placeholder="Filter courses..." class="px-3 py-1.5 text-xs bg-white dark:bg-gray-800 border border-gray-300 dark:border-gray-600 rounded-lg focus:outline-none focus:border-brand-500">
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6" id="courses-container">
                    <!-- Populated dynamically -->
                </div>
            </div>

            <!-- TAB 3: GRADES & TRANSCRIPT -->
            <div id="tab-grades" class="tab-content hidden space-y-6">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                    <div>
                        <h2 class="text-xl font-bold text-gray-900 dark:text-white">Academic Grades & Records</h2>
                        <p class="text-xs text-gray-500">Track your GPA and detailed course breakdown</p>
                    </div>
                    <button onclick="window.print()" class="flex items-center space-x-2 bg-gray-100 dark:bg-gray-700 hover:bg-gray-200 dark:hover:bg-gray-600 text-gray-700 dark:text-gray-200 px-3 py-2 rounded-lg text-xs font-medium transition">
                        <i data-lucide="printer" class="w-4 h-4"></i>
                        <span>Print Unofficial Transcript</span>
                    </button>
                </div>

                <!-- Grades Table -->
                <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-200 dark:border-gray-700 overflow-hidden shadow-sm">
                    <div class="p-4 border-b border-gray-200 dark:border-gray-700 flex justify-between items-center">
                        <h3 class="font-bold text-gray-900 dark:text-white">Current Term Results</h3>
                        <span class="text-xs font-semibold px-2.5 py-1 bg-brand-50 dark:bg-brand-900/40 text-brand-600 dark:text-brand-400 rounded-full">Term GPA: 3.82</span>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-gray-50 dark:bg-gray-700/50 text-gray-500 dark:text-gray-400 text-xs uppercase tracking-wider">
                                    <th class="p-4">Course Code</th>
                                    <th class="p-4">Course Title</th>
                                    <th class="p-4">Credits</th>
                                    <th class="p-4">Midterm</th>
                                    <th class="p-4">Assignments</th>
                                    <th class="p-4">Grade</th>
                                    <th class="p-4">Status</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-gray-100 dark:divide-gray-700 text-xs text-gray-800 dark:text-gray-200" id="grades-table-body">
                                <!-- Populated dynamically -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 4: SCHEDULE & TIMETABLE -->
            <div id="tab-schedule" class="tab-content hidden space-y-6">
                <div class="flex justify-between items-center">
                    <div>
                        <h2 class="text-xl font-bold text-gray-900 dark:text-white">Weekly Timetable</h2>
                        <p class="text-xs text-gray-500">Regular lecture & lab schedule</p>
                    </div>
                </div>

                <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-200 dark:border-gray-700 p-4 overflow-x-auto shadow-sm">
                    <div class="min-w-[600px] grid grid-cols-6 gap-2 text-center text-xs">
                        <div class="font-bold text-gray-400 p-2">Time</div>
                        <div class="font-bold text-gray-700 dark:text-gray-300 bg-gray-50 dark:bg-gray-700/50 p-2 rounded">Mon</div>
                        <div class="font-bold text-gray-700 dark:text-gray-300 bg-gray-50 dark:bg-gray-700/50 p-2 rounded">Tue</div>
                        <div class="font-bold text-gray-700 dark:text-gray-300 bg-gray-50 dark:bg-gray-700/50 p-2 rounded">Wed</div>
                        <div class="font-bold text-gray-700 dark:text-gray-300 bg-gray-50 dark:bg-gray-700/50 p-2 rounded">Thu</div>
                        <div class="font-bold text-gray-700 dark:text-gray-300 bg-gray-50 dark:bg-gray-700/50 p-2 rounded">Fri</div>

                        <!-- 09:00 AM Slot -->
                        <div class="font-semibold text-gray-400 py-4 flex items-center justify-center">09:00 AM</div>
                        <div class="bg-blue-50 dark:bg-blue-900/30 text-blue-800 dark:text-blue-300 p-3 rounded-lg text-left border border-blue-200 dark:border-blue-800">
                            <span class="font-bold block">CS301</span>
                            <span class="text-[10px] block">Room 304</span>
                        </div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>
                        <div class="bg-blue-50 dark:bg-blue-900/30 text-blue-800 dark:text-blue-300 p-3 rounded-lg text-left border border-blue-200 dark:border-blue-800">
                            <span class="font-bold block">CS301</span>
                            <span class="text-[10px] block">Room 304</span>
                        </div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>
                        <div class="bg-emerald-50 dark:bg-emerald-900/30 text-emerald-800 dark:text-emerald-300 p-3 rounded-lg text-left border border-emerald-200 dark:border-emerald-800">
                            <span class="font-bold block">CS305 Lab</span>
                            <span class="text-[10px] block">Lab 02</span>
                        </div>

                        <!-- 11:00 AM Slot -->
                        <div class="font-semibold text-gray-400 py-4 flex items-center justify-center">11:00 AM</div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>
                        <div class="bg-purple-50 dark:bg-purple-900/30 text-purple-800 dark:text-purple-300 p-3 rounded-lg text-left border border-purple-200 dark:border-purple-800">
                            <span class="font-bold block">CS308</span>
                            <span class="text-[10px] block">Auditorium B</span>
                        </div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>
                        <div class="bg-purple-50 dark:bg-purple-900/30 text-purple-800 dark:text-purple-300 p-3 rounded-lg text-left border border-purple-200 dark:border-purple-800">
                            <span class="font-bold block">CS308</span>
                            <span class="text-[10px] block">Auditorium B</span>
                        </div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>

                        <!-- 02:00 PM Slot -->
                        <div class="font-semibold text-gray-400 py-4 flex items-center justify-center">02:00 PM</div>
                        <div class="bg-amber-50 dark:bg-amber-900/30 text-amber-800 dark:text-amber-300 p-3 rounded-lg text-left border border-amber-200 dark:border-amber-800">
                            <span class="font-bold block">MATH202</span>
                            <span class="text-[10px] block">Hall 101</span>
                        </div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>
                        <div class="bg-amber-50 dark:bg-amber-900/30 text-amber-800 dark:text-amber-300 p-3 rounded-lg text-left border border-amber-200 dark:border-amber-800">
                            <span class="font-bold block">MATH202</span>
                            <span class="text-[10px] block">Hall 101</span>
                        </div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>
                        <div class="bg-gray-50 dark:bg-gray-800/30 p-3 rounded-lg"></div>
                    </div>
                </div>
            </div>

            <!-- TAB 5: FEES & FINANCE -->
            <div id="tab-finance" class="tab-content hidden space-y-6">
                <div>
                    <h2 class="text-xl font-bold text-gray-900 dark:text-white">Financial Statement & Fee Payment</h2>
                    <p class="text-xs text-gray-500">Manage account balance, view fee breakdown, and settle dues online</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Balance Card -->
                    <div class="bg-white dark:bg-gray-800 p-6 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm flex flex-col justify-between">
                        <div>
                            <span class="text-xs text-gray-500 dark:text-gray-400">Total Outstanding Balance</span>
                            <h3 class="text-3xl font-extrabold text-gray-900 dark:text-white mt-1" id="finance-balance">$450.00</h3>
                            <p class="text-xs text-amber-600 dark:text-amber-400 mt-1">Due Date: Nov 15, 2026</p>
                        </div>
                        <div class="mt-6 space-y-2">
                            <button onclick="openPaymentModal()" class="w-full bg-brand-600 hover:bg-brand-700 text-white font-medium py-2.5 rounded-lg text-xs shadow transition flex items-center justify-center gap-2">
                                <i data-lucide="credit-card" class="w-4 h-4"></i>
                                <span>Make Online Payment</span>
                            </button>
                        </div>
                    </div>

                    <!-- Fee Breakdown -->
                    <div class="lg:col-span-2 bg-white dark:bg-gray-800 p-6 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm">
                        <h3 class="font-bold text-gray-900 dark:text-white mb-4 text-sm">Semester Fee Summary</h3>
                        <div class="space-y-3 text-xs">
                            <div class="flex justify-between py-2 border-b border-gray-100 dark:border-gray-700">
                                <span class="text-gray-600 dark:text-gray-400">Tuition Fee (Fall 2026)</span>
                                <span class="font-semibold text-gray-900 dark:text-white">$3,200.00</span>
                            </div>
                            <div class="flex justify-between py-2 border-b border-gray-100 dark:border-gray-700">
                                <span class="text-gray-600 dark:text-gray-400">Laboratory & Computer Access</span>
                                <span class="font-semibold text-gray-900 dark:text-white">$250.00</span>
                            </div>
                            <div class="flex justify-between py-2 border-b border-gray-100 dark:border-gray-700">
                                <span class="text-gray-600 dark:text-gray-400">Campus Student Activity Fee</span>
                                <span class="font-semibold text-gray-900 dark:text-white">$100.00</span>
                            </div>
                            <div class="flex justify-between py-2 border-b border-gray-100 dark:border-gray-700 text-emerald-600 dark:text-emerald-400">
                                <span>Merit Scholarship Applied</span>
                                <span class="font-semibold">-$3,100.00</span>
                            </div>
                            <div class="flex justify-between py-2 font-bold text-sm text-gray-900 dark:text-white pt-2">
                                <span>Remaining Net Payable</span>
                                <span>$450.00</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Transaction History -->
                <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-200 dark:border-gray-700 overflow-hidden shadow-sm">
                    <div class="p-4 border-b border-gray-200 dark:border-gray-700">
                        <h3 class="font-bold text-gray-900 dark:text-white text-sm">Payment History</h3>
                    </div>
                    <table class="w-full text-left text-xs">
                        <thead class="bg-gray-50 dark:bg-gray-700/50 text-gray-500">
                            <tr>
                                <th class="p-4">Transaction ID</th>
                                <th class="p-4">Date</th>
                                <th class="p-4">Description</th>
                                <th class="p-4">Amount</th>
                                <th class="p-4">Status</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-gray-100 dark:divide-gray-700" id="payment-history-body">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- TAB 6: STUDENT PROFILE -->
            <div id="tab-profile" class="tab-content hidden space-y-6">
                <div>
                    <h2 class="text-xl font-bold text-gray-900 dark:text-white">Student Profile</h2>
                    <p class="text-xs text-gray-500">View and update your personal information</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="bg-white dark:bg-gray-800 p-6 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm text-center">
                        <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&q=80&w=150" class="w-24 h-24 rounded-full mx-auto object-cover ring-4 ring-brand-500/20">
                        <h3 class="font-bold text-lg text-gray-900 dark:text-white mt-4">Alex Morgan</h3>
                        <p class="text-xs text-gray-500">CS-2023-8849</p>
                        <span class="inline-block mt-2 text-xs bg-emerald-100 text-emerald-700 dark:bg-emerald-900/40 dark:text-emerald-300 px-3 py-1 rounded-full font-semibold">Active Student</span>
                    </div>

                    <div class="lg:col-span-2 bg-white dark:bg-gray-800 p-6 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm">
                        <form id="profile-form" onsubmit="saveProfile(event)" class="space-y-4 text-xs">
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                                <div>
                                    <label class="block font-semibold text-gray-700 dark:text-gray-300 mb-1">Full Name</label>
                                    <input type="text" id="prof-name" value="Alex Morgan" class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white">
                                </div>
                                <div>
                                    <label class="block font-semibold text-gray-700 dark:text-gray-300 mb-1">Email Address</label>
                                    <input type="email" id="prof-email" value="alex.morgan@university.edu" class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white">
                                </div>
                                <div>
                                    <label class="block font-semibold text-gray-700 dark:text-gray-300 mb-1">Phone Number</label>
                                    <input type="text" id="prof-phone" value="+1 (555) 234-5678" class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white">
                                </div>
                                <div>
                                    <label class="block font-semibold text-gray-700 dark:text-gray-300 mb-1">Major / Program</label>
                                    <input type="text" value="B.Sc. Computer Science" disabled class="w-full p-2.5 rounded-lg border border-gray-200 dark:border-gray-700 bg-gray-100 dark:bg-gray-800 text-gray-500 cursor-not-allowed">
                                </div>
                            </div>
                            <div class="pt-4 flex justify-end">
                                <button type="submit" class="bg-brand-600 hover:bg-brand-700 text-white px-5 py-2 rounded-lg font-semibold shadow transition">Save Profile</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- MODAL: Payment Modal -->
    <div id="payment-modal" class="fixed inset-0 bg-gray-900/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white dark:bg-gray-800 rounded-2xl max-w-md w-full p-6 shadow-2xl border border-gray-200 dark:border-gray-700">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-lg font-bold text-gray-900 dark:text-white">Online Fee Payment</h3>
                <button onclick="closePaymentModal()" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-200">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <form onsubmit="processPayment(event)" class="space-y-4 text-xs">
                <div>
                    <label class="block font-medium text-gray-700 dark:text-gray-300 mb-1">Payment Amount ($)</label>
                    <input type="number" id="pay-amount" value="450" max="450" min="1" required class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white font-semibold">
                </div>
                <div>
                    <label class="block font-medium text-gray-700 dark:text-gray-300 mb-1">Cardholder Name</label>
                    <input type="text" placeholder="Alex Morgan" required class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white">
                </div>
                <div>
                    <label class="block font-medium text-gray-700 dark:text-gray-300 mb-1">Card Number</label>
                    <input type="text" placeholder="•••• •••• •••• 4242" required class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white">
                </div>
                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block font-medium text-gray-700 dark:text-gray-300 mb-1">Expiry Date</label>
                        <input type="text" placeholder="MM/YY" required class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white">
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 dark:text-gray-300 mb-1">CVC / CVV</label>
                        <input type="text" placeholder="123" required class="w-full p-2.5 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white">
                    </div>
                </div>
                <button type="submit" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white py-3 rounded-lg font-bold text-sm shadow transition mt-2">Confirm Payment</button>
            </form>
        </div>
    </div>

    <!-- Application Script -->
    <script>
        // Data Store
        const appState = {
            user: { name: "Alex Morgan", id: "CS-2023-8849", email: "alex.morgan@university.edu" },
            balance: 450.00,
            notifications: [
                { id: 1, title: "Grade Posted", desc: "CS301 Midterm grade is now available.", read: false, time: "10m ago" },
                { id: 2, title: "Tuition Reminder", desc: "Balance of $450 is due on Nov 15.", read: false, time: "1h ago" }
            ],
            assignments: [
                { title: "Database Normalization Assignment", course: "CS301 - Database Systems", due: "Tomorrow, 11:59 PM", status: "Pending" },
                { title: "Algorithm Analysis Problem Set 3", course: "CS305 - Algorithms", due: "Nov 3, 2026", status: "Pending" }
            ],
            courses: [
                { code: "CS301", title: "Database Systems", instructor: "Prof. Alan Turing", progress: 75, grade: "A-", midterm: "92%" },
                { code: "CS305", title: "Data Structures & Algorithms", instructor: "Dr. Grace Hopper", progress: 60, grade: "A", midterm: "95%" },
                { code: "CS308", title: "Web Application Architecture", instructor: "Prof. Tim Berners-Lee", progress: 85, grade: "B+", midterm: "88%" },
                { code: "MATH202", title: "Discrete Mathematics", instructor: "Dr. John von Neumann", progress: 40, grade: "A", midterm: "94%" }
            ],
            transactions: [
                { id: "TXN-9082", date: "Sep 01, 2026", desc: "Fall Semester Tuition", amount: "$3,200.00", status: "Paid" },
                { id: "TXN-8721", date: "Sep 05, 2026", desc: "Merit Scholarship Credit", amount: "-$3,100.00", status: "Applied" }
            ]
        };

        // Initialize UI Elements
        document.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            initChart();
            renderAssignments();
            renderCourses();
            renderGrades();
            renderNotifications();
            renderTransactions();

            // Mobile Menu Toggle
            const mobileBtn = document.getElementById('mobile-menu-btn');
            const sidebar = document.getElementById('sidebar');
            const overlay = document.getElementById('sidebar-overlay');

            mobileBtn.addEventListener('click', () => {
                sidebar.classList.toggle('-translate-x-full');
                overlay.classList.toggle('hidden');
            });

            overlay.addEventListener('click', () => {
                sidebar.classList.add('-translate-x-full');
                overlay.classList.add('hidden');
            });

            // Dark Mode Switcher
            document.getElementById('theme-toggle').addEventListener('click', () => {
                document.documentElement.classList.toggle('dark');
            });

            // Notifications Dropdown Toggle
            document.getElementById('notif-btn').addEventListener('click', (e) => {
                e.stopPropagation();
                document.getElementById('notif-dropdown').classList.toggle('hidden');
            });

            document.addEventListener('click', () => {
                document.getElementById('notif-dropdown').classList.add('hidden');
            });
        });

        // Tab Navigation Switcher
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');

            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-brand-600', 'bg-brand-50', 'dark:bg-brand-900/30', 'dark:text-brand-400');
                btn.classList.add('text-gray-600', 'dark:text-gray-400');
            });

            const activeBtn = document.querySelector(`[data-tab="${tabId}"]`);
            if (activeBtn) {
                activeBtn.classList.add('text-brand-600', 'bg-brand-50', 'dark:bg-brand-900/30', 'dark:text-brand-400');
            }

            // Close mobile sidebar if open
            document.getElementById('sidebar').classList.add('-translate-x-full');
            document.getElementById('sidebar-overlay').classList.add('hidden');
        }

        // Render Dashboard Assignments
        function renderAssignments() {
            const list = document.getElementById('dash-assignments-list');
            list.innerHTML = appState.assignments.map(a => `
                <div class="flex items-center justify-between p-3 bg-gray-50 dark:bg-gray-700/40 rounded-lg border border-gray-100 dark:border-gray-700">
                    <div class="flex items-start space-x-3">
                        <div class="p-2 bg-brand-100 text-brand-600 dark:bg-brand-900/50 dark:text-brand-300 rounded-lg">
                            <i data-lucide="file-text" class="w-4 h-4"></i>
                        </div>
                        <div>
                            <h4 class="text-xs font-semibold text-gray-900 dark:text-white">${a.title}</h4>
                            <p class="text-[11px] text-gray-500">${a.course}</p>
                        </div>
                    </div>
                    <div class="text-right">
                        <span class="text-[11px] font-medium text-amber-600 dark:text-amber-400 block">${a.due}</span>
                        <span class="text-[10px] bg-amber-100 text-amber-800 dark:bg-amber-900/40 dark:text-amber-300 px-2 py-0.5 rounded-full font-semibold">Pending</span>
                    </div>
                </div>
            `).join('');
            lucide.createIcons();
        }

        // Render Enrolled Courses
        function renderCourses() {
            const container = document.getElementById('courses-container');
            container.innerHTML = appState.courses.map(c => `
                <div class="course-card bg-white dark:bg-gray-800 rounded-xl border border-gray-200 dark:border-gray-700 p-5 shadow-sm space-y-4">
                    <div class="flex justify-between items-start">
                        <div>
                            <span class="text-xs font-bold text-brand-600 dark:text-brand-400 bg-brand-50 dark:bg-brand-900/40 px-2 py-1 rounded">${c.code}</span>
                            <h3 class="font-bold text-gray-900 dark:text-white mt-2">${c.title}</h3>
                            <p class="text-xs text-gray-500">${c.instructor}</p>
                        </div>
                    </div>
                    <div>
                        <div class="flex justify-between text-xs mb-1">
                            <span class="text-gray-500">Syllabus Completion</span>
                            <span class="font-semibold text-gray-800 dark:text-gray-200">${c.progress}%</span>
                        </div>
                        <div class="w-full bg-gray-100 dark:bg-gray-700 rounded-full h-2 overflow-hidden">
                            <div class="bg-brand-600 h-2 rounded-full" style="width: ${c.progress}%"></div>
                        </div>
                    </div>
                    <div class="pt-3 border-t border-gray-100 dark:border-gray-700 flex justify-between text-xs">
                        <span class="text-gray-500">Current Grade: <strong class="text-gray-900 dark:text-white">${c.grade}</strong></span>
                        <button onclick="alert('Viewing syllabus for ${c.code}')" class="text-brand-600 dark:text-brand-400 font-semibold hover:underline">View Syllabus →</button>
                    </div>
                </div>
            `).join('');
        }

        // Render Grades Table
        function renderGrades() {
            const tbody = document.getElementById('grades-table-body');
            tbody.innerHTML = appState.courses.map(c => `
                <tr class="hover:bg-gray-50 dark:hover:bg-gray-700/30">
                    <td class="p-4 font-bold text-brand-600 dark:text-brand-400">${c.code}</td>
                    <td class="p-4 font-medium text-gray-900 dark:text-white">${c.title}</td>
                    <td class="p-4">3</td>
                    <td class="p-4">${c.midterm}</td>
                    <td class="p-4">90%</td>
                    <td class="p-4 font-bold">${c.grade}</td>
                    <td class="p-4"><span class="px-2 py-0.5 bg-emerald-100 text-emerald-800 dark:bg-emerald-900/40 dark:text-emerald-300 text-[10px] font-bold rounded-full">Passing</span></td>
                </tr>
            `).join('');
        }

        // Filter Courses Search
        function filterCourses() {
            const query = document.getElementById('course-filter').value.toLowerCase();
            document.querySelectorAll('.course-card').forEach(card => {
                const text = card.textContent.toLowerCase();
                card.style.display = text.includes(query) ? 'block' : 'none';
            });
        }

        // Notifications System
        function renderNotifications() {
            const list = document.getElementById('notif-list');
            list.innerHTML = appState.notifications.map(n => `
                <div class="p-3 hover:bg-gray-50 dark:hover:bg-gray-700/50 transition">
                    <p class="text-xs font-semibold text-gray-900 dark:text-white">${n.title}</p>
                    <p class="text-[11px] text-gray-500 mt-0.5">${n.desc}</p>
                    <span class="text-[9px] text-gray-400 block mt-1">${n.time}</span>
                </div>
            `).join('');
        }

        function markAllNotificationsRead() {
            document.getElementById('notif-badge').classList.add('hidden');
            alert('All notifications marked as read.');
        }

        // Transactions History
        function renderTransactions() {
            const tbody = document.getElementById('payment-history-body');
            tbody.innerHTML = appState.transactions.map(t => `
                <tr class="hover:bg-gray-50 dark:hover:bg-gray-700/30">
                    <td class="p-4 font-mono font-medium">${t.id}</td>
                    <td class="p-4 text-gray-500">${t.date}</td>
                    <td class="p-4 font-medium">${t.desc}</td>
                    <td class="p-4 font-bold ${t.amount.startsWith('-') ? 'text-emerald-600' : 'text-gray-900 dark:text-white'}">${t.amount}</td>
                    <td class="p-4"><span class="px-2 py-0.5 bg-gray-100 text-gray-700 dark:bg-gray-700 dark:text-gray-300 text-[10px] font-semibold rounded-full">${t.status}</span></td>
                </tr>
            `).join('');
        }

        // Modal Controls
        function openPaymentModal() {
            document.getElementById('payment-modal').classList.remove('hidden');
        }

        function closePaymentModal() {
            document.getElementById('payment-modal').classList.add('hidden');
        }

        function processPayment(e) {
            e.preventDefault();
            const amount = parseFloat(document.getElementById('pay-amount').value);
            appState.balance -= amount;
            document.getElementById('finance-balance').innerText = `$${appState.balance.toFixed(2)}`;

            appState.transactions.unshift({
                id: `TXN-${Math.floor(1000 + Math.random() * 9000)}`,
                date: "Today",
                desc: "Online Fee Payment",
                amount: `-$${amount.toFixed(2)}`,
                status: "Completed"
            });

            renderTransactions();
            closePaymentModal();
            alert("Payment successful! Transaction recorded.");
        }

        // Save Profile Form
        function saveProfile(e) {
            e.preventDefault();
            const newName = document.getElementById('prof-name').value;
            document.getElementById('user-name').innerText = newName;
            alert('Profile saved successfully!');
        }

        // Chart.js Graph Initialization
        function initChart() {
            const ctx = document.getElementById('gpaChart').getContext('2d');
            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: ['Sem 1', 'Sem 2', 'Sem 3', 'Sem 4 (Current)'],
                    datasets: [{
                        label: 'Cumulative GPA',
                        data: [3.55, 3.68, 3.70, 3.82],
                        borderColor: '#2563eb',
                        backgroundColor: 'rgba(37, 99, 235, 0.1)',
                        fill: true,
                        tension: 0.4,
                        borderWidth: 3,
                        pointRadius: 5
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        y: { min: 3.0, max: 4.0, grid: { color: 'rgba(160, 174, 192, 0.2)' } },
                        x: { grid: { display: false } }
                    }
                }
            });
        }
    </script>
</body>
</html>
