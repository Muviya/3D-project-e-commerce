Yes. Your uploaded specification is essentially asking for a 3D e-commerce website + AI shopping assistant. The document recommends building the core 3D store first and adding the AI assistant after the cart flow works. �
3D_Store_AI_Agent_Project_Spec.pdf
I would build it as a MERN + Three.js/React Three Fiber + AI API project.
1. Overall architecture
                         USER
                           |
                           v
                ┌─────────────────────┐
                │    React Frontend   │
                │                     │
                │  3D Virtual Store   │
                │  Product Viewer     │
                │  Product Details    │
                │  Cart               │
                │  AI Chat Assistant  │
                └──────────┬──────────┘
                           |
              ┌────────────┴────────────┐
              |                         |
              v                         v
     ┌────────────────┐       ┌─────────────────┐
     │ Three.js /     │       │ Backend         │
     │ React Three    │       │ Node + Express  │
     │ Fiber          │       │ REST APIs       │
     └────────────────┘       └────────┬────────┘
                                       |
                        ┌──────────────┼──────────────┐
                        |              |              |
                        v              v              v
                  ┌──────────┐   ┌──────────┐   ┌────────────┐
                  │ MongoDB  │   │ AI API   │   │ 3D Models  │
                  │ Database │   │ LLM      │   │ GLB/GLTF   │
                  └──────────┘   └──────────┘   └────────────┘
The specification itself separates the 3D UI, product viewer, product details, AI assistant, cart, backend API and database in essentially this way. �
3D_Store_AI_Agent_Project_Spec.pdf
2. Technologies I recommend
Frontend
Requirement
Technology
UI
React.js
3D environment
Three.js
React 3D integration
React Three Fiber
3D helpers
Drei
Styling
CSS / Tailwind CSS
Routing
React Router
API calls
Axios
State management
Context API initially
3D format
.glb / .gltf
Backend
Requirement
Technology
Server
Node.js
API
Express.js
Database
MongoDB
ODM
Mongoose
Authentication
JWT
Password security
bcrypt
Validation
Joi/Zod
AI
For the AI shopping assistant:
User question
      ↓
AI Model
      ↓
Extract requirements
      ↓
Product search
      ↓
MongoDB
      ↓
Matching products
      ↓
AI response
For the first version, use an LLM with function/tool calling rather than trying to train your own AI model.
3. Main modules
Your complete project should have 8 modules.
3D E-Commerce Store
│
├── 1. Authentication
│
├── 2. 3D Virtual Store
│
├── 3. Product Management
│
├── 4. 3D Product Viewer
│
├── 5. Product Variants
│
├── 6. Shopping Cart
│
├── 7. AI Shopping Assistant
│
└── 8. Checkout
The specification requires product information such as name, category, price, description, images, 3D model, colors, sizes, stock and rating. �
3D_Store_AI_Agent_Project_Spec.pdf
4. Module 1 — Login/Register
Optional for the minimum MVP, but recommended for the complete project.
Register
Name
Email
Password
Confirm Password
       ↓
Backend
       ↓
MongoDB
Login
Email + Password
       ↓
Express API
       ↓
Check MongoDB
       ↓
JWT Token
       ↓
React Dashboard
Methods
POST /api/auth/register
POST /api/auth/login
GET  /api/auth/profile
5. Module 2 — 3D Virtual Store
This is the main attraction.
Instead of:
Product 1
Product 2
Product 3
you create:
              3D STORE

       ┌─────────────────────┐
       │                     │
       │     🛍 SHOES        │
       │                     │
       │  👟    👟    👟     │
       │                     │
       │                     │
       │  CLOTHING           │
       │                     │
       └─────────────────────┘
User can move around the store.
Technology
React
  +
React Three Fiber
  +
Three.js
  +
Drei
3D objects
Floor
Walls
Shelves
Lights
Products
Category signs
Camera
Interaction
Mouse movement
     ↓
