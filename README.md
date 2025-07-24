# EZBooks-
Accounting/Bookkeeping App
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="referrer" content="no-referrer">
    <title>EZbooks - Simplified Accounting Software</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdn.jsdelivr.net/npm/daisyui@4.4.19/dist/full.min.css" rel="stylesheet" type="text/css" />
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .gradient-bg { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
        .card-hover { transition: transform 0.2s; }
        .card-hover:hover { transform: translateY(-2px); }
        .sidebar-transition { transition: transform 0.3s ease-in-out; }
        @media (max-width: 768px) {
            #sidebar { transform: translateX(-100%); }
            #sidebar.open { transform: translateX(0); }
        }
        #alertModal { transition: opacity 0.3s ease-in-out; }
    </style>
</head>
<body class="bg-gray-50 font-sans">
    <!-- Loading Screen -->
    <div id="loadingScreen" class="fixed inset-0 bg-white z-50 flex items-center justify-center">
        <div class="text-center">
            <div class="loading loading-spinner loading-lg text-primary"></div>
            <p class="mt-4 text-lg font-semibold">Loading EZbooks...</p>
        </div>
    </div>

    <!-- Login/Register Screen -->
    <div id="authScreen" class="min-h-screen gradient-bg flex items-center justify-center p-4 hidden">
        <div class="card w-full max-w-md bg-white shadow-2xl">
            <div class="card-body">
                <div class="text-center mb-6"><h1 class="text-3xl font-bold text-primary">EZbooks</h1><p class="text-gray-600">Simplified Accounting Software</p></div>
                <div class="tabs tabs-boxed mb-4"><a class="tab tab-active" id="loginTab">Login</a><a class="tab" id="registerTab">Register</a></div>
                <form id="loginForm" class="space-y-4">
                    <div class="form-control"><label class="label"><span class="label-text">Email</span></label><input type="email" name="email" class="input input-bordered" required></div>
                    <div class="form-control"><label class="label"><span class="label-text">Password</span></label><input type="password" name="password" class="input input-bordered" required></div>
                    <button type="submit" class="btn btn-primary w-full">Login</button>
                </form>
                <form id="registerForm" class="space-y-4 hidden">
                    <div class="form-control"><label class="label"><span class="label-text">Email</span></label><input type="email" name="email" class="input input-bordered" required></div>
                    <div class="form-control"><label class="label"><span class="label-text">Password</span></label><input type="password" name="password" class="input input-bordered" required></div>
                    <button type="submit" class="btn btn-primary w-full">Create Account</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Subscription Selection Screen -->
    <div id="subscriptionScreen" class="min-h-screen bg-gray-50 p-8 hidden">
        <div class="max-w-6xl mx-auto">
            <div class="text-center mb-8">
                <h2 class="text-4xl font-bold mb-4">Choose Your Plan</h2>
                <p class="text-xl text-gray-600">Start with our free tier or unlock more features</p>
            </div>
            <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
                <div class="card bg-white shadow-lg card-hover"><div class="card-body text-center"><h3 class="text-2xl font-bold">Free</h3><div class="text-4xl font-bold text-primary my-4">$0<span class="text-lg">/mo</span></div><ul class="text-left space-y-2 mb-6"><li>✓ 5 Invoices/month</li><li>✓ Basic expense tracking</li><li>✓ 1 User</li><li>✓ Email support</li></ul><button class="btn btn-outline btn-primary w-full" onclick="selectPlan('free')">Get Started</button></div></div>
                <div class="card bg-white shadow-lg card-hover border-2 border-primary"><div class="card-body text-center"><div class="badge badge-primary mb-2">Most Popular</div><h3 class="text-2xl font-bold">Basic</h3><div class="text-4xl font-bold text-primary my-4">$15<span class="text-lg">/mo</span></div><ul class="text-left space-y-2 mb-6"><li>✓ Unlimited invoices</li><li>✓ Advanced expense tracking</li><li>✓ 3 Users</li><li>✓ Basic reports</li><li>✓ Priority support</li></ul><button class="btn btn-primary w-full" onclick="selectPlan('basic')">Choose Basic</button></div></div>
                <div class="card bg-white shadow-lg card-hover"><div class="card-body text-center"><h3 class="text-2xl font-bold">Pro</h3><div class="text-4xl font-bold text-primary my-4">$29<span class="text-lg">/mo</span></div><ul class="text-left space-y-2 mb-6"><li>✓ Everything in Basic</li><li>✓ AI Assistant</li><li>✓ 10 Users</li><li>✓ Advanced reports</li><li>✓ API access</li></ul><button class="btn btn-outline btn-primary w-full" onclick="selectPlan('pro')">Choose Pro</button></div></div>
                <div class="card bg-white shadow-lg card-hover"><div class="card-body text-center"><h3 class="text-2xl font-bold">Enterprise</h3><div class="text-4xl font-bold text-primary my-4">$49<span class="text-lg">/mo</span></div><ul class="text-left space-y-2 mb-6"><li>✓ Everything in Pro</li><li>✓ Unlimited users</li><li>✓ Custom integrations</li><li>✓ Dedicated support</li><li>✓ White-label options</li></ul><button class="btn btn-outline btn-primary w-full" onclick="selectPlan('enterprise')">Choose Enterprise</button></div></div>
            </div>
        </div>
    </div>

    <!-- Main Application -->
    <div id="mainApp" class="min-h-screen bg-gray-50 hidden">
        <div class="navbar bg-white shadow-sm border-b sticky top-0 z-30"><div class="navbar-start"><button class="btn btn-ghost lg:hidden" onclick="toggleSidebar()"><i class="fas fa-bars"></i></button><div class="text-xl font-bold text-primary">EZbooks</div></div><div class="navbar-center hidden lg:flex"><div class="breadcrumbs text-sm"><ul id="breadcrumb"><li>Dashboard</li></ul></div></div><div class="navbar-end"><div class="dropdown dropdown-end"><div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar"><div class="w-10 rounded-full bg-primary text-white flex items-center justify-center"><i class="fas fa-user"></i></div></div><ul tabindex="0" class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52"><li><a onclick="showSection('settings')">Settings</a></li><li><a onclick="logout()">Logout</a></li></ul></div></div></div>
        <div class="flex">
            <aside id="sidebar" class="w-64 bg-white shadow-lg h-screen sticky top-[65px] sidebar-transition lg:translate-x-0 z-20"><div class="p-4"><nav class="space-y-2"><a class="btn btn-ghost w-full justify-start" onclick="showSection('dashboard')"><i class="fas fa-tachometer-alt mr-2"></i> Dashboard</a><a class="btn btn-ghost w-full justify-start" onclick="showSection('invoices')"><i class="fas fa-file-invoice mr-2"></i> Invoices</a><a class="btn btn-ghost w-full justify-start" onclick="showSection('expenses')"><i class="fas fa-receipt mr-2"></i> Expenses</a><a class="btn btn-ghost w-full justify-start" onclick="showSection('purchaseOrders')"><i class="fas fa-shopping-cart mr-2"></i> Purchase Orders</a><a class="btn btn-ghost w-full justify-start" onclick="showSection('clients')"><i class="fas fa-users mr-2"></i> Clients</a><a class="btn btn-ghost w-full justify-start" onclick="showSection('reports')"><i class="fas fa-chart-bar mr-2"></i> Reports</a><a class="btn btn-ghost w-full justify-start" onclick="showSection('ai-assistant')" id="aiAssistantBtn"><i class="fas fa-robot mr-2"></i> AI Assistant</a></nav></div></aside>
            <main class="flex-1 p-6">
                <div id="dashboardSection" class="section">
                    <div class="mb-6"><h2 class="text-3xl font-bold mb-2">Dashboard</h2><p class="text-gray-600">Welcome back! Here's your real-time business overview.</p></div>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8"><div class="stat bg-white rounded-lg shadow"><div class="stat-figure text-primary"><i class="fas fa-dollar-sign text-2xl"></i></div><div class="stat-title">Total Revenue</div><div class="stat-value text-primary" id="totalRevenue">$0</div></div><div class="stat bg-white rounded-lg shadow"><div class="stat-figure text-secondary"><i class="fas fa-file-invoice text-2xl"></i></div><div class="stat-title">Invoices Sent</div><div class="stat-value text-secondary" id="invoicesSent">0</div></div><div class="stat bg-white rounded-lg shadow"><div class="stat-figure text-accent"><i class="fas fa-receipt text-2xl"></i></div><div class="stat-title">Expenses</div><div class="stat-value text-accent" id="totalExpenses">$0</div></div><div class="stat bg-white rounded-lg shadow"><div class="stat-figure text-info"><i class="fas fa-users text-2xl"></i></div><div class="stat-title">Active Clients</div><div class="stat-value text-info" id="activeClients">0</div></div></div>
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6"><div class="card bg-white shadow"><div class="card-body"><h3 class="card-title">Revenue Trend (Last 6 Months)</h3><canvas id="revenueChart"></canvas></div></div><div class="card bg-white shadow"><div class="card-body"><h3 class="card-title">Expense Categories</h3><canvas id="expenseChart"></canvas></div></div></div>
                </div>
                <div id="invoicesSection" class="section hidden"><div class="flex justify-between items-center mb-6"><div><h2 class="text-3xl font-bold mb-2">Invoices</h2><p class="text-gray-600">Manage your invoices and track payments</p></div><button class="btn btn-primary" onclick="showCreateInvoice()"><i class="fas fa-plus mr-2"></i> New Invoice</button></div><div class="card bg-white shadow"><div class="card-body"><div class="overflow-x-auto"><table class="table table-zebra"><thead><tr><th>Invoice #</th><th>Client</th><th>Amount</th><th>Status</th><th>Due Date</th><th>Actions</th></tr></thead><tbody id="invoiceTableBody"></tbody></table></div></div></div></div>
                <div id="expensesSection" class="section hidden"><div class="flex justify-between items-center mb-6"><div><h2 class="text-3xl font-bold mb-2">Expenses</h2><p class="text-gray-600">Track and categorize your business expenses</p></div><button class="btn btn-primary" onclick="showCreateExpense()"><i class="fas fa-plus mr-2"></i> Add Expense</button></div><div class="card bg-white shadow"><div class="card-body"><div class="overflow-x-auto"><table class="table table-zebra"><thead><tr><th>Date</th><th>Description</th><th>Category</th><th>Amount</th><th>Actions</th></tr></thead><tbody id="expenseTableBody"></tbody></table></div></div></div></div>
                <div id="purchaseOrdersSection" class="section hidden"><div class="flex justify-between items-center mb-6"><div><h2 class="text-3xl font-bold mb-2">Purchase Orders</h2><p class="text-gray-600">Create and send purchase orders to vendors</p></div><button class="btn btn-primary" onclick="showCreatePurchaseOrder()"><i class="fas fa-plus mr-2"></i> New PO</button></div><div class="card bg-white shadow"><div class="card-body"><div class="overflow-x-auto"><table class="table table-zebra"><thead><tr><th>PO Number</th><th>Vendor</th><th>Total</th><th>Date</th><th>Actions</th></tr></thead><tbody id="purchaseOrderTableBody"></tbody></table></div></div></div></div>
                <div id="clientsSection" class="section hidden"><div class="flex justify-between items-center mb-6"><div><h2 class="text-3xl font-bold mb-2">Clients</h2><p class="text-gray-600">Manage your client relationships</p></div><button class="btn btn-primary" onclick="showCreateClient()"><i class="fas fa-plus mr-2"></i> Add Client</button></div><div id="clientGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"></div></div>
                <div id="reportsSection" class="section hidden"><div class="mb-6"><h2 class="text-3xl font-bold mb-2">Reports</h2><p class="text-gray-600">Live financial insights and analytics</p></div><div class="grid grid-cols-1 lg:grid-cols-2 gap-6"><div class="card bg-white shadow"><div class="card-body"><h3 class="card-title">Profit & Loss</h3><div class="space-y-2"><div class="flex justify-between"><span>Total Revenue</span><span id="pnlRevenue" class="font-bold text-green-600">$0</span></div><div class="flex justify-between"><span>Total Expenses</span><span id="pnlExpenses" class="font-bold text-red-600">$0</span></div><div class="divider"></div><div class="flex justify-between text-lg font-bold"><span>Net Profit</span><span id="pnlNetProfit" class="text-primary">$0</span></div></div></div></div></div></div>
                <div id="aiAssistantSection" class="section hidden"><div class="mb-6"><h2 class="text-3xl font-bold mb-2">AI Assistant</h2><p class="text-gray-600">Ask questions about your financial data.</p></div><div class="card bg-white shadow h-96"><div class="card-body flex flex-col"><div id="chatMessages" class="flex-1 overflow-y-auto space-y-4 p-4 mb-4 bg-gray-50 rounded-lg"></div><div class="flex gap-2"><input type="text" id="chatInput" class="input input-bordered flex-1" placeholder="e.g., What was my biggest expense last month?"><button class="btn btn-primary" onclick="sendMessage()"><i class="fas fa-paper-plane"></i></button></div></div></div></div>
                <div id="settingsSection" class="section hidden"><div class="mb-6"><h2 class="text-3xl font-bold mb-2">Settings</h2><p class="text-gray-600">Manage your account and preferences</p></div><div class="grid grid-cols-1 lg:grid-cols-2 gap-6"><div class="card bg-white shadow"><div class="card-body"><h3 class="card-title">Account Settings</h3><div class="form-control"><label class="label"><span class="label-text">Email</span></label><input type="email" id="settingsUserEmail" class="input input-bordered" readonly></div></div></div><div class="card bg-white shadow"><div class="card-body"><h3 class="card-title">Subscription</h3><div class="space-y-4"><div class="alert alert-info"><i class="fas fa-info-circle"></i><span>Current Plan: <strong id="currentPlan"></strong></span></div><button class="btn btn-outline" onclick="showSubscriptionScreen()">Change Plan</button></div></div></div></div></div>
            </main>
        </div>
    </div>

    <!-- Modals -->
    <div id="createClientModal" class="modal"><div class="modal-box"><h3 class="font-bold text-lg mb-4">Add New Client</h3><form id="clientForm" class="space-y-4"><div class="form-control"><label class="label"><span class="label-text">Company Name</span></label><input type="text" name="company" class="input input-bordered" required></div><div class="form-control"><label class="label"><span class="label-text">Contact Person</span></label><input type="text" name="contact" class="input input-bordered"></div><div class="form-control"><label class="label"><span class="label-text">Email</span></label><input type="email" name="email" class="input input-bordered"></div><div class="modal-action"><button type="button" class="btn" onclick="closeCreateClient()">Cancel</button><button type="submit" class="btn btn-primary">Add Client</button></div></form></div></div>
    <div id="editClientModal" class="modal"><div class="modal-box"><h3 class="font-bold text-lg mb-4">Edit Client</h3><form id="editClientForm" class="space-y-4"><input type="hidden" id="editClientId"><div class="form-control"><label class="label"><span class="label-text">Company Name</span></label><input type="text" name="company" class="input input-bordered" required></div><div class="form-control"><label class="label"><span class="label-text">Contact Person</span></label><input type="text" name="contact" class="input input-bordered"></div><div class="form-control"><label class="label"><span class="label-text">Email</span></label><input type="email" name="email" class="input input-bordered"></div><div class="modal-action"><button type="button" class="btn" onclick="closeEditClient()">Cancel</button><button type="submit" class="btn btn-primary">Save Changes</button></div></form></div></div>
    <div id="createInvoiceModal" class="modal"><div class="modal-box w-11/12 max-w-2xl"><h3 class="font-bold text-lg mb-4">Create New Invoice</h3><form id="invoiceForm" class="space-y-4"><div class="grid grid-cols-1 md:grid-cols-2 gap-4"><div class="form-control"><label class="label"><span class="label-text">Client</span></label><select name="clientId" class="select select-bordered" required></select></div><div class="form-control"><label class="label"><span class="label-text">Due Date</span></label><input type="date" name="dueDate" class="input input-bordered" required></div></div><div class="form-control"><label class="label"><span class="label-text">Amount</span></label><input type="number" name="amount" step="0.01" class="input input-bordered" placeholder="0.00" required></div><div class="form-control"><label class="label"><span class="label-text">Status</span></label><select name="status" class="select select-bordered" required><option value="pending">Pending</option><option value="paid">Paid</option></select></div><div class="modal-action"><button type="button" class="btn" onclick="closeCreateInvoice()">Cancel</button><button type="submit" class="btn btn-primary">Create Invoice</button></div></form></div></div>
    <div id="editInvoiceModal" class="modal"><div class="modal-box w-11/12 max-w-2xl"><h3 class="font-bold text-lg mb-4">Edit Invoice</h3><form id="editInvoiceForm" class="space-y-4"><input type="hidden" id="editInvoiceId"><div class="grid grid-cols-1 md:grid-cols-2 gap-4"><div class="form-control"><label class="label"><span class="label-text">Client</span></label><select name="clientId" class="select select-bordered" required></select></div><div class="form-control"><label class="label"><span class="label-text">Due Date</span></label><input type="date" name="dueDate" class="input input-bordered" required></div></div><div class="form-control"><label class="label"><span class="label-text">Amount</span></label><input type="number" name="amount" step="0.01" class="input input-bordered" placeholder="0.00" required></div><div class="form-control"><label class="label"><span class="label-text">Status</span></label><select name="status" class="select select-bordered" required><option value="pending">Pending</option><option value="paid">Paid</option></select></div><div class="modal-action"><button type="button" class="btn" onclick="closeEditInvoice()">Cancel</button><button type="submit" class="btn btn-primary">Save Changes</button></div></form></div></div>
    <div id="createPurchaseOrderModal" class="modal"><div class="modal-box"><h3 class="font-bold text-lg mb-4">Create Purchase Order</h3><form id="purchaseOrderForm" class="space-y-4"><div class="form-control"><label class="label"><span class="label-text">Vendor</span></label><input type="text" placeholder="Vendor Name" class="input input-bordered" required></div><div class="form-control"><label class="label"><span class="label-text">Item Description</span></label><input type="text" placeholder="e.g., Office Chairs" class="input input-bordered" required></div><div class="form-control"><label class="label"><span class="label-text">Quantity</span></label><input type="number" value="1" min="1" class="input input-bordered" required></div><div class="form-control"><label class="label"><span class="label-text">Unit Price</span></label><input type="number" step="0.01" placeholder="0.00" class="input input-bordered" required></div><div class="modal-action"><button type="button" class="btn" onclick="closeCreatePurchaseOrder()">Cancel</button><button type="submit" class="btn btn-primary">Create PO</button></div></form></div></div>
    <div id="purchaseOrderTextModal" class="modal"><div class="modal-box w-11/12 max-w-3xl"><h3 class="font-bold text-lg" id="poTextTitle">Purchase Order</h3><textarea id="poTextArea" class="textarea textarea-bordered w-full h-64 mt-4 font-mono" readonly></textarea><div class="modal-action"><button class="btn" onclick="closePurchaseOrderTextModal()">Close</button><button id="copyPoButton" class="btn btn-primary" onclick="copyPOText()">Copy to Clipboard</button></div></div></div>
    <div id="alertModal" class="modal"><div class="modal-box"><h3 id="alertTitle" class="font-bold text-lg"></h3><p id="alertMessage" class="py-4"></p><div class="modal-action"><button id="alertCancel" class="btn hidden">Cancel</button><button id="alertConfirm" class="btn btn-primary">OK</button></div></div></div>

    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, addDoc, updateDoc, onSnapshot, collection, query, deleteDoc, serverTimestamp, runTransaction } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        let app, auth, db, userId, currentPlan = 'free';
        let invoicesUnsubscribe, expensesUnsubscribe, clientsUnsubscribe, settingsUnsubscribe, purchaseOrdersUnsubscribe;
        let invoices = [], expenses = [], clients = [], purchaseOrders = [];
        let revenueChartInstance, expenseChartInstance;

        document.addEventListener('DOMContentLoaded', () => {
            try {
                app = initializeApp(firebaseConfig);
                auth = getAuth(app);
                db = getFirestore(app);
                setupAuthPage();
                setupAuthListeners();
            } catch (e) { console.error("Firebase init failed:", e); }
        });

        function setupAuthPage() {
            document.getElementById('loginTab').addEventListener('click', () => toggleAuthForm(true));
            document.getElementById('registerTab').addEventListener('click', () => toggleAuthForm(false));
            document.getElementById('loginForm').addEventListener('submit', handleLogin);
            document.getElementById('registerForm').addEventListener('submit', handleRegister);
        }

        function setupMainAppEventListeners() {
            document.getElementById('clientForm').addEventListener('submit', handleCreateClient);
            document.getElementById('editClientForm').addEventListener('submit', handleUpdateClient);
            document.getElementById('invoiceForm').addEventListener('submit', handleCreateInvoice);
            document.getElementById('editInvoiceForm').addEventListener('submit', handleUpdateInvoice);
            document.getElementById('purchaseOrderForm').addEventListener('submit', handleCreatePurchaseOrder);
            document.getElementById('chatInput').addEventListener('keypress', e => { if (e.key === 'Enter') sendMessage(); });
        }
        
        function setupAuthListeners() {
            onAuthStateChanged(auth, async user => {
                const loadingScreen = document.getElementById('loadingScreen');
                if (user) {
                    userId = user.uid;
                    document.getElementById('settingsUserEmail').value = user.email;
                    
                    const userSettingsRef = doc(db, `artifacts/${appId}/users/${userId}/settings/main`);
                    const userSettingsDoc = await getDoc(userSettingsRef);

                    if (userSettingsDoc.exists()) {
                        currentPlan = userSettingsDoc.data().plan || 'free';
                        initializeAppUI();
                    } else {
                        showSubscriptionScreen();
                    }
                } else {
                    userId = null;
                    if (invoicesUnsubscribe) cleanupListeners();
                    document.getElementById('authScreen').classList.remove('hidden');
                    document.getElementById('mainApp').classList.add('hidden');
                    loadingScreen.style.display = 'none';
                }
            });
        }

        function initializeAppUI() {
            document.getElementById('authScreen').classList.add('hidden');
            document.getElementById('subscriptionScreen').classList.add('hidden');
            document.getElementById('mainApp').classList.remove('hidden');
            document.getElementById('loadingScreen').style.display = 'none';
            setupMainAppEventListeners();
            setupRealtimeListeners();
            updatePlanFeatures();
        }

        const handleAuthAction = async (action, form, button) => {
            const originalButtonText = button.innerHTML;
            button.disabled = true;
            button.innerHTML = `<span class="loading loading-spinner"></span>`;
            try {
                await action(auth, form.email.value, form.password.value);
            } catch (err) {
                showAlert('Authentication Failed', err.message);
                button.disabled = false;
                button.innerHTML = originalButtonText;
            }
        };

        async function handleLogin(e) { e.preventDefault(); await handleAuthAction(signInWithEmailAndPassword, e.target, e.target.querySelector('button')); }
        async function handleRegister(e) { e.preventDefault(); await handleAuthAction(createUserWithEmailAndPassword, e.target, e.target.querySelector('button')); }
        window.logout = async () => await signOut(auth);
        
        function toggleAuthForm(isLogin) {
            document.getElementById('loginTab').classList.toggle('tab-active', isLogin);
            document.getElementById('registerTab').classList.toggle('tab-active', !isLogin);
            document.getElementById('loginForm').classList.toggle('hidden', !isLogin);
            document.getElementById('registerForm').classList.toggle('hidden', isLogin);
        }

        function showSubscriptionScreen() {
            document.getElementById('authScreen').classList.add('hidden');
            document.getElementById('subscriptionScreen').classList.remove('hidden');
            document.getElementById('loadingScreen').style.display = 'none';
        }

        window.selectPlan = async function(plan) {
            currentPlan = plan;
            const userSettingsRef = doc(db, `artifacts/${appId}/users/${userId}/settings/main`);
            await setDoc(userSettingsRef, { plan: plan }, { merge: true });
            initializeAppUI();
        }

        function setupRealtimeListeners() {
            cleanupListeners();
            const basePath = `artifacts/${appId}/users/${userId}`;
            const updateAndRender = (snapshot, targetArray, renderFunc) => {
                targetArray.splice(0, targetArray.length, ...snapshot.docs.map(d => ({ id: d.id, ...d.data() })));
                renderFunc();
                updateDashboard();
            };
            invoicesUnsubscribe = onSnapshot(query(collection(db, `${basePath}/invoices`)), s => updateAndRender(s, invoices, loadInvoices));
            expensesUnsubscribe = onSnapshot(query(collection(db, `${basePath}/expenses`)), s => updateAndRender(s, expenses, loadExpenses));
            clientsUnsubscribe = onSnapshot(query(collection(db, `${basePath}/clients`)), s => updateAndRender(s, clients, loadClients));
            purchaseOrdersUnsubscribe = onSnapshot(query(collection(db, `${basePath}/purchaseOrders`)), s => updateAndRender(s, purchaseOrders, loadPurchaseOrders));
        }
        function cleanupListeners() { [invoicesUnsubscribe, expensesUnsubscribe, clientsUnsubscribe, purchaseOrdersUnsubscribe, settingsUnsubscribe].forEach(unsub => unsub && unsub()); }

        const getClientDataFromForm = form => ({ company: form.company.value, contact: form.contact.value, email: form.email.value });
        const getInvoiceDataFromForm = form => ({ clientId: form.clientId.value, clientName: form.clientId.options[form.clientId.selectedIndex].text, dueDate: form.dueDate.value, amount: parseFloat(form.amount.value), status: form.status.value });

        async function handleCreateClient(e) { e.preventDefault(); await addDoc(collection(db, `artifacts/${appId}/users/${userId}/clients`), getClientDataFromForm(e.target)); e.target.reset(); closeCreateClient(); }
        async function handleUpdateClient(e) { e.preventDefault(); const form = e.target; await updateDoc(doc(db, `artifacts/${appId}/users/${userId}/clients`, form.editClientId.value), getClientDataFromForm(form)); closeEditClient(); }
        window.showEditClient = (id) => { const client = clients.find(c => c.id === id); if (!client) return; const form = document.getElementById('editClientForm'); form.editClientId.value = client.id; form.company.value = client.company; form.contact.value = client.contact; form.email.value = client.email; document.getElementById('editClientModal').classList.add('modal-open'); };
        
        async function handleCreateInvoice(e) { e.preventDefault(); await addDoc(collection(db, `artifacts/${appId}/users/${userId}/invoices`), { ...getInvoiceDataFromForm(e.target), createdAt: serverTimestamp() }); e.target.reset(); closeCreateInvoice(); }
        async function handleUpdateInvoice(e) { e.preventDefault(); const form = e.target; await updateDoc(doc(db, `artifacts/${appId}/users/${userId}/invoices`, form.editInvoiceId.value), getInvoiceDataFromForm(form)); closeEditInvoice(); }
        window.showEditInvoice = (id) => { const invoice = invoices.find(i => i.id === id); if (!invoice) return; const form = document.getElementById('editInvoiceForm'); populateClientDropdown(form.clientId, invoice.clientId); form.editInvoiceId.value = invoice.id; form.dueDate.value = invoice.dueDate; form.amount.value = invoice.amount; form.status.value = invoice.status; document.getElementById('editInvoiceModal').classList.add('modal-open'); };
        
        async function handleCreatePurchaseOrder(e) { /* ... */ }
        
        function loadClients() {
            const grid = document.getElementById('clientGrid');
            grid.innerHTML = clients.length ? clients.map(c => `<div class="card bg-white shadow card-hover"><div class="card-body"><h3 class="card-title">${c.company}</h3><p>${c.contact}</p><p class="text-sm">${c.email}</p><div class="card-actions justify-end"><button class="btn btn-sm btn-outline" onclick="showEditClient('${c.id}')">Edit</button><button class="btn btn-sm btn-error" onclick="deleteClient('${c.id}')">Delete</button></div></div></div>`).join('') : '<p class="col-span-full text-center text-gray-500">No clients found. Add one!</p>';
            populateClientDropdown(document.querySelector('#invoiceForm select[name="clientId"]'));
            populateClientDropdown(document.querySelector('#editInvoiceForm select[name="clientId"]'));
        }
        function populateClientDropdown(selectElement, selectedId) {
            if (!selectElement) return;
            selectElement.innerHTML = '<option value="">Select Client</option>' + clients.map(c => `<option value="${c.id}" ${c.id === selectedId ? 'selected' : ''}>${c.company}</option>`).join('');
        }
        function loadInvoices() { /* ... */ }
        function loadPurchaseOrders() { /* ... */ }
        
        function updateDashboard() { /* ... */ }
        function updateCharts() { /* ... */ }

        async function sendMessage() { /* ... */ }
        function addChatMessage(message, role, isThinking = false) { /* ... */ }
        
        function showAlert(title, message) {
            document.getElementById('alertTitle').textContent = title;
            document.getElementById('alertMessage').textContent = message;
            document.getElementById('alertModal').classList.add('modal-open');
        }
        document.getElementById('alertConfirm').addEventListener('click', () => document.getElementById('alertModal').classList.remove('modal-open'));

        window.showSection = (section) => {
            document.querySelectorAll('.section').forEach(s => s.classList.add('hidden'));
            document.getElementById(section + 'Section').classList.remove('hidden');
            document.getElementById('breadcrumb').innerHTML = `<li>${section.charAt(0).toUpperCase() + section.slice(1)}</li>`;
        };
        window.toggleSidebar = () => document.getElementById('sidebar').classList.toggle('open');
        
        window.closeCreateClient = () => document.getElementById('createClientModal').classList.remove('modal-open');
        window.closeEditClient = () => document.getElementById('editClientModal').classList.remove('modal-open');
        window.closeCreateInvoice = () => document.getElementById('createInvoiceModal').classList.remove('modal-open');
        window.closeEditInvoice = () => document.getElementById('editInvoiceModal').classList.remove('modal-open');
        // ... other close functions
    </script>
</body>
</html>

