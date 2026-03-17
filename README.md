# E-10-LAB-Hosting-Multiple-Websites-on-One-Server-Using-Multiple-IPs-Nginx-
Serving Multiple Applications Using Nginx and Elastic IPs


# Step 1: Create EC2 + IP setup

You already did this, just summarizing correctly:

Launch EC2 instance

Attach secondary private IP

EC2 → Network Interface → Actions → Manage IP → Add private IP

Allocate Elastic IPs

EC2 → Elastic IPs → Allocate

Associate Elastic IPs:

Attach each Elastic IP to:

Primary private IP

Secondary private IP


# Step 2: Install Nginx

sudo yum update -y

sudo yum install nginx -y

sudo systemctl start nginx

sudo systemctl enable nginx


# Step 3: Create Website Directories

cd /usr/share/nginx/html

sudo mkdir site1

sudo mkdir site2

# Step 4:  Create index files

cd site1

sudo vi index.html

Example content:

<h1> Welcome to Site 1 </h1>


cd ../site2

sudo nano index.html

Example:

<h1> Welcome to Site 2 </h1>

# Step 4: Fix Permissions

sudo chown -R nginx:nginx /usr/share/nginx/html


# Step 5: Configure Nginx (IMPORTANT)

cd /etc/nginx/conf.d/


sudo nano multiserver.conf

# Add this:
server {
    listen YOUR_PRIVATE_IP_1:80;
    root /usr/share/nginx/html/site1;
}

server {
    listen YOUR_PRIVATE_IP_2:80;
    root /usr/share/nginx/html/site2;
}



# Step 6: Test and Restart

sudo nginx -t


sudo systemctl restart nginx

# Step 7: Security Group (VERY IMPORTANT)

Make sure:

Port 80 (HTTP) is allowed in EC2 Security Group

Source: 0.0.0.0/0

# Step 8: Test in Browser

Open:

http://Elastic_IP_1 → should show Site 1

http://Elastic_IP_2 → should show Site 2


# Common mistakes (why it wasn’t working)

Here’s what usually goes wrong:

Using same server block for both IPs

Wrong server_name

Nginx default config overriding yours

Forgot restart nginx

Port 80 not open

Elastic IP not properly associated