Camera movement

Click product
     ↓
Open product
The specification specifically expects a virtual store where users can navigate and see categories/products in the 3D environment. �
3D_Store_AI_Agent_Project_Spec.pdf
6. Module 3 — Product Database
MongoDB collection:
products
Example:
{
  "productId": "shoe_001",
  "name": "Running Shoe",
  "category": "Shoes",
  "price": 2499,
  "description": "Comfortable running shoe",
  "image": "/images/shoe1.jpg",
  "model": "/models/shoe1.glb",
  "colors": ["Black", "White", "Blue"],
  "sizes": [7, 8, 9, 10],
  "stock": 20,
  "rating": 4.5
}
This follows the product structure given in your project specification. �
3D_Store_AI_Agent_Project_Spec.pdf
7. Module 4 — 3D Product Viewer
When the user clicks:
3D Shoe
   ↓
Product Details Page
Screen:
┌─────────────────────────────────────┐
│                                     │
│          3D PRODUCT                 │
│                                     │
│              👟                     │
│                                     │
│       ↻ Rotate   🔍 Zoom            │
│                                     │
├─────────────────────────────────────┤
│ Running Shoe                        │
│ ₹2,499                              │
│                                     │
│ Color:  ● Black ● White ● Blue      │
│ Size:   7  8  9  10                 │
│                                     │
│ Quantity:  -  1  +                  │
│                                     │
│        [ ADD TO CART ]              │
└─────────────────────────────────────┘
The specification requires rotate, zoom, different angles, product details and variant selection. �
3D_Store_AI_Agent_Project_Spec.pdf
Three.js methods/features
Use:
OrbitControls
GLTFLoader
PerspectiveCamera
DirectionalLight
Environment
Suspense
The easiest approach is:
<Canvas>
   <Environment />
   <Model />
   <OrbitControls />
</Canvas>
8. Module 5 — Product Variants
This is important because the cart must remember the exact selection.
Example:
Product:
Nike Running Shoe

Color:
Black

Size:
9

Quantity:
2
Store this as:
{
    productId: "shoe_001",
    name: "Nike Running Shoe",
    color: "Black",
    size: 9,
    quantity: 2,
    price: 2499,
    model: "/models/shoe_001.glb"
}
The specification explicitly says the selected color, size and quantity should be stored with the cart item. �
3D_Store_AI_Agent_Project_Spec.pdf
9. Module 6 — Shopping Cart
For your first working version, use:
LocalStorage
because the specification explicitly identifies LocalStorage as suitable for the student MVP. �
3D_Store_AI_Agent_Project_Spec.pdf
Flow:
Product
   ↓
Select color
   ↓
Select size
   ↓
Select quantity
   ↓
Add to Cart
   ↓
localStorage
   ↓
Cart Page
Example:
localStorage.setItem(
    "cart",
    JSON.stringify(cart)
);
Later upgrade it to:
React
 ↓
Express API
 ↓
MongoDB
 ↓
User Cart
10. Module 7 — AI Shopping Assistant
This is where your project becomes more interesting.
The specification gives this type of requirement:
User:
"Show me black shoes under ₹3000"

AI:
I found 3 products:

1. Black Running Shoe - ₹2499
2. Black Casual Shoe - ₹2799
3. Black Sports Shoe - ₹2999
�
3D_Store_AI_Agent_Project_Spec.pdf
Don't train an AI model yourself
For a student project, do not build/train an LLM from scratch.
Use:
LLM
+
Function Calling
+
MongoDB Product Search
Architecture:
             USER
               |
               v
       "Black shoes below 3000"
               |
               v
          AI MODEL
               |
               v
       Extract parameters
               |
       ┌───────┴────────┐
       ↓                ↓
 category = shoes    price < 3000
       |                |
       └───────┬────────┘
               ↓
          Product API
               ↓
           MongoDB
               ↓
       Matching Products
               ↓
           AI MODEL
               ↓
        Natural Response
