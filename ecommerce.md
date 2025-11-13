<!--
E-Commerce Portal - Single-file project (index.html)

Instructions:
1. Save this content as `index.html` in a folder named `ecommerce-portal`.
2. To turn into a GitHub repo:
   - git init
   - git add index.html
   - git commit -m "Initial commit: E-Commerce portal single-file project"
   - Create a repo on GitHub and follow the instructions to push (or: git remote add origin <URL>; git push -u origin main)
3. This single-file example uses localStorage for simple persistence. For a real app, split into components and use a backend.

Files contained below (all in one file for easy submission):
 - index.html (HTML + CSS + JS)

If you want me to split this into separate files (index.html, styles.css, app.js) or create a full GitHub-ready tree with README, LICENSE and .gitignore, tell me and I'll generate those.
-->

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>E-Commerce Portal - Assignment 3</title>
  <style>
    /* Basic reset */
    *{box-sizing:border-box;margin:0;padding:0;font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial}
    body{background:#f4f6fb;color:#111}
    header{background:#0b63d6;color:#fff;padding:12px 20px;display:flex;align-items:center;justify-content:space-between}
    header h1{font-size:18px}
    nav ul{list-style:none;display:flex;gap:12px}
    nav a{color:#fff;text-decoration:none;padding:8px 10px;border-radius:6px}
    nav a:hover{background:rgba(255,255,255,0.12)}

    .container{max-width:1100px;margin:20px auto;padding:0 16px}
    .grid{display:grid;grid-template-columns:1fr 320px;gap:18px}
    .card{background:#fff;padding:14px;border-radius:10px;box-shadow:0 6px 18px rgba(12,34,60,0.06)}

    /* Products */
    .products{display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));gap:14px}
    .product{border:1px solid #eef2fb;border-radius:8px;padding:8px;text-align:center}
    .product button{margin-top:8px;padding:8px 10px;border:0;border-radius:8px;cursor:pointer}

    /* Sections */
    .section-title{font-weight:700;margin-bottom:8px}
    .list{display:flex;flex-direction:column;gap:8px}
    .list-item{display:flex;justify-content:space-between;align-items:center;padding:8px;border-radius:6px;background:#fbfdff;border:1px solid #f0f4ff}

    footer{max-width:1100px;margin:20px auto;padding:20px;text-align:center;color:#666}

    /* Forms */
    form{display:flex;flex-direction:column;gap:8px}
    label{font-size:13px}
    input,textarea,select{padding:8px;border-radius:6px;border:1px solid #cfd8ee}
    .row{display:flex;gap:8px}
    .row .col{flex:1}
    .error{color:#b00020;font-size:12px}

    /* Modal */
    .modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(2,6,23,0.45)}
    .modal.open{display:flex}
    .modal .card{width:420px}

    .btn{background:#0b63d6;color:#fff;padding:8px 12px;border-radius:8px;border:0;cursor:pointer}
    .btn.ghost{background:transparent;color:#0b63d6;border:1px solid #cfe0ff}

    @media (max-width:900px){.grid{grid-template-columns:1fr}.container{padding:0 10px}}
  </style>
</head>
<body>
  <header>
    <h1>E-Shop Demo</h1>
    <nav>
      <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#products">Products</a></li>
        <li><a href="#contact">Contact Us</a></li>
        <li><a href="#about">About Us</a></li>
        <li><button id="open-login" class="btn">Login / Register</button></li>
      </ul>
    </nav>
  </header>

  <main class="container">
    <section id="home" class="card">
      <h2 class="section-title">Welcome to E-Shop Demo</h2>
      <p>This is a compact e-commerce portal demo created for Assignment 3. Use the menu to navigate the page. Login/register to simulate orders, wishlist and reviews. Data persists using localStorage.</p>
    </section>

    <div class="grid" style="margin-top:16px">
      <div>
        <section id="products" class="card">
          <h3 class="section-title">Products</h3>
          <div class="products" id="products-list"></div>
        </section>

        <section id="reviews" class="card" style="margin-top:14px">
          <h3 class="section-title">Comments & Reviews</h3>
          <div id="reviews-list" class="list" style="min-height:60px"></div>
          <form id="review-form" style="margin-top:8px">
            <label for="review-text">Write a review (200 chars max)</label>
            <textarea id="review-text" maxlength="200" rows="3" placeholder="Share your thoughts..."></textarea>
            <div class="row">
              <input id="review-rating" type="number" min="1" max="5" placeholder="Rating 1-5" style="width:120px">
              <button class="btn" type="button" id="submit-review">Submit Review</button>
            </div>
            <div id="review-error" class="error"></div>
          </form>
        </section>

        <section id="reports" class="card" style="margin-top:14px">
          <h3 class="section-title">Reporting Options</h3>
          <p>Report a problem with an order or product.</p>
          <form id="report-form">
            <label for="report-type">Type</label>
            <select id="report-type"><option value="order">Order Issue</option><option value="product">Product Fault</option><option value="other">Other</option></select>
            <label for="report-desc">Description</label>
            <textarea id="report-desc" rows="3" placeholder="Describe the issue"></textarea>
            <button class="btn" type="button" id="submit-report">Send Report</button>
            <div id="report-msg" style="margin-top:8px;color:green;font-size:13px"></div>
          </form>
        </section>
      </div>

      <aside>
        <section class="card">
          <h3 class="section-title">User Area</h3>
          <div id="user-info">Not logged in.</div>
          <div style="margin-top:8px" id="user-actions"></div>
        </section>

        <section class="card" style="margin-top:14px">
          <h3 class="section-title">Orders</h3>
          <div id="orders-list" class="list">No orders yet.</div>
        </section>

        <section class="card" style="margin-top:14px">
          <h3 class="section-title">Wishlist</h3>
          <div id="wishlist" class="list">No wishlist items.</div>
        </section>

        <section id="contact" class="card" style="margin-top:14px">
          <h3 class="section-title">Customer Care</h3>
          <form id="contact-form">
            <label for="contact-name">Name</label>
            <input id="contact-name" placeholder="Your name">
            <label for="contact-msg">Message</label>
            <textarea id="contact-msg" rows="3"></textarea>
            <button class="btn" type="button" id="submit-contact">Send</button>
            <div id="contact-res" style="margin-top:8px;color:green;font-size:13px"></div>
          </form>
        </section>
      </aside>
    </div>

    <section id="about" class="card" style="margin-top:18px">
      <h3 class="section-title">About Us</h3>
      <p>Demo e-commerce portal for educational purposes. Contains navigation, forms with validation, orders/wishlist display, reviews, customer care and reporting options. Use this as a baseline and extend with a backend and authentication for production.</p>
    </section>

  </main>

  <footer>
    <small>Assignment 3: E-Commerce Portal — Demo by Student</small>
  </footer>

  <!-- Modal for Login/Register -->
  <div id="auth-modal" class="modal">
    <div class="card">
      <h3 id="auth-title">Login</h3>
      <form id="auth-form">
        <div id="auth-fields">
          <!-- fields inserted by JS -->
        </div>
        <div id="auth-error" class="error"></div>
        <div style="display:flex;gap:8px;margin-top:8px">
          <button class="btn" type="button" id="auth-submit">Submit</button>
          <button class="btn ghost" type="button" id="auth-toggle">Switch to Register</button>
          <button class="btn ghost" type="button" id="auth-close">Close</button>
        </div>
      </form>
    </div>
  </div>

  <script>
    /***********************
     * Sample Data & Helpers
     ***********************/
    const sampleProducts = [
      {id:1,name:'Wireless Headphones',price:1999},
      {id:2,name:'Smart Watch',price:2499},
      {id:3,name:'Bluetooth Speaker',price:1299},
      {id:4,name:'USB-C Charger',price:499},
      {id:5,name:'Laptop Stand',price:899}
    ];

    // Simple localStorage wrapper
    const DB = {
      get(key, fallback){ const v = localStorage.getItem(key); return v?JSON.parse(v):fallback },
      set(key,val){ localStorage.setItem(key,JSON.stringify(val)) }
    }

    // Initialize storage
    if(!DB.get('products',null)) DB.set('products', sampleProducts)
    if(!DB.get('users',null)) DB.set('users',[])
    if(!DB.get('reviews',null)) DB.set('reviews',[])

    /***********************
     * UI Rendering
     ***********************/
    const productsList = document.getElementById('products-list')
    const reviewsList = document.getElementById('reviews-list')
    const wishlistEl = document.getElementById('wishlist')
    const ordersList = document.getElementById('orders-list')
    const userInfo = document.getElementById('user-info')
    const userActions = document.getElementById('user-actions')

    function renderProducts(){
      const products = DB.get('products', [])
      productsList.innerHTML = ''
      products.forEach(p=>{
        const el = document.createElement('div'); el.className='product'
        el.innerHTML = `<strong>${p.name}</strong><div>₹ ${p.price}</div>`
        const addBtn = document.createElement('button'); addBtn.textContent='Add to Cart'; addBtn.className='btn'
        addBtn.onclick = ()=>placeOrder(p.id)
        const wishBtn = document.createElement('button'); wishBtn.textContent='Wishlist'; wishBtn.className='btn ghost'
        wishBtn.onclick = ()=>toggleWishlist(p.id)
        el.appendChild(addBtn); el.appendChild(wishBtn)
        productsList.appendChild(el)
      })
    }

    function renderReviews(){
      const reviews = DB.get('reviews', [])
      reviewsList.innerHTML = ''
      if(reviews.length===0) reviewsList.innerHTML = '<div style="color:#666">No reviews yet.</div>'
      reviews.forEach(r=>{
        const item = document.createElement('div'); item.className='list-item'
        item.innerHTML = `<div><strong>${r.user}</strong> — ${r.rating}/5<br><small>${r.text}</small></div>`
        reviewsList.appendChild(item)
      })
    }

    function renderUserArea(){
      const current = DB.get('currentUser', null)
      if(!current){
        userInfo.textContent = 'Not logged in.'
        userActions.innerHTML = '<button class="btn" id="open-login-2">Login / Register</button>'
        document.getElementById('open-login-2').onclick = openLogin
      } else {
        userInfo.innerHTML = `<div><strong>${current.name}</strong><div style="font-size:13px;color:#666">${current.email}</div></div>`
        userActions.innerHTML = '<button class="btn" id="logout">Logout</button>'
        document.getElementById('logout').onclick = ()=>{ DB.set('currentUser', null); renderAll() }
      }
    }

    function renderWishlist(){
      const wl = DB.get('wishlist', [])
      wishlistEl.innerHTML = ''
      if(wl.length===0) wishlistEl.innerHTML = 'No wishlist items.'
      wl.forEach(id=>{
        const p = sampleProducts.find(x=>x.id===id) || {name:'Unknown'}
        const item = document.createElement('div'); item.className='list-item'
        item.innerHTML = `<div>${p.name}</div><div><button class="btn ghost" onclick="removeWishlist(${id})">Remove</button></div>`
        wishlistEl.appendChild(item)
      })
    }

    function renderOrders(){
      const orders = DB.get('orders', [])
      ordersList.innerHTML = ''
      if(!orders || orders.length===0) ordersList.innerHTML='No orders placed.'
      orders.forEach(o=>{
        const p = sampleProducts.find(x=>x.id===o.pid) || {name:'Unknown'}
        const el = document.createElement('div'); el.className='list-item'
        el.innerHTML = `<div>${p.name}</div><div>Qty: ${o.qty} • ₹${o.price}</div>`
        ordersList.appendChild(el)
      })
    }

    function renderAll(){ renderProducts(); renderReviews(); renderWishlist(); renderOrders(); renderUserArea() }

    /***********************
     * Auth (Simple client-only) - Registration & Login validations
     ***********************/
    const authModal = document.getElementById('auth-modal')
    const authTitle = document.getElementById('auth-title')
    const authFields = document.getElementById('auth-fields')
    const authError = document.getElementById('auth-error')
    let authMode = 'login' // or 'register'

    document.getElementById('open-login').onclick = openLogin
    function openLogin(){ authMode='login'; buildAuth(); authModal.classList.add('open') }
    document.getElementById('auth-close').onclick = ()=>authModal.classList.remove('open')
    document.getElementById('auth-toggle').onclick = ()=>{ authMode = authMode==='login'?'register':'login'; buildAuth() }

    function buildAuth(){
      authError.textContent=''
      if(authMode==='login'){
        authTitle.textContent='Login'
        authFields.innerHTML = `
          <label for="login-email">Email</label>
          <input id="login-email" type="email" placeholder="name@example.com">
          <label for="login-pass">Password</label>
          <input id="login-pass" type="password" placeholder="At least 6 chars">
        `
        document.getElementById('auth-submit').textContent='Login'
      } else {
        authTitle.textContent='Register'
        authFields.innerHTML = `
          <label for="reg-name">Full Name</label>
          <input id="reg-name" placeholder="Your name">
          <label for="reg-email">Email</label>
          <input id="reg-email" type="email" placeholder="name@example.com">
          <label for="reg-pass">Password</label>
          <input id="reg-pass" type="password" placeholder="At least 6 chars">
          <label for="reg-pass2">Confirm Password</label>
          <input id="reg-pass2" type="password" placeholder="Repeat password">
        `
        document.getElementById('auth-submit').textContent='Register'
      }
    }

    document.getElementById('auth-submit').onclick = ()=>{
      authError.textContent=''
      if(authMode==='login') return doLogin()
      return doRegister()
    }

    function doRegister(){
      const name = document.getElementById('reg-name').value.trim()
      const email = document.getElementById('reg-email').value.trim().toLowerCase()
      const pass = document.getElementById('reg-pass').value
      const pass2 = document.getElementById('reg-pass2').value

      // validations
      if(name.length<3) return authError.textContent='Name must be at least 3 characters.'
      if(!validateEmail(email)) return authError.textContent='Enter a valid email.'
      if(pass.length<6) return authError.textContent='Password must be at least 6 characters.'
      if(pass!==pass2) return authError.textContent='Passwords do not match.'

      const users = DB.get('users', [])
      if(users.find(u=>u.email===email)) return authError.textContent='Email already registered.'

      users.push({name,email,pass})
      DB.set('users', users)
      DB.set('currentUser', {name,email})
      authModal.classList.remove('open')
      renderAll()
    }

    function doLogin(){
      const email = document.getElementById('login-email').value.trim().toLowerCase()
      const pass = document.getElementById('login-pass').value
      if(!validateEmail(email)) return authError.textContent='Enter a valid email.'
      if(pass.length<6) return authError.textContent='Password must be at least 6 characters.'
      const users = DB.get('users', [])
      const user = users.find(u=>u.email===email && u.pass===pass)
      if(!user) return authError.textContent='Invalid credentials.'
      DB.set('currentUser', {name:user.name,email:user.email})
      authModal.classList.remove('open')
      renderAll()
    }

    function validateEmail(email){
      // simple regex for validation
      return /^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(email)
    }

    /***********************
     * Wishlist, Orders & Actions
     ***********************/
    function toggleWishlist(pid){
      const wl = DB.get('wishlist', [])
      const idx = wl.indexOf(pid)
      if(idx===-1){ wl.push(pid) } else { wl.splice(idx,1) }
      DB.set('wishlist', wl)
      renderWishlist()
    }

    window.removeWishlist = function(pid){ toggleWishlist(pid) }

    function placeOrder(pid){
      const user = DB.get('currentUser', null)
      if(!user){ alert('Please login to place orders.'); openLogin(); return }
      const prod = sampleProducts.find(p=>p.id===pid)
      const orders = DB.get('orders', [])
      const order = {pid, qty:1, price: prod.price, user: user.email, created: new Date().toISOString()}
      orders.push(order)
      DB.set('orders', orders)
      renderOrders()
      alert('Order placed successfully!')
    }

    /***********************
     * Reviews & Reports & Contact
     ***********************/
    document.getElementById('submit-review').onclick = ()=>{
      const text = document.getElementById('review-text').value.trim()
      const rating = Number(document.getElementById('review-rating').value)
      const user = DB.get('currentUser', {name:'Anonymous'})
      const err = document.getElementById('review-error')
      err.textContent=''
      if(text.length<5) return err.textContent='Review must be at least 5 characters.'
      if(!rating || rating<1 || rating>5) return err.textContent='Provide a rating between 1 and 5.'
      const reviews = DB.get('reviews', [])
      reviews.unshift({user:user.name||user.email||'Anonymous',text,rating,at:new Date().toISOString()})
      DB.set('reviews', reviews)
      document.getElementById('review-text').value=''
      document.getElementById('review-rating').value=''
      renderReviews()
    }

    document.getElementById('submit-report').onclick = ()=>{
      const t = document.getElementById('report-type').value
      const d = document.getElementById('report-desc').value.trim()
      if(d.length<10) return document.getElementById('report-msg').textContent='Please describe the issue (10 chars min).'
      // For demo just store reports
      const reports = DB.get('reports', [])
      reports.push({type:t,desc:d,at:new Date().toISOString()})
      DB.set('reports', reports)
      document.getElementById('report-desc').value=''
      document.getElementById('report-msg').textContent='Report submitted. Customer care will reach out.'
      setTimeout(()=>document.getElementById('report-msg').textContent='',3000)
    }

    document.getElementById('submit-contact').onclick = ()=>{
      const name = document.getElementById('contact-name').value.trim()
      const msg = document.getElementById('contact-msg').value.trim()
      if(name.length<2) return document.getElementById('contact-res').textContent='Enter a valid name.'
      if(msg.length<6) return document.getElementById('contact-res').textContent='Message too short.'
      const contacts = DB.get('contacts', [])
      contacts.push({name,msg,at:new Date().toISOString()})
      DB.set('contacts', contacts)
      document.getElementById('contact-name').value=''; document.getElementById('contact-msg').value=''
      document.getElementById('contact-res').textContent='Message sent. We will respond within 2 business days.'
      setTimeout(()=>document.getElementById('contact-res').textContent='',4000)
    }

    // Initial render
    renderAll()
  </script>
</body>
</html>
