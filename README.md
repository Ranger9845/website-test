# Layer Monster - 3D Prints Store

A modern e-commerce platform for selling 3D printed products with MongoDB backend.

## Setup Instructions

### 1. Install Node.js
Download from https://nodejs.org/

### 2. Install Dependencies
```bash
npm install
```

### 3. Start the Server
```bash
npm start
```

The server will run on `http://localhost:5000`

### 4. Open in Browser
- **Main Store**: Open `index.html` in your browser (use a local server or just open the file)
- **Admin Panel**: Open `admin.html` in your browser (secure access - password recommended for production)

## Features

✅ Add, edit, and delete products from admin panel  
✅ Real-time product display on main store  
✅ Products stored in MongoDB Atlas  
✅ Modern UI with smooth animations  
✅ Responsive design  
✅ Auto-refreshing product list  

## File Structure

```
├── index.html          # Main storefront
├── admin.html          # Admin control panel
├── server.js           # Node.js/Express backend
├── package.json        # Project dependencies
├── .env               # Environment variables
└── README.md          # This file
```

## API Endpoints

### Products
- `GET /api/products` - Get all products
- `POST /api/products` - Create new product
- `PUT /api/products/:id` - Update product
- `DELETE /api/products/:id` - Delete product
- `GET /api/health` - Health check

## Deployment to Linode

### Option 1: Using Node.js on Linode

1. Create a Linode VPS
2. SSH into your server:
```bash
ssh root@YOUR_LINODE_IP
```

3. Install Node.js:
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

4. Install PM2 (process manager):
```bash
sudo npm install -g pm2
```

5. Upload your files to Linode:
```bash
scp -r /path/to/your/files root@YOUR_LINODE_IP:/home/username/layer-monster/
```

6. Install dependencies on Linode:
```bash
cd /home/username/layer-monster
npm install
```

7. Start the server with PM2:
```bash
pm2 start server.js --name "layer-monster"
pm2 startup
pm2 save
```

8. Install Nginx to serve frontend:
```bash
sudo apt update
sudo apt install nginx
```

9. Create Nginx config at `/etc/nginx/sites-available/layer-monster`:
```nginx
server {
    listen 80;
    server_name YOUR_DOMAIN_OR_IP;

    # Serve static files
    location / {
        root /home/username/layer-monster;
        try_files $uri $uri/ =404;
    }

    # Proxy API requests to Node.js
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

10. Enable the site:
```bash
sudo ln -s /etc/nginx/sites-available/layer-monster /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### Option 2: Docker Deployment
Create a Dockerfile for easier deployment to Linode.

## MongoDB Connection
The app uses MongoDB Atlas with the provided connection string. Make sure your MongoDB credentials are correct in the `.env` file.

## Notes
- Admin panel is NOT visible on the main store page
- Products are saved to MongoDB when added
- All changes are real-time
- For production, add authentication to admin.html
