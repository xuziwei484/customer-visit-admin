<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>客户拜访管理系统 - 管理员界面</title>
  <!-- 引入Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- 引入Font Awesome -->
  <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
  <!-- 引入Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.8/dist/chart.umd.min.js"></script>
  
  <!-- 配置Tailwind自定义颜色和字体 -->
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: '#165DFF',
            secondary: '#36CFC9',
            success: '#52C41A',
            warning: '#FAAD14',
            danger: '#FF4D4F',
            neutral: '#F5F7FA',
            dark: '#1D2129',
          },
          fontFamily: {
            inter: ['Inter', 'system-ui', 'sans-serif'],
          },
        },
      }
    }
  </script>
  
  <style type="text/tailwindcss">
    @layer utilities {
      .content-auto {
        content-visibility: auto;
      }
      .card-shadow {
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
      }
      .input-focus {
        @apply focus:ring-2 focus:ring-primary/30 focus:border-primary focus:outline-none;
      }
      .btn-hover {
        @apply hover:shadow-lg hover:-translate-y-0.5 transition-all duration-300;
      }
      .sidebar-active {
        @apply bg-primary/10 text-primary border-l-4 border-primary;
      }
      .status-倡领 {
        @apply bg-success/10 text-success;
      }
      .status-支持 {
        @apply bg-primary/10 text-primary;
      }
      .status-中立 {
        @apply bg-warning/10 text-warning;
      }
      .status-反对 {
        @apply bg-danger/10 text-danger;
      }
    }
  </style>
</head>