11. AI model methods
I recommend using these AI techniques:
1. Intent Detection
Understand what the user wants.
Example:
"Show me black shoes below 3000"
AI extracts:
{
  "category": "Shoes",
  "color": "Black",
  "maxPrice": 3000
}
2. Entity Extraction
Extract:
Product category
Color
Size
Price
Brand
Quantity
3. Tool/Function Calling
AI calls your backend:
searchProducts({
    category: "Shoes",
    color: "Black",
    maxPrice: 3000
})
4. Database Filtering
MongoDB performs the actual product search.
Important: Don't allow the AI to invent product prices or stock. Your database should be the source of truth.
12. AI Assistant UI
Make it like:
                         ┌──────────────────────┐
                         │   AI SHOPPING AI     │
                         ├──────────────────────┤
                         │                      │
                         │ You:                 │
                         │ Black shoes under    │
                         │ ₹3000                │
                         │                      │
                         │ AI:                  │
                         │ I found 3 products.  │
                         │                      │
                         │ 👟 Running Shoe      │
                         │ ₹2499                │
                         │ [View in 3D]         │
                         │                      │
                         │ 👟 Casual Shoe       │
                         │ ₹2799                │
                         │ [View in 3D]         │
                         │                      │
                         ├──────────────────────┤
                         │ Type message...  ➤   │
                         └──────────────────────┘
When the user clicks:
[View in 3D]
navigate to:
/product/shoe_001
Then open the 3D viewer.
13. Backend architecture
Your Express backend can be:
server/
│
├── controllers/
│   ├── authController.js
│   ├── productController.js
│   ├── cartController.js
│   └── aiController.js
│
├── models/
│   ├── User.js
│   ├── Product.js
│   └── Cart.js
│
├── routes/
│   ├── authRoutes.js
│   ├── productRoutes.js
│   ├── cartRoutes.js
│   └── aiRoutes.js
│
├── services/
│   └── aiService.js
│
├── middleware/
│   └── authMiddleware.js
│
├── config/
│   └── db.js
│
└── server.js
14. Frontend architecture
client/
│
├── public/
│   ├── models/
│   │   ├── shoe1.glb
│   │   ├── shoe2.glb
│   │   └── bag1.glb
│   │
│   └── images/
│
├── src/
│   │
│   ├── components/
│   │   ├── Navbar.jsx
│   │   ├── ProductCard.jsx
│   │   ├── ProductViewer.jsx
│   │   ├── VariantSelector.jsx
│   │   ├── QuantitySelector.jsx
│   │   ├── CartItem.jsx
│   │   └── AIChat.jsx
│   │
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── Store3D.jsx
│   │   ├── ProductDetails.jsx
│   │   ├── Cart.jsx
│   │   ├── Checkout.jsx
│   │   ├── Login.jsx
│   │   └── Register.jsx
│   │
│   ├── context/
│   │   ├── CartContext.jsx
│   │   └── AuthContext.jsx
│   │
│   ├── services/
│   │   └── api.js
│   │
│   ├── App.jsx
│   └── main.jsx
15. Complete user flow
Your final project should work like this:
                     WEBSITE
                        |
                        ↓
                  HOME PAGE
                        |
                        ↓
                  ENTER STORE
                        |
                        ↓
                 3D STORE ROOM
                        |
              ┌─────────┴─────────┐
              ↓                   ↓
           Product              AI
              ↓                   ↓
       3D Product Viewer    Search Product
              ↓                   ↓
       Rotate / Zoom        Matching Products
              ↓                   ↓
       Select Color          Open Product
              ↓                   ↓
        Select Size              └─────┐
              ↓                         ↓
        Select Quantity          3D Product
              ↓
         Add to Cart
              ↓
            CART
              ↓
          CHECKOUT
