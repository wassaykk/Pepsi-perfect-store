<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Pepsi Beverage Management System — Admin/Customer</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <style>
    :root{
      --primary-blue:#0066CC; --secondary-blue:#004C97; --success:#28a745; --warning:#ffc107; --danger:#dc3545;
      --bg:#f0f2f5; --card:#fff; --muted:#666;
    }
    *{box-sizing:border-box;margin:0;padding:0;font-family:Inter,Segoe UI,system-ui,Arial,sans-serif}
    body{background:var(--bg);color:#222}
    .container{display:flex;min-height:100vh}
    /* Login */
    .login-container{width:100%;height:100vh;display:flex;align-items:center;justify-content:center;
      background:linear-gradient(135deg,var(--primary-blue),var(--secondary-blue));}
    .login-form{width:420px;background:var(--card);padding:20px;border-radius:10px;box-shadow:0 8px 28px rgba(0,0,0,.18)}
    .login-form h2{text-align:center;color:var(--primary-blue);margin-bottom:14px}
    .user-type-selector{display:flex;margin-bottom:10px;border-radius:8px;overflow:hidden;border:1px solid #eee}
    .user-type{flex:1;padding:8px;text-align:center;background:#f7f7f7;cursor:pointer}
    .user-type.active{background:var(--primary-blue);color:#fff}
    .form-group{margin:10px 0}
    input[type="text"],input[type="password"],input[type="number"],select,textarea{width:100%;padding:10px;border-radius:6px;border:1px solid #ddd}
    .btn{display:inline-block;padding:10px 14px;border-radius:6px;border:none;cursor:pointer;font-weight:600}
    .btn-primary{background:var(--primary-blue);color:#fff}
    .link{color:var(--primary-blue);cursor:pointer}
    .small{font-size:13px;color:#666;margin-top:8px}
    /* App */
    .sidebar{width:240px;background:linear-gradient(180deg,var(--primary-blue),var(--secondary-blue));color:#fff;position:fixed;left:0;top:0;bottom:0;padding-bottom:20px;overflow:auto}
    .logo{padding:20px;border-bottom:1px solid rgba(255,255,255,.06);text-align:center}
    .logo h2{font-size:18px;display:flex;align-items:center;gap:10px;justify-content:center}
    .nav-menu{padding-top:10px}
    .nav-item{padding:12px 18px;display:flex;gap:12px;align-items:center;cursor:pointer}
    .nav-item:hover,.nav-item.active{background:rgba(255,255,255,.06)}
    .main-content{margin-left:240px;padding:18px;flex:1}
    .header{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px}
    .card{background:var(--card);padding:14px;border-radius:10px;box-shadow:0 4px 14px rgba(0,0,0,.06)}
    .dashboard{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px;margin-bottom:12px}
    .stat-value{font-size:20px;font-weight:700;color:var(--primary-blue);margin-top:8px}
    .charts-container{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px}
    .table-container{overflow-x:auto}
    table{width:100%;border-collapse:collapse}
    th,td{padding:10px;border-bottom:1px solid #eee;text-align:left}
    .form-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:10px}
    .products-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:10px}
    .product-card{background:var(--card);padding:12px;border-radius:10px;text-align:center;cursor:pointer;border:1px solid #f0f0f0}
    .product-image{width:80px;height:80px;object-fit:contain;margin-bottom:8px}
    .cart{background:var(--card);padding:12px;border-radius:10px}
    .cart-item{display:flex;justify-content:space-between;padding:6px 0;border-bottom:1px dashed #eee}
    .actions{display:flex;gap:6px}
    .btn-sm{padding:6px 8px;border-radius:6px;font-size:13px}
    .btn-danger{background:var(--danger);color:#fff}
    .btn-success{background:var(--success);color:#fff}
    footer{margin-top:18px;text-align:center;color:#666;font-size:13px}
    .badge{display:inline-block;padding:4px 8px;border-radius:8px;font-size:12px}
    .badge-pending{background:#fff3cd;color:#856404}
    .badge-approved{background:#d4edda;color:#155724}
    .badge-cancel{background:#f8d7da;color:#721c24}
    .flex-row{display:flex;gap:8px;align-items:center}
    @media (max-width:900px){.charts-container{grid-template-columns:1fr}.main-content{padding:12px}.sidebar{width:70px}.main-content{margin-left:70px}}
  </style>
</head>
<body>
  <!-- LOGIN -->
  <div id="loginScreen" class="login-container">
    <div class="login-form card">
      <h2><i class="fas fa-cocktail"></i> Pepsi Beverage System</h2>

      <div class="user-type-selector" id="userTypeSelector">
        <div class="user-type active" data-type="admin">Admin</div>
        <div class="user-type" data-type="customer">Customer</div>
      </div>

      <div class="form-group">
        <label>Username</label>
        <input id="username" type="text" placeholder="username">
      </div>
      <div class="form-group">
        <label>Password</label>
        <input id="password" type="password" placeholder="password">
      </div>

      <div style="display:flex;gap:8px">
        <button id="loginBtn" class="btn btn-primary" style="flex:1">Login</button>
        <button id="openRegisterBtn" class="btn" style="flex:1">Register (customer)</button>
      </div>

      <div class="small" style="margin-top:10px">Demo admin: <b>admin / admin</b> — Admin creates customers.</div>
    </div>
  </div>

  <!-- MAIN APP -->
  <div id="mainApp" style="display:none" class="container">
    <aside class="sidebar">
      <div class="logo">
        <h2><i class="fas fa-bottle-water"></i> <span>Pepsi System</span></h2>
      </div>
      <nav class="nav-menu" id="navMenu">
        <!-- Admin menu will be shown/hidden depending on role -->
        <div class="nav-item active" data-page="dashboard"><i class="fas fa-tachometer-alt"></i><span class="nav-label">Dashboard</span></div>
        <div class="nav-item" data-page="products"><i class="fas fa-boxes-stacked"></i><span class="nav-label">Products</span></div>
        <div class="nav-item" data-page="customers"><i class="fas fa-users"></i><span class="nav-label">Customers</span></div>
        <div class="nav-item" data-page="orders"><i class="fas fa-shopping-cart"></i><span class="nav-label">Orders</span></div>
        <div class="nav-item" data-page="messages"><i class="fas fa-envelope"></i><span class="nav-label">Messages</span></div>
        <div class="nav-item" data-page="reports"><i class="fas fa-chart-bar"></i><span class="nav-label">Reports</span></div>

        <!-- Customer-only menu -->
        <div class="nav-item" data-page="shop"><i class="fas fa-store"></i><span class="nav-label">Shop</span></div>
        <div class="nav-item" data-page="myorders"><i class="fas fa-list"></i><span class="nav-label">My Orders</span></div>
        <div class="nav-item" data-page="mymessages"><i class="fas fa-envelope-open-text"></i><span class="nav-label">Messages</span></div>

        <div class="nav-item" id="logoutBtn"><i class="fas fa-sign-out-alt"></i><span class="nav-label">Logout</span></div>
      </nav>
    </aside>

    <main class="main-content">
      <div class="header">
        <div>
          <h3 id="pageTitle">Dashboard</h3>
          <div id="subTitle" style="color:#666;font-size:13px">Welcome</div>
        </div>
        <div style="display:flex;align-items:center;gap:10px">
          <div style="display:flex;align-items:center;gap:10px">
            <div style="width:40px;height:40px;border-radius:50%;background:var(--primary-blue);color:#fff;display:flex;align-items:center;justify-content:center;font-weight:700" id="avatar">A</div>
            <div style="text-align:right">
              <div id="accountName">Admin User</div>
              <small id="accountType" style="color:#666">Administrator</small>
            </div>
          </div>
        </div>
      </div>

      <!-- ADMIN DASHBOARD -->
      <section id="dashboardPage" class="page-content">
        <div class="dashboard">
          <div class="card">
            <div><strong>Pending Orders</strong></div>
            <div class="stat-value" id="statPending">0</div>
            <div class="small">Orders awaiting admin action</div>
          </div>
          <div class="card">
            <div><strong>Total Orders</strong></div>
            <div class="stat-value" id="statOrders">0</div>
            <div class="small">All orders in system</div>
          </div>
          <div class="card">
            <div><strong>Total Customers</strong></div>
            <div class="stat-value" id="statCustomers">0</div>
            <div class="small">Customers created</div>
          </div>
          <div class="card">
            <div><strong>Low Stock</strong></div>
            <div class="stat-value" id="statLowStock">0</div>
            <div class="small">Products with qty ≤ 5</div>
          </div>
        </div>

        <div class="charts-container">
          <div class="card">
            <h4>Sales Overview</h4>
            <canvas id="salesChart" style="height:220px"></canvas>
          </div>
          <div class="card">
            <h4>Top Products</h4>
            <canvas id="topProductsChart" style="height:220px"></canvas>
          </div>
        </div>
      </section>

      <!-- PRODUCTS (Admin) -->
      <section id="productsPage" class="page-content" style="display:none">
        <div class="card" style="margin-bottom:12px">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h4>Products</h4>
            <div>
              <button id="addProductBtn" class="btn btn-primary">Add Product</button>
            </div>
          </div>
          <div id="productsList" style="margin-top:10px"></div>
        </div>

        <div id="productFormCard" class="card" style="display:none">
          <h4 id="productFormTitle">Add Product</h4>
          <div class="form-row" style="margin-top:8px">
            <input id="prodName" placeholder="Product name">
            <input id="prodPrice" placeholder="Price" type="number" step="0.01">
            <input id="prodQty" placeholder="Quantity" type="number">
            <input id="prodImg" placeholder="Image URL (optional)">
          </div>
          <div style="margin-top:8px;display:flex;gap:8px">
            <button id="saveProductBtn" class="btn btn-primary">Save</button>
            <button id="cancelProductBtn" class="btn">Cancel</button>
          </div>
        </div>
      </section>

      <!-- CUSTOMERS (Admin) -->
      <section id="customersPage" class="page-content" style="display:none">
        <div class="card" style="margin-bottom:12px">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h4>Customers</h4>
            <button id="openCreateCustomerBtn" class="btn btn-primary">Create Customer</button>
          </div>
          <div id="customerList" style="margin-top:10px"></div>
        </div>

        <div id="customerFormCard" class="card" style="display:none">
          <h4 id="customerFormTitle">Create Customer</h4>
          <div class="form-row" style="margin-top:8px">
            <input id="custUser" placeholder="username (unique)">
            <input id="custPass" placeholder="password" type="password">
            <input id="custName" placeholder="Full name">
            <input id="custPhone" placeholder="Phone">
            <input id="custAddress" placeholder="Address">
            <input id="custEmail" placeholder="Email">
          </div>
          <div style="margin-top:8px;display:flex;gap:8px">
            <button id="saveCustomerBtn" class="btn btn-primary">Save</button>
            <button id="cancelCustomerBtn" class="btn">Cancel</button>
          </div>
        </div>
      </section>

      <!-- ORDERS (Admin) -->
      <section id="ordersPage" class="page-content" style="display:none">
        <div class="card table-container">
          <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
            <h4>Orders</h4>
            <button id="refreshOrdersBtn" class="btn">Refresh</button>
          </div>
          <table>
            <thead><tr><th>ID</th><th>Customer</th><th>Date</th><th>Items</th><th>Amount</th><th>Status</th><th>Action</th></tr></thead>
            <tbody id="ordersTbody"></tbody>
          </table>
        </div>
      </section>

      <!-- MESSAGES (Admin) -->
      <section id="messagesPage" class="page-content" style="display:none">
        <div class="card" style="margin-bottom:12px">
          <h4>Send Message to Customer</h4>
          <div class="form-row" style="margin-top:8px">
            <select id="msgCustomerSelect"></select>
            <input id="msgSubject" placeholder="Subject">
          </div>
          <div style="margin-top:8px">
            <textarea id="msgBody" placeholder="Message body" style="width:100%;min-height:100px;padding:10px;border-radius:6px;border:1px solid #ddd"></textarea>
          </div>
          <div style="margin-top:8px;display:flex;gap:8px">
            <button id="sendMsgBtn" class="btn btn-primary">Send Message</button>
            <button id="clearMsgsBtn" class="btn">Clear Messages</button>
          </div>
        </div>

        <div class="card table-container">
          <h4>All Messages Sent (latest first)</h4>
          <table>
            <thead><tr><th>To</th><th>Subject</th><th>Body</th><th>Date</th></tr></thead>
            <tbody id="allMessagesTbody"></tbody>
          </table>
        </div>
      </section>

      <!-- REPORTS (Admin) -->
      <section id="reportsPage" class="page-content" style="display:none">
        <div class="card" style="margin-bottom:12px">
          <h4>Reports</h4>
          <div class="small">Sales and top-products (admin only)</div>
        </div>

        <div class="charts-container">
          <div class="card">
            <h4>Sales by Date</h4>
            <canvas id="salesByDateChart" style="height:220px"></canvas>
          </div>
          <div class="card">
            <h4>Top Selling Products</h4>
            <canvas id="topSellingChart" style="height:220px"></canvas>
          </div>
        </div>
      </section>

      <!-- SHOP (Customer) -->
      <section id="shopPage" class="page-content" style="display:none">
        <div class="card" style="margin-bottom:12px">
          <h4>Shop</h4>
          <div id="productsGrid" class="products-grid" style="margin-top:8px"></div>
        </div>

        <div class="card">
          <h4>Quick Cart</h4>
          <div id="cartItems" style="min-height:60px"></div>
          <div style="display:flex;justify-content:space-between;align-items:center;margin-top:8px">
            <div><strong>Total:</strong></div>
            <div id="cartTotal">$0.00</div>
          </div>
          <div style="margin-top:8px;display:flex;gap:8px">
            <input id="orderNote" placeholder="Order note (optional)" style="flex:1;padding:8px;border:1px solid #ddd;border-radius:6px">
            <button id="checkoutBtn" class="btn btn-success">Place Order</button>
          </div>
          <div class="small" style="margin-top:8px">Orders placed are sent to Admin for approval.</div>
        </div>
      </section>

      <!-- MY ORDERS (Customer) -->
      <section id="myordersPage" class="page-content" style="display:none">
        <div class="card table-container">
          <h4>My Orders</h4>
          <table>
            <thead><tr><th>ID</th><th>Date</th><th>Items</th><th>Amount</th><th>Status</th><th>Admin Message</th></tr></thead>
            <tbody id="myOrdersTbody"></tbody>
          </table>
        </div>
      </section>

      <!-- MY MESSAGES (Customer) -->
      <section id="mymessagesPage" class="page-content" style="display:none">
        <div class="card table-container">
          <h4>Messages from Admin</h4>
          <table>
            <thead><tr><th>From</th><th>Subject</th><th>Message</th><th>Date</th></tr></thead>
            <tbody id="myMessagesTbody"></tbody>
          </table>
        </div>
      </section>

      <!-- SETTINGS -->
      <section id="settingsPage" class="page-content" style="display:none">
        <div class="card">
          <h4>Settings</h4>
          <div style="margin-top:8px">
            <button id="resetDataBtn" class="btn btn-danger">Reset All Demo Data (clear localStorage)</button>
            <div class="small" style="margin-top:8px">Use carefully.</div>
          </div>
        </div>
      </section>

      <footer>Pepsi Beverage Management System — Demo</footer>
    </main>
  </div>

<script>
/* -------------------------
   Local data model & init
   ------------------------- */
const LS_KEYS = {
  USERS:'pbs_users_v2',
  PRODUCTS:'pbs_products_v2',
  CUSTOMERS:'pbs_customers_v2',
  ORDERS:'pbs_orders_v2',
  MESSAGES:'pbs_messages_v2',
  SESSION:'pbs_session_v2'
};

const sampleProducts = [
  {id: 'P001', name:'Pepsi 330ml (Can)', price:35.00, qty:50, img:'https://i.imgur.com/5qH1R8D.png'},
  {id: 'P002', name:'Diet Pepsi 330ml', price:36.00, qty:30, img:'https://i.imgur.com/5qH1R8D.png'},
  {id: 'P003', name:'7Up 330ml', price:34.00, qty:25, img:'https://i.imgur.com/8pMZ0mB.png'},
  {id: 'P004', name:'Mirinda 330ml', price:34.00, qty:18, img:'https://i.imgur.com/8pMZ0mB.png'},
  {id: 'P005', name:'Mountain Dew 330ml', price:38.00, qty:12, img:'https://i.imgur.com/AqgF6Kq.png'},
];

const sampleCustomers = [
  // customers as account entries will be in USERS with type 'customer'
];

function load(key, fallback){
  try{ const v = localStorage.getItem(key); return v ? JSON.parse(v) : fallback; }catch(e){return fallback;}
}
function save(key, val){ localStorage.setItem(key, JSON.stringify(val)); }

function ensureInitialData(){
  if(!load(LS_KEYS.USERS, null)){
    const users = [
      {username:'admin', password:'admin', type:'admin', name:'Admin User', id:'UADMIN'},
      {username:'customer', password:'cust', type:'customer', name:'Demo Customer', id:'UC001', phone:'03210000002', address:'Demo', email:'cust@example.com'}
    ];
    save(LS_KEYS.USERS, users);
  }
  if(!load(LS_KEYS.PRODUCTS, null)){
    save(LS_KEYS.PRODUCTS, sampleProducts);
  }
  if(!load(LS_KEYS.CUSTOMERS, null)){
    // customers list is derived from USERS of type 'customer' but keep a separate customer profile store for easy fields:
    save(LS_KEYS.CUSTOMERS, []);
  }
  if(!load(LS_KEYS.ORDERS, null)){
    save(LS_KEYS.ORDERS, []);
  }
  if(!load(LS_KEYS.MESSAGES, null)){
    save(LS_KEYS.MESSAGES, []);
  }
}
ensureInitialData();

/* -------------------------
   Utilities
   ------------------------- */
function q(sel, root=document) { return root.querySelector(sel); }
function qa(sel, root=document) { return Array.from(root.querySelectorAll(sel)); }
function uid(prefix='ID'){ return prefix + Math.random().toString(36).slice(2,9).toUpperCase(); }
function formatMoney(n){ return '$' + Number(n).toFixed(2); }
function todayStr(){ return new Date().toISOString().slice(0,10); }

/* -------------------------
   Session / Auth
   ------------------------- */
function setSession(user){ save(LS_KEYS.SESSION, user); }
function getSession(){ return load(LS_KEYS.SESSION, null); }
function clearSession(){ localStorage.removeItem(LS_KEYS.SESSION); }

/* -------------------------
   UI helpers & role gating
   ------------------------- */
const allPages = ['dashboard','products','customers','orders','messages','reports','shop','myorders','mymessages','settings'];
function showPage(page){
  // hide all pages
  allPages.forEach(p => {
    const el = q(`#${p}Page`);
    if(el) el.style.display = 'none';
  });
  // show requested
  const el = q(`#${page}Page`);
  if(el) el.style.display = '';
  // nav active
  qa('#navMenu .nav-item').forEach(n => n.classList.toggle('active', n.dataset.p