<body class="font-inter bg-gray-50 text-dark min-h-screen flex overflow-hidden">
  <!-- 侧边栏导航 -->
  <aside class="bg-white w-64 shadow-sm h-screen flex-shrink-0 border-r overflow-y-auto transition-all duration-300 ease-in-out" id="sidebar">
    <div class="p-4 border-b flex items-center">
      <i class="fa fa-tachometer text-primary text-2xl mr-3"></i>
      <h1 class="text-xl font-bold text-primary">管理系统</h1>
    </div>
    
    <nav class="py-4">
      <ul>
        <li>
          <a href="#dashboard" class="sidebar-active flex items-center px-4 py-3 text-sm transition-colors">
            <i class="fa fa-dashboard w-6 text-center mr-3"></i>
            <span>仪表盘</span>
          </a>
        </li>
        <li>
          <a href="#customer-management" class="flex items-center px-4 py-3 text-sm text-gray-600 hover:bg-gray-50 transition-colors">
            <i class="fa fa-users w-6 text-center mr-3"></i>
            <span>客户管理</span>
          </a>
        </li>
        <li>
          <a href="#visit-records" class="flex items-center px-4 py-3 text-sm text-gray-600 hover:bg-gray-50 transition-colors">
            <i class="fa fa-calendar-check-o w-6 text-center mr-3"></i>
            <span>客户拜访记录</span>
          </a>
        </li>
        <li>
          <a href="#settings" class="flex items-center px-4 py-3 text-sm text-gray-600 hover:bg-gray-50 transition-colors">
            <i class="fa fa-cog w-6 text-center mr-3"></i>
            <span>系统设置</span>
          </a>
        </li>
      </ul>
    </nav>
  </aside>

  <!-- 主内容区 -->
  <div class="flex-1 flex flex-col overflow-hidden">
    <!-- 顶部导航 -->
    <header class="bg-white shadow-sm h-16 flex items-center justify-between px-6 z-10">
      <button id="toggleSidebar" class="text-gray-500 hover:text-primary">
        <i class="fa fa-bars text-xl"></i>
      </button>
      
      <div class="flex items-center space-x-4">
        <button class="text-gray-500 hover:text-primary relative">
          <i class="fa fa-bell text-xl"></i>
          <span class="absolute -top-1 -right-1 bg-danger text-white text-xs rounded-full w-4 h-4 flex items-center justify-center">3</span>
        </button>
        
        <div class="flex items-center space-x-2">
          <img src="https://picsum.photos/id/1005/40/40" alt="管理员头像" class="w-8 h-8 rounded-full object-cover border border-gray-200">
          <span class="text-sm font-medium">管理员</span>
        </div>
      </div>
    </header>

    <!-- 页面内容 -->
    <main class="flex-1 overflow-y-auto p-6" id="main-content">
      <!-- 仪表盘页面 -->
      <div id="dashboard-page" class="page-content">
        <div class="flex flex-col md:flex-row md:items-center md:justify-between mb-6">
          <h2 class="text-2xl font-bold mb-4 md:mb-0">仪表盘</h2>
          <div class="flex items-center space-x-3">
            <div class="relative">
              <select class="pl-3 pr-10 py-2 bg-white border border-gray-300 rounded-lg input-focus appearance-none text-sm">
                <option>全部时间</option>
                <option>本月</option>
                <option>本季度</option>
                <option>本年</option>
              </select>
              <i class="fa fa-chevron-down absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 pointer-events-none text-xs"></i>
            </div>
            <button class="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/90 btn-hover text-sm">
              <i class="fa fa-download mr-1"></i> 导出数据
            </button>
          </div>
        </div>
        
        <!-- 关键指标卡片 -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
          <div class="bg-white rounded-xl p-5 card-shadow">
            <div class="flex items-start justify-between">
              <div>
                <p class="text-gray-500 text-sm mb-1">总拜访进度</p>
                <h3 class="text-2xl font-bold mb-2">78%</h3>
                <p class="text-success text-xs flex items-center">
                  <i class="fa fa-arrow-up mr-1"></i> 较上月增长 5.2%
                </p>
              </div>
              <div class="w-10 h-10 rounded-lg bg-primary/10 flex items-center justify-center text-primary">
                <i class="fa fa-calendar"></i>
              </div>
            </div>
            <div class="mt-4 w-full bg-gray-100 rounded-full h-2">
              <div class="bg-primary h-2 rounded-full" style="width: 78%"></div>
            </div>
          </div>
          
          <div class="bg-white rounded-xl p-5 card-shadow">
            <div class="flex items-start justify-between">
              <div>
                <p class="text-gray-500 text-sm mb-1">总认可度</p>
                <h3 class="text-2xl font-bold mb-2">82% <span class="text-sm font-normal text-gray-500">246/300</span></h3>
                <p class="text-success text-xs flex items-center">
                  <i class="fa fa-arrow-up mr-1"></i> 较上月增长 3.8%
                </p>
              </div>
              <div class="w-10 h-10 rounded-lg bg-success/10 flex items-center justify-center text-success">
                <i class="fa fa-thumbs-up"></i>
              </div>
            </div>
            <div class="mt-4 w-full bg-gray-100 rounded-full h-2">
              <div class="bg-success h-2 rounded-full" style="width: 82%"></div>
            </div>
          </div>
          
          <div class="bg-white rounded-xl p-5 card-shadow">
            <div class="flex items-start justify-between">
              <div>
                <p class="text-gray-500 text-sm mb-1">总支持数量</p>
                <h3 class="text-2xl font-bold mb-2">75% <span class="text-sm font-normal text-gray-500">225/300</span></h3>
                <p class="text-danger text-xs flex items-center">
                  <i class="fa fa-arrow-down mr-1"></i> 较上月下降 1.2%
                </p>
              </div>
              <div class="w-10 h-10 rounded-lg bg-primary/10 flex items-center justify-center text-primary">
                <i class="fa fa-check-circle"></i>
              </div>
            </div>
            <div class="mt-4 w-full bg-gray-100 rounded-full h-2">
              <div class="bg-primary h-2 rounded-full" style="width: 75%"></div>
            </div>
          </div>
          
          <div class="bg-white rounded-xl p-5 card-shadow">
            <div class="flex items-start justify-between">
              <div>
                <p class="text-gray-500 text-sm mb-1">总倡领数量</p>
                <h3 class="text-2xl font-bold mb-2">42% <span class="text-sm font-normal text-gray-500">126/300</span></h3>
                <p class="text-success text-xs flex items-center">
                  <i class="fa fa-arrow-up mr-1"></i> 较上月增长 8.5%
                </p>
              </div>
              <div class="w-10 h-10 rounded-lg bg-secondary/10 flex items-center justify-center text-secondary">
                <i class="fa fa-star"></i>
              </div>
            </div>
            <div class="mt-4 w-full bg-gray-100 rounded-full h-2">
              <div class="bg-secondary h-2 rounded-full" style="width: 42%"></div>
            </div>
          </div>
        </div>
        
        <!-- 图表区域 -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
          <div class="bg-white rounded-xl p-5 card-shadow">
            <h3 class="text-lg font-semibold mb-4">RCL拜访完成情况</h3>
            <div class="h-80">
              <canvas id="rclChart"></canvas>
            </div>
          </div>
          
          <div class="bg-white rounded-xl p-5 card-shadow">
            <h3 class="text-lg font-semibold mb-4">客户态度分布</h3>
            <div class="h-80">
              <canvas id="attitudeChart"></canvas>
            </div>
          </div>
        </div>
        
        <!-- 详细数据表格 -->
        <div class="bg-white rounded-xl p-5 card-shadow mb-6">
          <div class="flex items-center justify-between mb-4">
            <h3 class="text-lg font-semibold">梯度客户拜访情况</h3>
            <div class="relative">
              <select class="pl-3 pr-10 py-1.5 bg-white border border-gray-300 rounded-lg input-focus appearance-none text-sm">
                <option>全部梯度</option>
                <option>VVIP</option>
                <option>T1</option>
                <option>T2</option>
                <option>T3</option>
              </select>
              <i class="fa fa-chevron-down absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 pointer-events-none text-xs"></i>
            </div>
          </div>
          
          <div class="overflow-x-auto">
            <table class="w-full text-sm">
              <thead>
                <tr class="border-b border-gray-200">
                  <th class="text-left py-3 px-4 font-semibold">客户梯度</th>
                  <th class="text-left py-3 px-4 font-semibold">总客户数</th>
                  <th class="text-left py-3 px-4 font-semibold">已拜访</th>
                  <th class="text-left py-3 px-4 font-semibold">拜访进度</th>
                  <th class="text-left py-3 px-4 font-semibold">认可3+6优势</th>
                  <th class="text-left py-3 px-4 font-semibold">倡领数量</th>
                </tr>
              </thead>
              <tbody>
                <tr class="border-b border-gray-100 hover:bg-gray-50">
                  <td class="py-3 px-4">VVIP</td>
                  <td class="py-3 px-4">50</td>
                  <td class="py-3 px-4">45</td>
                  <td class="py-3 px-4">
                    <div class="flex items-center">
                      <div class="w-24 bg-gray-100 rounded-full h-2 mr-2">
                        <div class="bg-success h-2 rounded-full" style="width: 90%"></div>
                      </div>
                      <span>90%</span>
                    </div>
                  </td>
                  <td class="py-3 px-4">42 (93%)</td>
                  <td class="py-3 px-4">30 (67%)</td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50">
                  <td class="py-3 px-4">T1</td>
                  <td class="py-3 px-4">100</td>
                  <td class="py-3 px-4">85</td>
                  <td class="py-3 px-4">
                    <div class="flex items-center">
                      <div class="w-24 bg-gray-100 rounded-full h-2 mr-2">
                        <div class="bg-success h-2 rounded-full" style="width: 85%"></div>
                      </div>
                      <span>85%</span>
                    </div>
                  </td>
                  <td class="py-3 px-4">72 (85%)</td>
                  <td class="py-3 px-4">45 (53%)</td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50">
                  <td class="py-3 px-4">T2</td>
                  <td class="py-3 px-4">150</td>
                  <td class="py-3 px-4">110</td>
                  <td class="py-3 px-4">
                    <div class="flex items-center">
                      <div class="w-24 bg-gray-100 rounded-full h-2 mr-2">
                        <div class="bg-warning h-2 rounded-full" style="width: 73%"></div>
                      </div>
                      <span>73%</span>
                    </div>
                  </td>
                  <td class="py-3 px-4">95 (86%)</td>
                  <td class="py-3 px-4">51 (46%)</td>
                </tr>
                <tr class="hover:bg-gray-50">
                  <td class="py-3 px-4">T3</td>
                  <td class="py-3 px-4">200</td>
                  <td class="py-3 px-4">120</td>
                  <td class="py-3 px-4">
                    <div class="flex items-center">
                      <div class="w-24 bg-gray-100 rounded-full h-2 mr-2">
                        <div class="bg-warning h-2 rounded-full" style="width: 60%"></div>
                      </div>
                      <span>60%</span>
                    </div>
                  </td>
                  <td class="py-3 px-4">78 (65%)</td>
                  <td class="py-3 px-4">30 (25%)</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      
      <!-- 客户管理页面 -->
      <div id="customer-management-page" class="page-content hidden">
        <div class="flex flex-col md:flex-row md:items-center md:justify-between mb-6">
          <h2 class="text-2xl font-bold mb-4 md:mb-0">客户管理</h2>
          <div class="flex flex-wrap gap-3">
            <div class="relative">
              <input type="text" placeholder="搜索客户..." class="pl-10 pr-4 py-2 border border-gray-300 rounded-lg input-focus w-full md:w-64 text-sm">
              <i class="fa fa-search absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
            </div>
            <button class="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/90 btn-hover text-sm flex items-center">
              <i class="fa fa-plus mr-1"></i> 添加客户
            </button>
            <button class="px-4 py-2 bg-secondary text-white rounded-lg hover:bg-secondary/90 btn-hover text-sm flex items-center">
              <i class="fa fa-upload mr-1"></i> 导入数据
            </button>
          </div>
        </div>
        
        <!-- 层级筛选器 -->
        <div class="bg-white rounded-xl p-4 card-shadow mb-6">
          <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
            <div>
              <label for="filter-rcl" class="block text-sm font-medium text-gray-700 mb-1">RCL</label>
              <select id="filter-rcl" class="w-full px-3 py-2 border border-gray-300 rounded-lg input-focus text-sm">
                <option value="">全部RCL</option>
                <option value="rcl1">北区RCL</option>
                <option value="rcl2">南区RCL</option>
              </select>
            </div>
            <div>
              <label for="filter-rel" class="block text-sm font-medium text-gray-700 mb-1">REL</label>
              <select id="filter-rel" class="w-full px-3 py-2 border border-gray-300 rounded-lg input-focus text-sm">
                <option value="">全部REL</option>
                <option value="rel1-1">北京REL</option>
                <option value="rel1-2">天津REL</option>
                <option value="rel2-1">上海REL</option>
              </select>
            </div>
            <div>
              <label for="filter-lel" class="block text-sm font-medium text-gray-700 mb-1">LEL</label>
              <select id="filter-lel" class="w-full px-3 py-2 border border-gray-300 rounded-lg input-focus text-sm">
                <option value="">全部LEL</option>
                <option value="lel1-1-1">北京东部LEL</option>
                <option value="lel1-1-2">北京西部LEL</option>
                <option value="lel1-2-1">天津LEL</option>
                <option value="lel2-1-1">上海LEL</option>
              </select>
            </div>
            <div>
              <label for="filter-gradient" class="block text-sm font-medium text-gray-700 mb-1">客户梯度</label>
              <select id="filter-gradient" class="w-full px-3 py-2 border border-gray-300 rounded-lg input-focus text-sm">
                <option value="">全部梯度</option>
                <option value="VVIP">VVIP</option>
                <option value="T1">T1</option>
                <option value="T2">T2</option>
                <option value="T3">T3</option>
              </select>
            </div>
          </div>
        </div>
        
        <!-- 客户列表 -->
        <div class="bg-white rounded-xl card-shadow overflow-hidden">
          <div class="overflow-x-auto">
            <table class="w-full text-sm">
              <thead>
                <tr class="bg-gray-50">
                  <th class="text-left py-3 px-4 font-semibold w-10">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </th>
                  <th class="text-left py-3 px-4 font-semibold">客户信息</th>
                  <th class="text-left py-3 px-4 font-semibold">层级信息</th>
                  <th class="text-left py-3 px-4 font-semibold">对接同事</th>
                  <th class="text-left py-3 px-4 font-semibold">梯度</th>
                  <th class="text-left py-3 px-4 font-semibold">拜访状态</th>
                  <th class="text-right py-3 px-4 font-semibold">操作</th>
                </tr>
              </thead>
              <tbody>
                <tr class="border-b border-gray-100 hover:bg-gray-50">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">张三</div>
                    <div class="text-gray-500 text-xs">北京协和医院 · 心内科 · 主任医师</div>
                  </td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京东部LEL</div>
                  </td>
                  <td class="py-3 px-4">张同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-primary/10 text-primary text-xs rounded-full">VVIP</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-success/10 text-success text-xs rounded-full">已拜访</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary" title="编辑">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger" title="删除">
                        <i class="fa fa-trash"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看拜访详情" data-id="1">
                        <i class="fa fa-eye"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑拜访记录" data-id="1">
                        <i class="fa fa-edit"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">李四</div>
                    <div class="text-gray-500 text-xs">北京301医院 · 心外科 · 副主任医师</div>
                  </td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京东部LEL</div>
                  </td>
                  <td class="py-3 px-4">张同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-primary/10 text-primary text-xs rounded-full">VVIP</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-success/10 text-success text-xs rounded-full">已拜访</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary" title="编辑">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger" title="删除">
                        <i class="fa fa-trash"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看拜访详情" data-id="2">
                        <i class="fa fa-eye"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑拜访记录" data-id="2">
                        <i class="fa fa-edit"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">王五</div>
                    <div class="text-gray-500 text-xs">北京天坛医院 · 神经内科 · 主任医师</div>
                  </td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京东部LEL</div>
                  </td>
                  <td class="py-3 px-4">李同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-secondary/10 text-secondary text-xs rounded-full">T1</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-warning/10 text-warning text-xs rounded-full">未拜访</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary" title="编辑">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger" title="删除">
                        <i class="fa fa-trash"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看拜访详情" data-id="3">
                        <i class="fa fa-eye"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="添加拜访记录" data-id="3">
                        <i class="fa fa-edit"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">赵六</div>
                    <div class="text-gray-500 text-xs">北京大学第一医院 · 消化内科 · 主治医师</div>
                  </td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京西部LEL</div>
                  </td>
                  <td class="py-3 px-4">王同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-warning/10 text-warning text-xs rounded-full">T2</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-success/10 text-success text-xs rounded-full">已拜访</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary" title="编辑">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger" title="删除">
                        <i class="fa fa-trash"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看拜访详情" data-id="4">
                        <i class="fa fa-eye"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑拜访记录" data-id="4">
                        <i class="fa fa-edit"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="hover:bg-gray-50">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">周八</div>
                    <div class="text-gray-500 text-xs">上海瑞金医院 · 神经内科 · 副主任医师</div>
                  </td>
                  <td class="py-3 px-4">
                    <div>南区RCL</div>
                    <div class="text-gray-500 text-xs">上海REL · 上海LEL</div>
                  </td>
                  <td class="py-3 px-4">陈同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-warning/10 text-warning text-xs rounded-full">T2</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-warning/10 text-warning text-xs rounded-full">未拜访</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary" title="编辑">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger" title="删除">
                        <i class="fa fa-trash"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看拜访详情" data-id="5">
                        <i class="fa fa-eye"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="添加拜访记录" data-id="5">
                        <i class="fa fa-edit"></i>
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          
          <!-- 分页 -->
          <div class="px-4 py-3 flex items-center justify-between border-t border-gray-200">
            <div class="flex-1 flex justify-between sm:hidden">
              <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
                上一页
              </button>
              <button class="ml-3 relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
                下一页
              </button>
            </div>
            <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between">
              <div>
                <p class="text-sm text-gray-700">
                  显示第 <span class="font-medium">1</span> 至 <span class="font-medium">5</span> 条，共 <span class="font-medium">24</span> 条结果
                </p>
              </div>
              <div>
                <nav class="relative z-0 inline-flex rounded-md shadow-sm -space-x-px" aria-label="Pagination">
                  <button class="relative inline-flex items-center px-2 py-2 rounded-l-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                    <span class="sr-only">上一页</span>
                    <i class="fa fa-chevron-left text-xs"></i>
                  </button>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-primary text-sm font-medium text-white">
                    1
                  </button>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                    2
                  </button>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                    3
                  </button>
                  <span class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700">
                    ...
                  </span>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                    5
                  </button>
                  <button class="relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                    <span class="sr-only">下一页</span>
                    <i class="fa fa-chevron-right text-xs"></i>
                  </button>
                </nav>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 客户拜访记录页面 -->
      <div id="visit-records-page" class="page-content hidden">
        <div class="flex flex-col md:flex-row md:items-center md:justify-between mb-6">
          <h2 class="text-2xl font-bold mb-4 md:mb-0">客户拜访记录</h2>
          <div class="flex flex-wrap gap-3">
            <div class="relative">
              <input type="text" placeholder="搜索客户或记录..." class="pl-10 pr-4 py-2 border border-gray-300 rounded-lg input-focus w-full md:w-64 text-sm">
              <i class="fa fa-search absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
            </div>
            <div class="relative">
              <select class="pl-3 pr-10 py-2 bg-white border border-gray-300 rounded-lg input-focus appearance-none text-sm">
                <option>全部状态</option>
                <option>已完成</option>
                <option>未完成</option>
              </select>
              <i class="fa fa-chevron-down absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 pointer-events-none text-xs"></i>
            </div>
            <button class="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/90 btn-hover text-sm flex items-center">
              <i class="fa fa-filter mr-1"></i> 高级筛选
            </button>
          </div>
        </div>
        
        <!-- 拜访记录列表 -->
        <div class="bg-white rounded-xl card-shadow overflow-hidden">
          <div class="overflow-x-auto">
            <table class="w-full text-sm">
              <thead>
                <tr class="bg-gray-50">
                  <th class="text-left py-3 px-4 font-semibold w-10">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </th>
                  <th class="text-left py-3 px-4 font-semibold">客户信息</th>
                  <th class="text-left py-3 px-4 font-semibold">拜访日期</th>
                  <th class="text-left py-3 px-4 font-semibold">层级信息</th>
                  <th class="text-left py-3 px-4 font-semibold">对接同事</th>
                  <th class="text-left py-3 px-4 font-semibold">拜访态度</th>
                  <th class="text-left py-3 px-4 font-semibold">状态</th>
                  <th class="text-right py-3 px-4 font-semibold">操作</th>
                </tr>
              </thead>
              <tbody>
                <tr class="border-b border-gray-100 hover:bg-gray-50" data-record-id="1">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">张三</div>
                    <div class="text-gray-500 text-xs">北京协和医院 · VVIP</div>
                  </td>
                  <td class="py-3 px-4">2023-06-15</td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京东部LEL</div>
                  </td>
                  <td class="py-3 px-4">张同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 status-倡领 text-xs rounded-full">倡领</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-success/10 text-success text-xs rounded-full">已完成</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看报告" data-id="1">
                        <i class="fa fa-file-text-o"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑" data-id="1">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger delete-visit-record" title="删除" data-id="1">
                        <i class="fa fa-trash"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50" data-record-id="2">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">李四</div>
                    <div class="text-gray-500 text-xs">北京301医院 · VVIP</div>
                  </td>
                  <td class="py-3 px-4">2023-06-12</td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京东部LEL</div>
                  </td>
                  <td class="py-3 px-4">张同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 status-支持 text-xs rounded-full">支持</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-success/10 text-success text-xs rounded-full">已完成</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看报告" data-id="2">
                        <i class="fa fa-file-text-o"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑" data-id="2">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger delete-visit-record" title="删除" data-id="2">
                        <i class="fa fa-trash"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50" data-record-id="3">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">赵六</div>
                    <div class="text-gray-500 text-xs">北京大学第一医院 · T2</div>
                  </td>
                  <td class="py-3 px-4">2023-06-10</td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京西部LEL</div>
                  </td>
                  <td class="py-3 px-4">王同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 status-中立 text-xs rounded-full">中立</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-success/10 text-success text-xs rounded-full">已完成</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看报告" data-id="3">
                        <i class="fa fa-file-text-o"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑" data-id="3">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger delete-visit-record" title="删除" data-id="3">
                        <i class="fa fa-trash"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="border-b border-gray-100 hover:bg-gray-50" data-record-id="4">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">王五</div>
                    <div class="text-gray-500 text-xs">北京天坛医院 · T1</div>
                  </td>
                  <td class="py-3 px-4">2023-06-20</td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">北京REL · 北京东部LEL</div>
                  </td>
                  <td class="py-3 px-4">李同事</td>
                  <td class="py-3 px-4">
                    <span class="text-gray-400">-</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-warning/10 text-warning text-xs rounded-full">未完成</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-success" title="标记为完成">
                        <i class="fa fa-check"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑" data-id="4">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger delete-visit-record" title="删除" data-id="4">
                        <i class="fa fa-trash"></i>
                      </button>
                    </div>
                  </td>
                </tr>
                <tr class="hover:bg-gray-50" data-record-id="5">
                  <td class="py-3 px-4">
                    <input type="checkbox" class="rounded border-gray-300 text-primary focus:ring-primary">
                  </td>
                  <td class="py-3 px-4">
                    <div class="font-medium">孙七</div>
                    <div class="text-gray-500 text-xs">天津医科大学总医院 · T1</div>
                  </td>
                  <td class="py-3 px-4">2023-06-05</td>
                  <td class="py-3 px-4">
                    <div>北区RCL</div>
                    <div class="text-gray-500 text-xs">天津REL · 天津LEL</div>
                  </td>
                  <td class="py-3 px-4">刘同事</td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 status-反对 text-xs rounded-full">反对</span>
                  </td>
                  <td class="py-3 px-4">
                    <span class="px-2 py-1 bg-success/10 text-success text-xs rounded-full">已完成</span>
                  </td>
                  <td class="py-3 px-4 text-right">
                    <div class="flex justify-end space-x-2">
                      <button class="text-gray-500 hover:text-primary view-visit-details" title="查看报告" data-id="5">
                        <i class="fa fa-file-text-o"></i>
                      </button>
                      <button class="text-gray-500 hover:text-primary edit-visit-form" title="编辑" data-id="5">
                        <i class="fa fa-pencil"></i>
                      </button>
                      <button class="text-gray-500 hover:text-danger delete-visit-record" title="删除" data-id="5">
                        <i class="fa fa-trash"></i>
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          
          <!-- 分页 -->
          <div class="px-4 py-3 flex items-center justify-between border-t border-gray-200">
            <div class="flex-1 flex justify-between sm:hidden">
              <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
                上一页
              </button>
              <button class="ml-3 relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
                下一页
              </button>
            </div>
            <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between">
              <div>
                <p class="text-sm text-gray-700">
                  显示第 <span class="font-medium">1</span> 至 <span class="font-medium">5</span> 条，共 <span class="font-medium">32</span> 条结果
                </p>
              </div>
              <div>
                <nav class="relative z-0 inline-flex rounded-md shadow-sm -space-x-px" aria-label="Pagination">
                  <button class="relative inline-flex items-center px-2 py-2 rounded-l-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                    <span class="sr-only">上一页</span>
                    <i class="fa fa-chevron-left text-xs"></i>
                  </button>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-primary text-sm font-medium text-white">
                    1
                  </button>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                    2
                  </button>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                    3
                  </button>
                  <span class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700">
                    ...
                  </span>
                  <button class="relative inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 hover:bg-gray-50">
                    7
                  </button>
                  <button class="relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm font-medium text-gray-500 hover:bg-gray-50">
                    <span class="sr-only">下一页</span>
                    <i class="fa fa-chevron-right text-xs"></i>
                  </button>
                </nav>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 系统设置页面 -->
      <div id="settings-page" class="page-content hidden">
        <h2 class="text-2xl font-bold mb-6">系统设置</h2>
        <!-- 设置内容将在这里显示 -->
      </div>
    </main>
  </div>

  <!-- 拜访记录表单模态框 -->
  <div id="visit-form-modal" class="fixed inset-0 bg-black/50 z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white rounded-xl w-full max-w-2xl max-h-[90vh] overflow-y-auto">
      <div class="p-6 border-b">
        <div class="flex justify-between items-center">
          <h3 class="text-xl font-bold" id="visit-form-title">添加拜访记录</h3>
          <button id="close-form-modal" class="text-gray-500 hover:text-gray-700">
            <i class="fa fa-times text-xl"></i>
          </button>
        </div>
      </div>
      <div class="p-6">
        <form id="visit-form">
          <input type="hidden" id="visit-id">
          <input type="hidden" id="customer-id">
          
          <div class="mb-5">
            <label for="visit-date" class="block text-sm font-medium text-gray-700 mb-1">拜访日期</label>
            <input type="date" id="visit-date" class="w-full px-3 py-2 border border-gray-300 rounded-lg input-focus text-sm">
          </div>
          
          <div class="mb-5">
            <label class="block text-sm font-medium text-gray-700 mb-1">1. 是否了解基药相关政策及背景？</label>
            <div class="flex space-x-4">
              <label class="inline-flex items-center">
                <input type="radio" name="know-policy" value="是" class="text-primary focus:ring-primary">
                <span class="ml-2">是</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" name="know-policy" value="否" class="text-primary focus:ring-primary">
                <span class="ml-2">否</span>
              </label>
            </div>
          </div>
          
          <div class="mb-5">
            <label class="block text-sm font-medium text-gray-700 mb-1">2. 近期是否参与到卫健委基药工作中（会议 / 咨询等）</label>
            <div class="flex flex-wrap gap-4">
              <label class="inline-flex items-center">
                <input type="radio" name="participate" value="是" class="text-primary focus:ring-primary">
                <span class="ml-2">是</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" name="participate" value="否" class="text-primary focus:ring-primary">
                <span class="ml-2">否</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" name="participate" value="即将参与" class="text-primary focus:ring-primary">
                <span class="ml-2">即将参与</span>
              </label>
            </div>
          </div>
          
          <div class="mb-5">
            <label class="block text-sm font-medium text-gray-700 mb-1">3. 是否认可阿来替尼的 3+6 优势？</label>
            <div class="flex space-x-4">
              <label class="inline-flex items-center">
                <input type="radio" name="approve-advantage" value="是" class="text-primary focus:ring-primary">
                <span class="ml-2">是</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" name="approve-advantage" value="否" class="text-primary focus:ring-primary">
                <span class="ml-2">否</span>
              </label>
            </div>
          </div>
          
          <div class="mb-5" id="disapprove-reason-container">
            <label for="disapprove-reason" class="block text-sm font-medium text-gray-700 mb-1">4. 不认可阿来替尼的 3+6 优势的原因</label>
            <textarea id="disapprove-reason" rows="3" class="w-full px-3 py-2 border border-gray-300 rounded-lg input-focus text-sm"></textarea>
          </div>
          
          <div class="mb-5">
            <label class="block text-sm font-medium text-gray-700 mb-1">5. 专家反馈态度</label>
            <div class="flex flex-wrap gap-4">
              <label class="inline-flex items-center">
                <input type="radio" name="attitude" value="倡领" class="text-primary focus:ring-primary">
                <span class="ml-2">倡领</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" name="attitude" value="支持" class="text-primary focus:ring-primary">
                <span class="ml-2">支持</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" name="attitude" value="中立" class="text-primary focus:ring-primary">
                <span class="ml-2">中立</span>
              </label>
              <label class="inline-flex items-center">
                <input type="radio" name="attitude" value="反对" class="text-primary focus:ring-primary">
                <span class="ml-2">反对</span>
              </label>
            </div>
          </div>
          
          <div class="mb-5" id="neutral-oppose-reason-container">
            <label for="neutral-oppose-reason" class="block text-sm font-medium text-gray-700 mb-1">6. 专家中立 / 反对的原因</label>
            <textarea id="neutral-oppose-reason" rows="3" class="w-full px-3 py-2 border border-gray-300 rounded-lg input-focus text-sm"></textarea>
          </div>
          
          <div class="flex justify-end space-x-3 mt-8">
            <button type="button" id="cancel-visit-form" class="px-4 py-2 border border-gray-300 text-gray-700 rounded-lg hover:bg-gray-50 btn-hover text-sm">
              取消
            </button>
            <button type="submit" class="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/90 btn-hover text-sm">
              保存记录
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- 拜访详情模态框 -->
  <div id="visit-details-modal" class="fixed inset-0 bg-black/50 z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white rounded-xl w-full max-w-2xl max-h-[90vh] overflow-y-auto">
      <div class="p-6 border-b">
        <div class="flex justify-between items-center">
          <h3 class="text-xl font-bold">拜访详情</h3>
          <button id="close-details-modal" class="text-gray-500 hover:text-gray-700">
            <i class="fa fa-times text-xl"></i>
          </button>
        </div>
      </div>
      <div class="p-6">
        <div id="visit-details-content">
          <!-- 拜访详情内容将通过JS动态填充 -->
        </div>
        <div class="mt-6 flex justify-end">
          <button id="close-details-btn" class="px-4 py-2 bg-primary text-white rounded-lg hover:bg-primary/90 btn-hover text-sm">
            关闭
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- 删除确认模态框 -->
  <div id="delete-confirm-modal" class="fixed inset-0 bg-black/50 z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white rounded-xl w-full max-w-md">
      <div class="p-6 border-b">
        <h3 class="text-xl font-bold">确认删除</h3>
      </div>
      <div class="p-6">
        <p class="text-gray-600 mb-6">您确定要删除这条拜访记录吗？此操作不可撤销。</p>
        <input type="hidden" id="delete-record-id">
        <div class="flex justify-end space-x-3">
          <button id="cancel-delete" class="px-4 py-2 border border-gray-300 text-gray-700 rounded-lg hover:bg-gray-50 btn-hover text-sm">
            取消
          </button>
          <button id="confirm-delete" class="px-4 py-2 bg-danger text-white rounded-lg hover:bg-danger/90 btn-hover text-sm">
            确认删除
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- JavaScript -->
  <script>
    // 模拟拜访记录数据
    const visitRecords = [
      {
        id: 1,
        customerId: 1,
        customerName: "张三",
        customerHospital: "北京协和医院",
        customerLevel: "VVIP",
        visitDate: "2023-06-15",
        knowPolicy: "是",
        participate: "是",
        approveAdvantage: "是",
        disapproveReason: "",
        attitude: "倡领",
        neutralOpposeReason: ""
      },
      {
        id: 2,
        customerId: 2,
        customerName: "李四",
        customerHospital: "北京301医院",
        customerLevel: "VVIP",
        visitDate: "2023-06-12",
        knowPolicy: "是",
        participate: "即将参与",
        approveAdvantage: "是",
        disapproveReason: "",
        attitude: "支持",
        neutralOpposeReason: ""
      },
      {
        id: 3,
        customerId: 4,
        customerName: "赵六",
        customerHospital: "北京大学第一医院",
        customerLevel: "T2",
        visitDate: "2023-06-10",
        knowPolicy: "否",
        participate: "否",
        approveAdvantage: "是",
        disapproveReason: "",
        attitude: "中立",
        neutralOpposeReason: "对长期疗效数据持观望态度"
      },
      {
        id: 5,
        customerId: 6,
        customerName: "孙七",
        customerHospital: "天津医科大学总医院",
        customerLevel: "T1",
        visitDate: "2023-06-05",
        knowPolicy: "是",
        participate: "是",
        approveAdvantage: "否",
        disapproveReason: "认为价格偏高，性价比不足",
        attitude: "反对",
        neutralOpposeReason: "更倾向于使用其他同类药物"
      }
    ];

    // 页面导航
    document.addEventListener('DOMContentLoaded', function() {
      // 侧边栏导航切换
      const navLinks = document.querySelectorAll('nav a');
      const pageContents = document.querySelectorAll('.page-content');
      
      navLinks.forEach(link => {
        link.addEventListener('click', function(e) {
          e.preventDefault();
          
          // 移除所有激活状态
          navLinks.forEach(l => l.classList.remove('sidebar-active'));
          pageContents.forEach(p => p.classList.add('hidden'));
          
          // 添加当前激活状态
          this.classList.add('sidebar-active');
          
          // 显示对应页面
          const targetId = this.getAttribute('href').substring(1) + '-page';
          document.getElementById(targetId).classList.remove('hidden');
        });
      });
      
      // 侧边栏折叠/展开
      const toggleSidebarBtn = document.getElementById('toggleSidebar');
      const sidebar = document.getElementById('sidebar');
      
      toggleSidebarBtn.addEventListener('click', function() {
        sidebar.classList.toggle('w-64');
        sidebar.classList.toggle('w-20');
        
        // 隐藏/显示文字
        const sidebarTexts = document.querySelectorAll('nav a span');
        sidebarTexts.forEach(text => {
          text.classList.toggle('hidden');
        });
        
        // 调整图标位置
        const sidebarIcons = document.querySelectorAll('nav a i');
        sidebarIcons.forEach(icon => {
          if (sidebar.classList.contains('w-20')) {
            icon.classList.add('mr-0', 'mx-auto');
            icon.classList.remove('mr-3');
          } else {
            icon.classList.remove('mr-0', 'mx-auto');
            icon.classList.add('mr-3');
          }
        });
      });
      
      // 初始化图表
      initCharts();
      
      // 模态框相关事件绑定
      initModals();
      
      // 删除拜访记录事件
      initDeleteFunctionality();
      
      // 拜访表单逻辑
      initVisitFormLogic();
    });
    
    // 初始化模态框相关功能
    function initModals() {
      // 打开拜访表单
      document.querySelectorAll('.edit-visit-form').forEach(button => {
        button.addEventListener('click', function() {
          const customerId = this.getAttribute('data-id');
          const record = visitRecords.find(r => r.customerId == customerId);
          
          // 重置表单
          document.getElementById('visit-form').reset();
          document.getElementById('disapprove-reason-container').classList.add('hidden');
          document.getElementById('neutral-oppose-reason-container').classList.add('hidden');
          
          // 设置表单值
          document.getElementById('customer-id').value = customerId;
          
          if (record) {
            // 编辑现有记录
            document.getElementById('visit-form-title').textContent = '编辑拜访记录';
            document.getElementById('visit-id').value = record.id;
            document.getElementById('visit-date').value = record.visitDate;
            
            // 设置单选按钮
            document.querySelector(`input[name="know-policy"][value="${record.knowPolicy}"]`).checked = true;
            document.querySelector(`input[name="participate"][value="${record.participate}"]`).checked = true;
            document.querySelector(`input[name="approve-advantage"][value="${record.approveAdvantage}"]`).checked = true;
            document.querySelector(`input[name="attitude"][value="${record.attitude}"]`).checked = true;
            
            // 设置文本框
            document.getElementById('disapprove-reason').value = record.disapproveReason;
            document.getElementById('neutral-oppose-reason').value = record.neutralOpposeReason;
            
            // 显示相关文本框
            if (record.approveAdvantage === '否') {
              document.getElementById('disapprove-reason-container').classList.remove('hidden');
            }
            
            if (record.attitude === '中立' || record.attitude === '反对') {
              document.getElementById('neutral-oppose-reason-container').classList.remove('hidden');
            }
          } else {
            // 添加新记录
            document.getElementById('visit-form-title').textContent = '添加拜访记录';
            document.getElementById('visit-id').value = '';
            // 设置默认日期为今天
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('visit-date').value = today;
          }
          
          // 显示模态框
          document.getElementById('visit-form-modal').classList.remove('hidden');
        });
      });
      
      // 关闭拜访表单
      document.getElementById('close-form-modal').addEventListener('click', function() {
        document.getElementById('visit-form-modal').classList.add('hidden');
      });
      
      document.getElementById('cancel-visit-form').addEventListener('click', function() {
        document.getElementById('visit-form-modal').classList.add('hidden');
      });
      
      // 打开拜访详情
      document.querySelectorAll('.view-visit-details').forEach(button => {
        button.addEventListener('click', function() {
          const recordId = this.getAttribute('data-id');
          const record = visitRecords.find(r => r.id == recordId || r.customerId == recordId);
          
          if (record) {
            let detailsHtml = `
              <div class="mb-6">
                <h4 class="text-lg font-semibold mb-2">${record.customerName}</h4>
                <p class="text-gray-500 text-sm">${record.customerHospital} · ${record.customerLevel}</p>
                <p class="text-gray-500 text-sm mt-1">拜访日期: ${record.visitDate}</p>
              </div>
              
              <div class="space-y-4">
                <div class="p-4 bg-gray-50 rounded-lg">
                  <p class="font-medium mb-1">1. 是否了解基药相关政策及背景？</p>
                  <p>${record.knowPolicy}</p>
                </div>
                
                <div class="p-4 bg-gray-50 rounded-lg">
                  <p class="font-medium mb-1">2. 近期是否参与到卫健委基药工作中？</p>
                  <p>${record.participate}</p>
                </div>
                
                <div class="p-4 bg-gray-50 rounded-lg">
                  <p class="font-medium mb-1">3. 是否认可阿来替尼的 3+6 优势？</p>
                  <p>${record.approveAdvantage}</p>
                  ${record.approveAdvantage === '否' ? `
                    <div class="mt-2">
                      <p class="font-medium mb-1">不认可原因：</p>
                      <p>${record.disapproveReason}</p>
                    </div>
                  ` : ''}
                </div>
                
                <div class="p-4 bg-gray-50 rounded-lg">
                  <p class="font-medium mb-1">4. 专家反馈态度？</p>
                  <p><span class="px-2 py-1 status-${record.attitude} text-xs rounded-full">${record.attitude}</span></p>
                  ${(record.attitude === '中立' || record.attitude === '反对') ? `
                    <div class="mt-2">
                      <p class="font-medium mb-1">原因：</p>
                      <p>${record.neutralOpposeReason}</p>
                    </div>
                  ` : ''}
                </div>
              </div>
            `;
            
            document.getElementById('visit-details-content').innerHTML = detailsHtml;
            document.getElementById('visit-details-modal').classList.remove('hidden');
          }
        });
      });
      
      // 关闭拜访详情
      document.getElementById('close-details-modal').addEventListener('click', function() {
        document.getElementById('visit-details-modal').classList.add('hidden');
      });
      
      document.getElementById('close-details-btn').addEventListener('click', function() {
        document.getElementById('visit-details-modal').classList.add('hidden');
      });
    }
    
    // 初始化删除功能
    function initDeleteFunctionality() {
      // 打开删除确认
      document.querySelectorAll('.delete-visit-record').forEach(button => {
        button.addEventListener('click', function() {
          const recordId = this.getAttribute('data-id');
          document.getElementById('delete-record-id').value = recordId;
          document.getElementById('delete-confirm-modal').classList.remove('hidden');
        });
      });
      
      // 关闭删除确认
      document.getElementById('cancel-delete').addEventListener('click', function() {
        document.getElementById('delete-confirm-modal').classList.add('hidden');
      });
      
      // 确认删除
      document.getElementById('confirm-delete').addEventListener('click', function() {
        const recordId = document.getElementById('delete-record-id').value;
        
        // 从数据中删除
        const index = visitRecords.findIndex(r => r.id == recordId);
        if (index !== -1) {
          visitRecords.splice(index, 1);
        }
        
        // 从DOM中删除
        const row = document.querySelector(`tr[data-record-id="${recordId}"]`);
        if (row) {
          row.remove();
        }
        
        // 关闭模态框
        document.getElementById('delete-confirm-modal').classList.add('hidden');
        
        // 显示成功提示（实际项目中可以使用toast组件）
        alert('拜访记录已成功删除');
      });
    }
    
    // 初始化拜访表单逻辑
    function initVisitFormLogic() {
      // 监听"是否认可"选项变化
      document.querySelectorAll('input[name="approve-advantage"]').forEach(radio => {
        radio.addEventListener('change', function() {
          if (this.value === '否') {
            document.getElementById('disapprove-reason-container').classList.remove('hidden');
          } else {
            document.getElementById('disapprove-reason-container').classList.add('hidden');
          }
        });
      });
      
      // 监听"专家态度"选项变化
      document.querySelectorAll('input[name="attitude"]').forEach(radio => {
        radio.addEventListener('change', function() {
          if (this.value === '中立' || this.value === '反对') {
            document.getElementById('neutral-oppose-reason-container').classList.remove('hidden');
          } else {
            document.getElementById('neutral-oppose-reason-container').classList.add('hidden');
          }
        });
      });
      
      // 表单提交
      document.getElementById('visit-form').addEventListener('submit', function(e) {
        e.preventDefault();
        
        // 获取表单数据
        const formData = {
          id: document.getElementById('visit-id').value || Date.now(), // 使用时间戳作为临时ID
          customerId: document.getElementById('customer-id').value,
          visitDate: document.getElementById('visit-date').value,
          knowPolicy: document.querySelector('input[name="know-policy"]:checked')?.value || '',
          participate: document.querySelector('input[name="participate"]:checked')?.value || '',
          approveAdvantage: document.querySelector('input[name="approve-advantage"]:checked')?.value || '',
          disapproveReason: document.getElementById('disapprove-reason').value,
          attitude: document.querySelector('input[name="attitude"]:checked')?.value || '',
          neutralOpposeReason: document.getElementById('neutral-oppose-reason').value
        };
        
        // 获取客户信息（实际项目中应从客户数据中获取）
        const customerRow = document.querySelector(`button[class*="edit-visit-form"][data-id="${formData.customerId}"]`).closest('tr');
        const customerName = customerRow.querySelector('.font-medium').textContent;
        const customerDetails = customerRow.querySelector('.text-gray-500').textContent.split(' · ');
        const customerHospital = customerDetails[0];
        const customerLevel = customerRow.querySelector('td:nth-child(5) span').textContent;
        
        // 添加客户信息到表单数据
        formData.customerName = customerName;
        formData.customerHospital = customerHospital;
        formData.customerLevel = customerLevel;
        
        // 保存数据
        const existingIndex = visitRecords.findIndex(r => r.id == formData.id);
        if (existingIndex !== -1) {
          // 更新现有记录
          visitRecords[existingIndex] = formData;
        } else {
          // 添加新记录
          visitRecords.push(formData);
        }
        
        // 关闭模态框
        document.getElementById('visit-form-modal').classList.add('hidden');
        
        // 显示成功提示（实际项目中可以使用toast组件）
        alert('拜访记录已保存成功');
      });
    }
    
    // 初始化图表
    function initCharts() {
      // RCL拜访完成情况图表
      const rclCtx = document.getElementById('rclChart').getContext('2d');
      new Chart(rclCtx, {
        type: 'bar',
        data: {
          labels: ['北区RCL', '南区RCL'],
          datasets: [
            {
              label: '已拜访',
              data: [185, 61],
              backgroundColor: '#165DFF',
              borderRadius: 4
            },
            {
              label: '未拜访',
              data: [65, 39],
              backgroundColor: '#E5E6EB',
              borderRadius: 4
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            x: {
              stacked: true,
            },
            y: {
              stacked: true,
              beginAtZero: true
            }
          },
          plugins: {
            legend: {
              position: 'top',
            }
          }
        }
      });
      
      // 客户态度分布图表
      const attitudeCtx = document.getElementById('attitudeChart').getContext('2d');
      new Chart(attitudeCtx, {
        type: 'doughnut',
        data: {
          labels: ['倡领', '支持', '中立', '反对'],
          datasets: [{
            data: [126, 99, 45, 30],
            backgroundColor: [
              '#52C41A',
              '#165DFF',
              '#FAAD14',
              '#FF4D4F'
            ],
            borderWidth: 0,
            hoverOffset: 4
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              position: 'bottom',
            }
          },
          cutout: '65%'
        }
      });
    }
  </script>
</body>
</html>
