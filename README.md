<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>المتجر الاحترافي V2</title>

<style>
body{
margin:0;
font-family:Arial;
background:#f5f7fa;
direction:rtl;
}

/* الهيدر */
header{
background:linear-gradient(90deg,#111,#333);
color:white;
padding:15px;
text-align:center;
font-size:22px;
font-weight:bold;
}

/* شريط البحث */
.topbar{
display:flex;
gap:10px;
padding:10px;
background:#222;
}

.topbar input{
flex:1;
padding:10px;
border:none;
border-radius:8px;
}

.cartBtn{
color:white;
cursor:pointer;
}

/* التصنيفات */
.categories{
display:flex;
gap:10px;
overflow:auto;
padding:10px;
background:#fff;
}

.cat{
background:#eee;
padding:8px 12px;
border-radius:20px;
cursor:pointer;
white-space:nowrap;
}

.cat:hover{
background:#ddd;
}

/* المنتجات */
.container{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(170px,1fr));
gap:15px;
padding:15px;
}

.card{
background:white;
border-radius:12px;
overflow:hidden;
box-shadow:0 4px 10px rgba(0,0,0,0.1);
cursor:pointer;
transition:0.3s;
}

.card:hover{
transform:scale(1.03);
}

.card img{
width:100%;
height:140px;
object-fit:cover;
}

.card h3{
margin:5px;
font-size:16px;
}

.price{
color:green;
font-weight:bold;
}

.card button{
width:100%;
padding:8px;
border:none;
background:orange;
color:white;
cursor:pointer;
}

/* صفحة المنتج */
.productPage{
display:none;
padding:15px;
}

.productPage img{
width:100%;
border-radius:10px;
}

.back{
background:#111;
color:white;
padding:8px;
border:none;
margin-bottom:10px;
cursor:pointer;
}

/* السلة */
.cartBox{
position:fixed;
top:0;
right:-100%;
width:320px;
height:100%;
background:white;
box-shadow:-2px 0 10px rgba(0,0,0,0.2);
padding:10px;
transition:0.3s;
overflow:auto;
}

.close{
background:red;
color:white;
padding:8px;
border:none;
cursor:pointer;
width:100%;
}

.item{
border-bottom:1px solid #ddd;
padding:5px;
}

.buy{
background:green;
color:white;
width:100%;
padding:10px;
border:none;
margin-top:10px;
cursor:pointer;
}
</style>
</head>

<body>

<header>🛒تحطيم الاسعار  V2</header> Apex store

<div class="topbar">
<input id="search" placeholder="ابحث عن منتج...">
<div class="cartBtn" onclick="openCart()">🧺 السلة (<span id="count">0</span>)</div>
</div>

<!-- التصنيفات -->
<div class="categories">
<div class="cat" onclick="filterCat('all')">الكل</div>
<div class="cat" onclick="filterCat('electronics')">إلكترونيات</div>
<div class="cat" onclick="filterCat('fashion')">أزياء</div>
</div>

<!-- المنتجات -->
<div class="container" id="products"></div>

<!-- صفحة المنتج -->
<div class="productPage" id="productPage">
<button class="back" onclick="back()">رجوع</button>
<div id="productDetails"></div>
</div>

<!-- السلة -->
<div class="cartBox" id="cartBox">
<button class="close" onclick="closeCart()">إغلاق السلة</button>
<div id="cartItems"></div>
<p>الإجمالي: $<span id="total">0</span></p>
<button class="buy">إتمام الطلب (أفلييت)</button>
</div>

<script>

let products = [
{name:"سماعات احترافية", price:20, cat:"electronics", img:"https://images.unsplash.com/photo-1511707171634-5f897ff02aa9"},
{name:"ساعة ذكية", price:30, cat:"electronics", img:"https://images.unsplash.com/photo-1510557880182-3d4d3cba35a5"},
{name:"جاكيت عصري", price:40, cat:"fashion", img:"https://images.unsplash.com/photo-1520975958225-0c6c4c6b0a0a"},
{name:"هاتف قوي", price:199, cat:"electronics", img:"https://images.unsplash.com/photo-1511707171634-5f897ff02aa9"}
];

let cart = JSON.parse(localStorage.getItem("cart")) || [];

// عرض المنتجات
function show(list){
let box=document.getElementById("products");
box.innerHTML="";

list.forEach((p,i)=>{
box.innerHTML+=`
<div class="card">
<img src="${p.img}" onclick="openProduct(${i})">
<h3>${p.name}</h3>
<p class="price">$${p.price}</p>
<button onclick="add(${i})">أضف للسلة</button>
</div>
`;
});
}

show(products);

// بحث
document.getElementById("search").addEventListener("input",function(){
let v=this.value.toLowerCase();
let f=products.filter(p=>p.name.toLowerCase().includes(v));
show(f);
});

// تصنيف
function filterCat(c){
if(c=="all") show(products);
else show(products.filter(p=>p.cat==c));
}

// صفحة المنتج
function openProduct(i){
let p=products[i];
document.getElementById("productPage").style.display="block";
document.getElementById("products").style.display="none";

document.getElementById("productDetails").innerHTML=`
<img src="${p.img}">
<h2>${p.name}</h2>
<p class="price">$${p.price}</p>
<button onclick="add(${i})">أضف للسلة</button>
`;
}

function back(){
document.getElementById("productPage").style.display="none";
document.getElementById("products").style.display="grid";
}

// السلة
function add(i){
cart.push(products[i]);
localStorage.setItem("cart",JSON.stringify(cart));
update();
}

function update(){
document.getElementById("count").innerText=cart.length;

let box=document.getElementById("cartItems");
box.innerHTML="";

let total=0;

cart.forEach(p=>{
box.innerHTML+=`<div class="item">${p.name} - $${p.price}</div>`;
total+=p.price;
});

document.getElementById("total").innerText=total;
}

function openCart(){
document.getElementById("cartBox").style.right="0";
update();
}

function closeCart(){
document.getElementById("cartBox").style.right="-100%";
}

update();

</script>

</body>
</html>
