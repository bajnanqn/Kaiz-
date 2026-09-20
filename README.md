<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>A Highsock - Financial & Order Management</title>
  
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brandDark: '#0A1912',
            brandGreen: '#12372A',
            brandEmerald: '#436850',
            brandBlue: '#1E3E62',
            brandRed: '#8B0000',
            brandAccentRed: '#E53E3E',
            brandLight: '#FBFADA'
          }
        }
      }
    }
  </script>
  
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  
  <style>
    body { background-color: #0A1912; color: #FBFADA; font-family: system-ui, -apple-system, sans-serif; }
    .glass-card { background: rgba(18, 55, 42, 0.7); backdrop-filter: blur(10px); border: 1px solid rgba(67, 104, 80, 0.3); }
    .custom-scrollbar::-webkit-scrollbar { width: 6px; }
    .custom-scrollbar::-webkit-scrollbar-track { background: #0A1912; }
    .custom-scrollbar::-webkit-scrollbar-thumb { background: #436850; border-radius: 3px; }
  </style>
</head>
<body class="min-h-screen pb-20 md:pb-0 md:pl-64 custom-scrollbar">

  <!-- Sidebar Navigation -->
  <aside class="fixed left-0 top-0 h-full w-64 glass-card border-r border-brandEmerald/30 hidden md:flex flex-col justify-between z-40">
    <div>
      <div class="p-6 border-b border-brandEmerald/30 flex items-center gap-3">
        <div class="w-10 h-10 rounded-full bg-brandRed flex items-center justify-center font-bold text-xl text-white">AH</div>
        <div>
          <h1 class="font-bold text-lg leading-tight text-white">A HIGHSOCK</h1>
          <p class="text-xs text-brandLight/60">LEDI & BEBI ERP</p>
        </div>
      </div>
      
      <nav class="p-4 space-y-2">
        <button onclick="switchTab('dashboard')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl hover:bg-brandEmerald/30 transition text-brandLight" id="nav-dashboard">
          <i data-lucide="layout-dashboard"></i> <span>Dashboard</span>
        </button>
        <button onclick="switchTab('completed')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl hover:bg-brandEmerald/30 transition text-brandLight" id="nav-completed">
          <i data-lucide="users"></i> <span>Completed Customers</span>
        </button>
        <button onclick="switchTab('employees')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl hover:bg-brandEmerald/30 transition text-brandLight" id="nav-employees">
          <i data-lucide="user-check"></i> <span>Employees</span>
        </button>
        <button onclick="switchTab('items')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl hover:bg-brandEmerald/30 transition text-brandLight" id="nav-items">
          <i data-lucide="shopping-bag"></i> <span>Item Store</span>
        </button>
        <button onclick="switchTab('reports')" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl hover:bg-brandEmerald/30 transition text-brandLight" id="nav-reports">
          <i data-lucide="bar-chart-3"></i> <span>Financial Reports</span>
        </button>
      </nav>
    </div>

    <div class="p-4 border-t border-brandEmerald/30">
      <div class="flex justify-around text-xs text-brandLight/70">
        <span class="px-2 py-1 bg-brandBlue/40 rounded border border-brandBlue">LEDI</span>
        <span class="px-2 py-1 bg-brandRed/40 rounded border border-brandRed">BEBI</span>
      </div>
    </div>
  </aside>

  <!-- Mobile Bottom Navigation -->
  <div class="md:hidden fixed bottom-0 left-0 right-0 glass-card border-t border-brandEmerald/30 flex justify-around p-3 z-50">
    <button onclick="switchTab('dashboard')" class="p-2 text-brandLight"><i data-lucide="layout-dashboard"></i></button>
    <button onclick="switchTab('completed')" class="p-2 text-brandLight"><i data-lucide="users"></i></button>
    <button onclick="switchTab('employees')" class="p-2 text-brandLight"><i data-lucide="user-check"></i></button>
    <button onclick="switchTab('items')" class="p-2 text-brandLight"><i data-lucide="shopping-bag"></i></button>
    <button onclick="switchTab('reports')" class="p-2 text-brandLight"><i data-lucide="bar-chart-3"></i></button>
  </div>

  <!-- Main Content Container -->
  <main class="p-4 md:p-8 max-w-7xl mx-auto">
    
    <!-- DASHBOARD VIEW -->
    <section id="view-dashboard" class="space-y-6">
      
      <!-- Top Section: Revenue & Profit Cards -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <!-- Total Revenue Card -->
        <div class="glass-card p-6 rounded-2xl border-l-4 border-brandBlue relative overflow-hidden">
          <div class="flex justify-between items-start">
            <div>
              <p class="text-xs uppercase tracking-wider text-brandLight/70">Total Revenue</p>

              <h2 id="dash-total-revenue" class="text-3xl md:text-5xl font-extrabold text-white mt-1">₹0</h2>
            </div>
            <div class="p-3 bg-brandBlue/30 rounded-xl text-brandBlue">
              <i data-lucide="indian-rupee" class="w-8 h-8"></i>
            </div>
          </div>
          <div class="mt-4 pt-4 border-t border-brandEmerald/20 flex justify-between text-xs text-brandLight/80">
            <span>LEDI: <strong id="dash-led-rev" class="text-white">₹0</strong></span>
            <span>BEBI: <strong id="dash-beb-rev" class="text-white">₹0</strong></span>
          </div>
        </div>

        <!-- Total Profit Card -->
        <div class="glass-card p-6 rounded-2xl border-l-4 border-brandEmerald relative overflow-hidden">
          <div class="flex justify-between items-start">
            <div>
              <p class="text-xs uppercase tracking-wider text-brandLight/70">Net Business Profit</p>
              <h2 id="dash-total-profit" class="text-3xl md:text-5xl font-extrabold text-emerald-400 mt-1">₹0</h2>
              <p class="text-[10px] text-brandLight/50 mt-1">(After Expense, Stitching & Owner Share)</p>
            </div>
            <div class="p-3 bg-brandEmerald/30 rounded-xl text-emerald-400">
              <i data-lucide="trending-up" class="w-8 h-8"></i>
            </div>
          </div>
          <div class="mt-4 pt-4 border-t border-brandEmerald/20 flex justify-between text-xs text-brandLight/80">
            <span>LEDI Profit: <strong id="dash-led-prof" class="text-emerald-400">₹0</strong></span>
            <span>BEBI Profit: <strong id="dash-beb-prof" class="text-emerald-400">₹0</strong></span>
          </div>
        </div>
      </div>

      <!-- Quick Action Buttons -->
      <div class="grid grid-cols-2 gap-4">
        <button onclick="openOrderModal()" class="glass-card p-4 rounded-xl flex items-center justify-center gap-3 bg-brandBlue/40 hover:bg-brandBlue/60 transition border border-brandBlue/50 text-white font-semibold">
          <i data-lucide="plus-circle"></i> <span>Add Order</span>
        </button>
        <button onclick="openEmployeeModal()" class="glass-card p-4 rounded-xl flex items-center justify-center gap-3 bg-brandEmerald/40 hover:bg-brandEmerald/60 transition border border-brandEmerald/50 text-white font-semibold">
          <i data-lucide="user-plus"></i> <span>Add Employee</span>
        </button>
      </div>

      <!-- Quick Status & Counts Bar -->
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
        <div class="glass-card p-3 rounded-xl text-center">
          <p class="text-[10px] text-brandLight/60 uppercase">Pending Orders</p>
          <p id="count-pending" class="text-xl font-bold text-amber-400">0</p>
        </div>
        <div class="glass-card p-3 rounded-xl text-center">
          <p class="text-[10px] text-brandLight/60 uppercase">Completed Orders</p>
          <p id="count-completed" class="text-xl font-bold text-emerald-400">0</p>
        </div>
        <div class="glass-card p-3 rounded-xl text-center">
          <p class="text-[10px] text-brandLight/60 uppercase">Total Employees</p>
          <p id="count-employees" class="text-xl font-bold text-blue-400">0</p>
        </div>
        <div class="glass-card p-3 rounded-xl text-center">
          <p class="text-[10px] text-brandLight/60 uppercase">Total Items</p>
          <p id="count-items" class="text-xl font-bold text-purple-400">0</p>
        </div>
      </div>

      <!-- Upcoming Deliveries & Active Orders -->
      <div class="glass-card rounded-2xl p-6">
        <div class="flex justify-between items-center mb-4">
          <h3 class="font-bold text-lg flex items-center gap-2">
            <i data-lucide="clock" class="text-amber-400"></i> Active Orders & Next Deliveries
          </h3>
        </div>
        <div id="active-orders-list" class="space-y-3 max-h-96 overflow-y-auto custom-scrollbar pr-1">
          <!-- Live Orders Inject Here -->
        </div>
      </div>

    </section>

    <!-- COMPLETED CUSTOMERS VIEW -->
    <section id="view-completed" class="hidden space-y-6">
      <h2 class="text-2xl font-bold flex items-center gap-2">
        <i data-lucide="users" class="text-emerald-400"></i> Completed Customers History
      </h2>
      <div class="glass-card rounded-2xl p-4 overflow-x-auto">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="border-b border-brandEmerald/30 text-xs text-brandLight/60 uppercase">
              <th class="p-3">Customer</th>
              <th class="p-3">WhatsApp</th>
              <th class="p-3">Total Orders</th>
              <th class="p-3">Total Spend</th>
              <th class="p-3">Generated Profit</th>
              <th class="p-3 text-right">Action</th>
            </tr>
          </thead>
          <tbody id="completed-customers-table" class="divide-y divide-brandEmerald/10 text-sm">
            <!-- Inject Completed Customers -->
          </tbody>
        </table>
      </div>
    </section>

    <!-- EMPLOYEES VIEW -->
    <section id="view-employees" class="hidden space-y-6">
      <div class="flex justify-between items-center">
        <h2 class="text-2xl font-bold flex items-center gap-2">
          <i data-lucide="user-check" class="text-blue-400"></i> Employee Directory & Performance
        </h2>
        <button onclick="openEmployeeModal()" class="px-4 py-2 bg-brandEmerald hover:bg-emerald-600 rounded-xl text-sm font-semibold flex items-center gap-2">
          <i data-lucide="plus"></i> Add New Employee
        </button>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4" id="employees-card-grid">
        <!-- Employee Cards Inject Here -->
      </div>
    </section>

    <!-- ITEM STORE VIEW -->
    <section id="view-items" class="hidden space-y-6">
      <div class="flex justify-between items-center">
        <h2 class="text-2xl font-bold flex items-center gap-2">
          <i data-lucide="shopping-bag" class="text-purple-400"></i> Dress Item Catalog
        </h2>
        <button onclick="openItemModal()" class="px-4 py-2 bg-brandBlue hover:bg-blue-600 rounded-xl text-sm font-semibold flex items-center gap-2">
          <i data-lucide="plus"></i> Store New Item
        </button>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4" id="items-card-grid">
        <!-- Item Store Cards Inject Here -->
      </div>
    </section>

    <!-- FINANCIAL REPORTS VIEW -->
    <section id="view-reports" class="hidden space-y-6">
      <h2 class="text-2xl font-bold flex items-center gap-2">
        <i data-lucide="bar-chart-3" class="text-amber-400"></i> Business Status Report
      </h2>
      
      <!-- Date Filter Bar -->
      <div class="glass-card p-4 rounded-xl flex flex-wrap gap-4 items-end">
        <div>
          <label class="text-xs text-brandLight/60 block mb-1">Start Date</label>
          <input type="date" id="report-start" class="bg-brandDark border border-brandEmerald/40 rounded-lg p-2 text-sm text-white">
        </div>
        <div>
          <label class="text-xs text-brandLight/60 block mb-1">End Date</label>
          <input type="date" id="report-end" class="bg-brandDark border border-brandEmerald/40 rounded-lg p-2 text-sm text-white">
        </div>
        <button onclick="generateReport()" class="px-4 py-2 bg-brandEmerald rounded-lg font-semibold text-sm flex items-center gap-2">
          <i data-lucide="filter"></i> Generate Report
        </button>
      </div>

      <!-- Report Metrics Result -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div class="glass-card p-5 rounded-xl border-t-2 border-brandBlue">
          <p class="text-xs text-brandLight/60 uppercase">Filtered Revenue</p>
          <h3 id="rep-revenue" class="text-2xl font-bold text-white mt-1">₹0</h3>
          <div class="mt-2 text-xs text-brandLight/70 space-y-1">
            <p>LEDI: <span id="rep-led-rev">₹0</span></p>
            <p>BEBI: <span id="rep-beb-rev">₹0</span></p>
          </div>
        </div>

        <div class="glass-card p-5 rounded-xl border-t-2 border-brandEmerald">
          <p class="text-xs text-brandLight/60 uppercase">Filtered Profit</p>
          <h3 id="rep-profit" class="text-2xl font-bold text-emerald-400 mt-1">₹0</h3>
          <div class="mt-2 text-xs text-brandLight/70 space-y-1">
            <p>LEDI: <span id="rep-led-prof">₹0</span></p>
            <p>BEBI: <span id="rep-beb-prof">₹0</span></p>
          </div>
        </div>

        <div class="glass-card p-5 rounded-xl border-t-2 border-amber-400">
          <p class="text-xs text-brandLight/60 uppercase">Owner Total Earnings (₹300/order)</p>
          <h3 id="rep-owner" class="text-2xl font-bold text-amber-400 mt-1">₹0</h3>
        </div>
      </div>
    </section>

  </main>

  <!-- MODALS -->

  <!-- 1. Order Modal (Create / Edit) -->
  <div id="modal-order" class="fixed inset-0 bg-black/80 hidden items-center justify-center p-4 z-50 overflow-y-auto">
    <div class="glass-card w-full max-w-2xl rounded-2xl p-6 my-8 space-y-4">
      <div class="flex justify-between items-center border-b border-brandEmerald/30 pb-3">
        <h3 id="order-modal-title" class="text-lg font-bold">New Customer Order</h3>
        <button onclick="closeModal('modal-order')" class="text-brandLight/50 hover:text-white"><i data-lucide="x"></i></button>
      </div>

      <form id="order-form" onsubmit="saveOrder(event)" class="space-y-4">
        <input type="hidden" id="order-id">
        
        <!-- Platform Choose -->
        <div class="grid grid-cols-2 gap-4">
          <label class="cursor-pointer">
            <input type="radio" name="platform" value="BEBI" checked class="peer hidden" onchange="calculateOrderFields()">
            <div class="p-3 text-center border border-brandEmerald/40 rounded-xl peer-checked:bg-brandRed peer-checked:border-brandAccentRed font-bold text-sm">
              BEBI Platform
            </div>
          </label>
          <label class="cursor-pointer">
            <input type="radio" name="platform" value="LEDI" class="peer hidden" onchange="calculateOrderFields()">
            <div class="p-3 text-center border border-brandEmerald/40 rounded-xl peer-checked:bg-brandBlue peer-checked:border-blue-400 font-bold text-sm">
              LEDI Platform
            </div>
          </label>
        </div>

        <!-- Customer Basic Details -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
          <input type="text" id="cust-name" placeholder="Customer Name" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
          <input type="text" id="cust-whatsapp" placeholder="WhatsApp Number" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
          <div>
            <label class="text-[10px] text-brandLight/60 block mb-1">Send Date</label>
            <input type="date" id="date-send" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
          </div>
          <div>
            <label class="text-[10px] text-brandLight/60 block mb-1">Address Received Date</label>
            <input type="date" id="date-address" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
          </div>
        </div>

        <!-- Item Store Preset Selection -->
        <div>
          <label class="text-xs text-brandLight/60 block mb-1">Choose Item Preset Code (Optional)</label>
          <select id="item-code-select" onchange="applyItemCodePreset()" class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
            <option value="">-- Custom Item --</option>
          </select>
        </div>

        <!-- Financial Calculations Section -->
        <div class="p-4 bg-brandDark/50 rounded-xl space-y-3 border border-brandEmerald/30">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
            <div>
              <label class="text-[10px] text-brandLight/60 block mb-1">Total Price (₹)</label>
              <input type="number" id="price-total" placeholder="0" min="0" oninput="calculateOrderFields()" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
            </div>
            <div>
              <label class="text-[10px] text-brandLight/60 block mb-1">Advance (Auto 50%)</label>
              <input type="number" id="price-advance" placeholder="0" readonly class="bg-brandDark/80 border border-brandEmerald/20 rounded-xl p-3 text-sm text-emerald-400 font-bold w-full">
              <span class="text-[10px] text-brandLight/50">Bal Advance: ₹<span id="adv-balance-text">0</span></span>
            </div>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
            <div>
              <label class="text-[10px] text-brandLight/60 block mb-1">Material Expense / Total Cost (₹)</label>
              <input type="number" id="exp-material" placeholder="0" min="0" oninput="calculateOrderFields()" class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
            </div>
            <div>
              <label class="text-[10px] text-brandLight/60 block mb-1">Assign Employee & Stitching Fee</label>
              <div class="flex gap-2">
                <select id="stitch-employee" class="bg-brandDark border border-brandEmerald/40 rounded-xl p-2 text-sm text-white w-2/3">
                  <option value="">Select Tailor</option>
                </select>
                <input type="number" id="stitch-cost" placeholder="Fee ₹" min="0" oninput="calculateOrderFields()" class="bg-brandDark border border-brandEmerald/40 rounded-xl p-2 text-sm text-white w-1/3">
              </div>
            </div>
          </div>

          <!-- Auto Profit Breakdown -->
          <div class="p-3 bg-brandEmerald/20 rounded-lg flex justify-between items-center text-xs">
            <span>Owner Share: <strong>₹300</strong></span>
            <span class="text-sm">Calculated Net Profit: <strong id="calc-net-profit" class="text-emerald-400 text-base">₹0</strong></span>
          </div>
        </div>

        <!-- Address & Description -->
        <div>
          <textarea id="cust-description" placeholder="Customer Shipping Address & Custom Notes" rows="3" class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full"></textarea>
        </div>

        <div class="flex gap-3 justify-end pt-3">
          <button type="button" onclick="closeModal('modal-order')" class="px-4 py-2 rounded-xl text-sm border border-brandEmerald/40">Cancel</button>
          <button type="submit" class="px-6 py-2 bg-brandEmerald hover:bg-emerald-600 rounded-xl text-sm font-bold text-white">Save Order</button>
        </div>
      </form>
    </div>
  </div>

  <!-- 2. Employee Modal -->
  <div id="modal-employee" class="fixed inset-0 bg-black/80 hidden items-center justify-center p-4 z-50">
    <div class="glass-card w-full max-w-md rounded-2xl p-6 space-y-4">
      <div class="flex justify-between items-center border-b border-brandEmerald/30 pb-3">
        <h3 class="text-lg font-bold">Add Employee</h3>
        <button onclick="closeModal('modal-employee')" class="text-brandLight/50"><i data-lucide="x"></i></button>
      </div>
      <form onsubmit="saveEmployee(event)" class="space-y-4">
        <input type="text" id="emp-name" placeholder="Employee Full Name" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
        <input type="text" id="emp-whatsapp" placeholder="WhatsApp Number" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
        <div class="flex gap-3 justify-end pt-2">
          <button type="button" onclick="closeModal('modal-employee')" class="px-4 py-2 rounded-xl text-sm border border-brandEmerald/40">Cancel</button>
          <button type="submit" class="px-6 py-2 bg-brandBlue hover:bg-blue-600 rounded-xl text-sm font-bold text-white">Add Staff</button>
        </div>
      </form>
    </div>
  </div>

  <!-- 3. Item Store Modal -->
  <div id="modal-item" class="fixed inset-0 bg-black/80 hidden items-center justify-center p-4 z-50">
    <div class="glass-card w-full max-w-md rounded-2xl p-6 space-y-4">
      <div class="flex justify-between items-center border-b border-brandEmerald/30 pb-3">
        <h3 class="text-lg font-bold">Store Dress Item</h3>
        <button onclick="closeModal('modal-item')" class="text-brandLight/50"><i data-lucide="x"></i></button>
      </div>
      <form onsubmit="saveItem(event)" class="space-y-4">
        <input type="text" id="item-code" placeholder="Unique Item Code (e.g. HS-101)" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
        <input type="number" id="item-price" placeholder="Default Selling Price (₹)" required class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full">
        <div>
          <label class="text-xs text-brandLight/60 block mb-1">Dress Image (Max 5MB)</label>
          <input type="file" id="item-image" accept="image/*" class="bg-brandDark border border-brandEmerald/40 rounded-xl p-2 text-xs text-white w-full">
        </div>
        <textarea id="item-desc" placeholder="Item Description / Fabric Details" rows="2" class="bg-brandDark border border-brandEmerald/40 rounded-xl p-3 text-sm text-white w-full"></textarea>

        <div class="flex gap-3 justify-end pt-2">
          <button type="button" onclick="closeModal('modal-item')" class="px-4 py-2 rounded-xl text-sm border border-brandEmerald/40">Cancel</button>
          <button type="submit" class="px-6 py-2 bg-brandBlue hover:bg-blue-600 rounded-xl text-sm font-bold text-white">Save Store Item</button>
        </div>
      </form>
    </div>
  </div>

  <!-- JAVASCRIPT APPLICATION LOGIC -->
  <script>
    // LocalStorage Initial State
    let orders = JSON.parse(localStorage.getItem('ah_orders')) || [];
    let employees = JSON.parse(localStorage.getItem('ah_employees')) || [];
    let items = JSON.parse(localStorage.getItem('ah_items')) || [];

    // App Initialization
    document.addEventListener('DOMContentLoaded', () => {
      lucide.createIcons();
      renderAll();
    });

    function saveToStorage() {
      localStorage.setItem('ah_orders', JSON.stringify(orders));
      localStorage.setItem('ah_employees', JSON.stringify(employees));
      localStorage.setItem('ah_items', JSON.stringify(items));
      renderAll();
    }

    // Tab Switcher Logic
    function switchTab(tabId) {
      ['dashboard', 'completed', 'employees', 'items', 'reports'].forEach(tab => {
        document.getElementById(`view-${tab}`).classList.add('hidden');
      });
      document.getElementById(`view-${tabId}`).classList.remove('hidden');
    }

    // Modal Handlers
    function openOrderModal(id = null) {
      document.getElementById('order-form').reset();
      document.getElementById('order-id').value = '';
      populateEmployeeDropdown();
      populateItemCodeDropdown();
      
      if(id) {
        const ord = orders.find(o => o.id === id);
        if(ord) {
          document.getElementById('order-id').value = ord.id;
          document.querySelector(`input[name="platform"][value="${ord.platform}"]`).checked = true;
          document.getElementById('cust-name').value = ord.customerName;
          document.getElementById('cust-whatsapp').value = ord.whatsapp;
          document.getElementById('date-send').value = ord.sendDate;
          document.getElementById('date-address').value = ord.addressDate;
          document.getElementById('price-total').value = ord.totalPrice;
          document.getElementById('exp-material').value = ord.materialExpense;
          document.getElementById('stitch-employee').value = ord.employeeName || '';
          document.getElementById('stitch-cost').value = ord.stitchCost;
          document.getElementById('cust-description').value = ord.addressDesc;
          calculateOrderFields();
        }
      }
      document.getElementById('modal-order').classList.remove('hidden');
      document.getElementById('modal-order').classList.add('flex');
    }

    function openEmployeeModal() {
      document.getElementById('modal-employee').classList.remove('hidden');
      document.getElementById('modal-employee').classList.add('flex');
    }

    function openItemModal() {
      document.getElementById('modal-item').classList.remove('hidden');
      document.getElementById('modal-item').classList.add('flex');
    }

    function closeModal(modalId) {
      document.getElementById(modalId).classList.add('hidden');
      document.getElementById(modalId).classList.remove('flex');
    }

    // Financial Calculation Mechanics
    function calculateOrderFields() {
      const totalPrice = parseFloat(document.getElementById('price-total').value) || 0;
      const advance = totalPrice / 2;
      const materialExp = parseFloat(document.getElementById('exp-material').value) || 0;
      const stitchCost = parseFloat(document.getElementById('stitch-cost').value) || 0;
      const ownerShare = 300;

      const advBalance = advance - materialExp;
      const netProfit = totalPrice - (materialExp + stitchCost + ownerShare);

      document.getElementById('price-advance').value = advance.toFixed(0);
      document.getElementById('adv-balance-text').innerText = advBalance.toFixed(0);
      document.getElementById('calc-net-profit').innerText = `₹${netProfit.toFixed(0)}`;
    }

    // Save Order Handler
    function saveOrder(e) {
      e.preventDefault();
      const id = document.getElementById('order-id').value || 'ORD-' + Date.now();
      const totalPrice = parseFloat(document.getElementById('price-total').value) || 0;
      const advance = totalPrice / 2;
      const materialExpense = parseFloat(document.getElementById('exp-material').value) || 0;
      const stitchCost = parseFloat(document.getElementById('stitch-cost').value) || 0;
      const ownerShare = 300;
      const netProfit = totalPrice - (materialExpense + stitchCost + ownerShare);

      const newOrder = {
        id,
        platform: document.querySelector('input[name="platform"]:checked').value,
        customerName: document.getElementById('cust-name').value,
        whatsapp: document.getElementById('cust-whatsapp').value,
        sendDate: document.getElementById('date-send').value,
        addressDate: document.getElementById('date-address').value,
        totalPrice,
        advance,
        advanceBalance: advance - materialExpense,
        materialExpense,
        employeeName: document.getElementById('stitch-employee').value,
        stitchCost,
        ownerShare,
        netProfit,
        addressDesc: document.getElementById('cust-description').value,
        status: orders.find(o => o.id === id)?.status || 'Pending'
      };

      const existingIdx = orders.findIndex(o => o.id === id);
      if(existingIdx > -1) {
        orders[existingIdx] = newOrder;
      } else {
        orders.unshift(newOrder);
      }

      saveToStorage();
      closeModal('modal-order');
    }

    // Mark Order Completed Action
    function markOrderCompleted(id) {
      const ord = orders.find(o => o.id === id);
      if(ord) {
        ord.status = 'Completed';
        saveToStorage();
      }
    }

    function deleteOrder(id) {
      if(confirm('Are you sure you want to delete this order?')) {
        orders = orders.filter(o => o.id !== id);
        saveToStorage();
      }
    }

    // Save Employee
    function saveEmployee(e) {
      e.preventDefault();
      const name = document.getElementById('emp-name').value;
      const whatsapp = document.getElementById('emp-whatsapp').value;
      
      employees.push({ id: 'EMP-' + Date.now(), name, whatsapp });
      saveToStorage();
      closeModal('modal-employee');
    }

    function deleteEmployee(id) {
      if(confirm('Remove this employee?')) {
        employees = employees.filter(emp => emp.id !== id);
        saveToStorage();
      }
    }

    // Save Store Item (Max 5MB Image)
    function saveItem(e) {
      e.preventDefault();
      const code = document.getElementById('item-code').value;
      const price = parseFloat(document.getElementById('item-price').value) || 0;
      const desc = document.getElementById('item-desc').value;
      const fileInput = document.getElementById('item-image');

      if(fileInput.files[0] && fileInput.files[0].size > 5 * 1024 * 1024) {
        alert("Image size exceeds 5MB limit!");
        return;
      }

      const processSave = (imgData = '') => {
        items.push({ id: 'ITM-' + Date.now(), code, price, desc, image: imgData });
        saveToStorage();
        closeModal('modal-item');
      };

      if(fileInput.files[0]) {
        const reader = new FileReader();
        reader.onload = (evt) => processSave(evt.target.result);
        reader.readAsDataURL(fileInput.files[0]);
      } else {
        processSave();
      }
    }

    function applyItemCodePreset() {
      const code = document.getElementById('item-code-select').value;
      const found = items.find(i => i.code === code);
      if(found) {
        document.getElementById('price-total').value = found.price;
        document.getElementById('cust-description').value = `[${found.code}] ${found.desc}`;
        calculateOrderFields();
      }
    }

    // Helper Populators
    function populateEmployeeDropdown() {
      const select = document.getElementById('stitch-employee');
      select.innerHTML = '<option value="">Select Employee</option>';
      employees.forEach(e => {
        select.innerHTML += `<option value="${e.name}">${e.name}</option>`;
      });
    }

    function populateItemCodeDropdown() {
      const select = document.getElementById('item-code-select');
      select.innerHTML = '<option value="">-- Custom Item --</option>';
      items.forEach(i => {
        select.innerHTML += `<option value="${i.code}">${i.code} (₹${i.price})</option>`;
      });
    }

    // Main Renderer Logic
    function renderAll() {
      // 1. Dashboard Financial Highlights
      const completedOrders = orders.filter(o => o.status === 'Completed');
      const activeOrders = orders.filter(o => o.status === 'Pending');

      const totalRev = orders.reduce((sum, o) => sum + o.totalPrice, 0);
      const totalProf = orders.reduce((sum, o) => sum + o.netProfit, 0);

      const ledOrders = orders.filter(o => o.platform === 'LEDI');
      const bebOrders = orders.filter(o => o.platform === 'BEBI');

      document.getElementById('dash-total-revenue').innerText = `₹${totalRev.toLocaleString()}`;
      document.getElementById('dash-total-profit').innerText = `₹${totalProf.toLocaleString()}`;
      document.getElementById('dash-led-rev').innerText = `₹${ledOrders.reduce((s, o) => s + o.totalPrice, 0).toLocaleString()}`;
      document.getElementById('dash-beb-rev').innerText = `₹${bebOrders.reduce((s, o) => s + o.totalPrice, 0).toLocaleString()}`;
      document.getElementById('dash-led-prof').innerText = `₹${ledOrders.reduce((s, o) => s + o.netProfit, 0).toLocaleString()}`;
      document.getElementById('dash-beb-prof').innerText = `₹${bebOrders.reduce((s, o) => s + o.netProfit, 0).toLocaleString()}`;

      // Counts
      document.getElementById('count-pending').innerText = activeOrders.length;
      document.getElementById('count-completed').innerText = completedOrders.length;
      document.getElementById('count-employees').innerText = employees.length;
      document.getElementById('count-items').innerText = items.length;

      // Active Orders List
      const activeContainer = document.getElementById('active-orders-list');
      activeContainer.innerHTML = '';
      if(activeOrders.length === 0) {
        activeContainer.innerHTML = `<p class="text-xs text-brandLight/50 py-4 text-center">No active pending orders.</p>`;
      } else {
        activeOrders.forEach(o => {
          activeContainer.innerHTML += `
            <div class="p-4 bg-brandDark/60 rounded-xl border border-brandEmerald/30 flex flex-wrap justify-between items-center gap-3">
              <div>
                <div class="flex items-center gap-2">
                  <span class="px-2 py-0.5 text-[10px] rounded font-bold ${o.platform === 'LEDI' ? 'bg-brandBlue/50 text-blue-300' : 'bg-brandRed/50 text-red-300'}">${o.platform}</span>
                  <h4 class="font-bold text-sm text-white">${o.customerName}</h4>
                </div>
                <p class="text-xs text-brandLight/60 mt-1"><i data-lucide="phone" class="w-3 h-3 inline"></i> ${o.whatsapp} | Delivery: <strong class="text-amber-300">${o.sendDate}</strong></p>
                <p class="text-[11px] text-brandLight/50 mt-1">Stitcher: ${o.employeeName || 'Unassigned'} | Bal Adv: ₹${o.advanceBalance}</p>
              </div>
              <div class="flex items-center gap-3">
                <div class="text-right">
                  <p class="text-sm font-bold text-white">₹${o.totalPrice}</p>
                  <p class="text-[10px] text-emerald-400">Profit: ₹${o.netProfit}</p>
                </div>
                <button onclick="markOrderCompleted('${o.id}')" title="Mark Completed" class="p-2 bg-brandEmerald/40 hover:bg-emerald-600 rounded-lg text-emerald-300"><i data-lucide="check"></i></button>
                <button onclick="openOrderModal('${o.id}')" title="Edit" class="p-2 bg-brandBlue/40 hover:bg-blue-600 rounded-lg text-blue-300"><i data-lucide="edit-2"></i></button>
                <button onclick="deleteOrder('${o.id}')" title="Delete" class="p-2 bg-brandRed/40 hover:bg-red-600 rounded-lg text-red-300"><i data-lucide="trash-2"></i></button>
              </div>
            </div>
          `;
        });
      }

      // 2. Completed Customers View
      const completedTable = document.getElementById('completed-customers-table');
      completedTable.innerHTML = '';
      
      // Group completed by Customer Name
      const customerGroups = {};
      completedOrders.forEach(o => {
        if(!customerGroups[o.customerName]) {
          customerGroups[o.customerName] = { name: o.customerName, whatsapp: o.whatsapp, orders: [] };
        }
        customerGroups[o.customerName].orders.push(o);
      });

      Object.values(customerGroups).forEach(c => {
        const cTotalSpend = c.orders.reduce((s, o) => s + o.totalPrice, 0);
        const cTotalProfit = c.orders.reduce((s, o) => s + o.netProfit, 0);
        completedTable.innerHTML += `
          <tr>
            <td class="p-3 font-semibold text-white">${c.name}</td>
            <td class="p-3">${c.whatsapp}</td>
            <td class="p-3">${c.orders.length} Orders</td>
            <td class="p-3 font-bold text-white">₹${cTotalSpend}</td>
            <td class="p-3 font-bold text-emerald-400">₹${cTotalProfit}</td>
            <td class="p-3 text-right">
              <button onclick="alert('Order History:\\n' + '${c.orders.map(o => o.sendDate + ': ₹' + o.totalPrice).join('\\n')}')" class="p-1 bg-brandBlue/40 rounded text-xs px-2"><i data-lucide="eye" class="w-3 h-3 inline"></i> Details</button>
            </td>
          </tr>
        `;
      });

      // 3. Employee View Cards
      const empGrid = document.getElementById('employees-card-grid');
      empGrid.innerHTML = '';
      employees.forEach(emp => {
        const empOrders = orders.filter(o => o.employeeName === emp.name);
        const finished = empOrders.filter(o => o.status === 'Completed');
        const pending = empOrders.filter(o => o.status === 'Pending');
        const totalPaidStitching = finished.reduce((s, o) => s + o.stitchCost, 0);

        empGrid.innerHTML += `
          <div class="glass-card p-5 rounded-2xl flex justify-between items-start">
            <div>
              <h3 class="font-bold text-lg text-white">${emp.name}</h3>
              <p class="text-xs text-brandLight/60 mt-1"><i data-lucide="phone" class="w-3 h-3 inline"></i> ${emp.whatsapp}</p>
              <div class="mt-4 flex gap-4 text-xs">
                <div>Finished: <strong class="text-emerald-400">${finished.length}</strong></div>
                <div>Pending: <strong class="text-amber-400">${pending.length}</strong></div>
              </div>
              <p class="text-xs text-brandLight/80 mt-2">Stitching Fees Earned: <strong class="text-white">₹${totalPaidStitching}</strong></p>
            </div>
            <button onclick="deleteEmployee('${emp.id}')" class="text-brandRed/70 hover:text-red-400 p-2"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
          </div>
        `;
      });

      // 4. Item Store Grid
      const itemGrid = document.getElementById('items-card-grid');
      itemGrid.innerHTML = '';
      items.forEach(itm => {
        itemGrid.innerHTML += `
          <div class="glass-card rounded-2xl overflow-hidden">
            ${itm.image ? `<img src="${itm.image}" class="w-full h-40 object-cover">` : `<div class="w-full h-40 bg-brandDark/80 flex items-center justify-center text-xs text-brandLight/40">No Image</div>`}
            <div class="p-4">
              <div class="flex justify-between items-center">
                <span class="px-2 py-0.5 bg-purple-900/50 text-purple-300 rounded font-bold text-xs">${itm.code}</span>
                <span class="font-bold text-white text-base">₹${itm.price}</span>
              </div>
              <p class="text-xs text-brandLight/70 mt-2 line-clamp-2">${itm.desc}</p>
            </div>
          </div>
        `;
      });

      lucide.createIcons();
    }

    // Reports Generation
    function generateReport() {
      const start = document.getElementById('report-start').value;
      const end = document.getElementById('report-end').value;

      let filtered = orders;
      if(start) filtered = filtered.filter(o => o.sendDate >= start);
      if(end) filtered = filtered.filter(o => o.sendDate <= end);

      const totalRev = filtered.reduce((s, o) => s + o.totalPrice, 0);
      const totalProf = filtered.reduce((s, o) => s + o.netProfit, 0);
      const totalOwner = filtered.reduce((s, o) => s + o.ownerShare, 0);

      const led = filtered.filter(o => o.platform === 'LEDI');
      const beb = filtered.filter(o => o.platform === 'BEBI');

      document.getElementById('rep-revenue').innerText = `₹${totalRev.toLocaleString()}`;
      document.getElementById('rep-profit').innerText = `₹${totalProf.toLocaleString()}`;
      document.getElementById('rep-owner').innerText = `₹${totalOwner.toLocaleString()}`;

      document.getElementById('rep-led-rev').innerText = `₹${led.reduce((s, o) => s + o.totalPrice, 0).toLocaleString()}`;
      document.getElementById('rep-beb-rev').innerText = `₹${beb.reduce((s, o) => s + o.totalPrice, 0).toLocaleString()}`;
      document.getElementById('rep-led-prof').innerText = `₹${led.reduce((s, o) => s + o.netProfit, 0).toLocaleString()}`;
      document.getElementById('rep-beb-prof').innerText = `₹${beb.reduce((s, o) => s + o.netProfit, 0).toLocaleString()}`;
    }
  </script>
</body>
</html>