16. Development order — very important
Don't start everything at once.
Build in this order:
Phase 1 — Basic React
React project
Navbar
Home
Routing
Basic UI
↓
Phase 2 — Product database
MongoDB
Product schema
Add products
GET products API
↓
Phase 3 — 3D
Three.js
React Three Fiber
GLB models
3D Store
Camera
Lighting
OrbitControls
↓
Phase 4 — Product interaction
Click 3D product
        ↓
Product Details
        ↓
3D Viewer
        ↓
Rotate
Zoom
↓
Phase 5 — Variants
Color
Size
Quantity
↓
Phase 6 — Cart
Add to Cart
        ↓
LocalStorage
        ↓
Cart Page
        ↓
Total
↓
Phase 7 — Backend Cart
After the above works:
React
 ↓
Express
 ↓
MongoDB
↓
Phase 8 — AI
Finally:
AI Chat
 ↓
Intent extraction
 ↓
Function calling
 ↓
Product API
 ↓
MongoDB
 ↓
Products
 ↓
3D Viewer
17. What models/methods should you use?
For your project report, you can describe it like this:
AI model
Large Language Model (LLM)
Purpose:
Natural language understanding
Product search
User query interpretation
Product recommendation based on filters
AI techniques
1. Natural Language Processing
2. Intent Detection
3. Entity/Parameter Extraction
4. Function Calling / Tool Calling
5. Database-backed Product Retrieval
6. Context-aware Conversational Interaction
3D methods
Three.js
React Three Fiber
GLTF/GLB loading
OrbitControls
Perspective Camera
Lighting
Raycasting / pointer interaction
Software architecture
I recommend:
                MVC + REST API
Backend:
Controller
    ↓
Service
    ↓
Model
    ↓
MongoDB
Frontend:
Pages
 ↓
Components
 ↓
Context/State
 ↓
API Service
 ↓
Backend
18. Database design
You can start with these collections:
MongoDB
│
├── users
│
├── products
│
├── carts
│
└── orders
Products
_id
productId
name
category
price
description
images
model
colors
sizes
stock
rating
Cart
userId
items[]
    ├── productId
    ├── color
    ├── size
    ├── quantity
    ├── price
    └── model
totalAmount
Orders
orderId
userId
items
totalAmount
paymentStatus
orderStatus
createdAt
19. Minimum version vs final version
MVP
You should first achieve:
✅ 3D Store
✅ 3D Products
✅ Click Product
✅ Product Details
✅ Rotate
✅ Zoom
✅ Color
✅ Size
✅ Quantity
✅ Add to Cart
✅ LocalStorage Cart
This exactly follows the minimum implementation sequence in the supplied specification. �
3D_Store_AI_Agent_Project_Spec.pdf
Final project
Then add:
✅ Login/Register
✅ MongoDB
✅ Backend
✅ Persistent Cart
✅ AI Assistant
✅ AI Product Search
✅ AI → Product → 3D Viewer
✅ Checkout
✅ Orders
✅ Admin Product Management
The final demonstration should be able to show selecting a product, manipulating its 3D view, choosing Black/Size 9/Quantity 2, adding it to the cart, verifying the stored details, and then using the AI assistant to find another product. �
3D_Store_AI_Agent_Project_Spec.pdf
⭐ My recommended final stack
FRONTEND
React.js
   +
Three.js
   +
React Three Fiber
   +
Drei
   +
Tailwind/CSS

BACKEND
Node.js
   +
Express.js
   +
REST API

DATABASE
MongoDB
   +
Mongoose

AI
LLM API
   +
Function Calling
   +
NLP / Intent Extraction

3D
GLB / GLTF
   +
Three.js
   +
OrbitControls

AUTH
JWT
   +
bcrypt
Best approach for you: don't begin with AI. First make one 3D shoe → click → rotate/zoom → select color/size → add to cart completely functional. Then expand to multiple products/categories, backend/database, and finally the AI assistant. This keeps the project manageable and matches the order recommended in your internship specification.
