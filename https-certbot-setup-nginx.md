## Setup HTTPS by certbot nginx



1. **First, install the required packages on your system:**
    ```
    sudo apt install certbot python3-certbot-nginx -y
    ```
2. **Next, run certbot:**

    ```
    certbot --nginx
    ```
   

3. **Your security certificate must be renewed every 90 days. Certbot schedules the renewal automatically. Run a test to verify this process is in place:**
    ```
    sudo certbot renew --dry-run

    ```
    If this command returns no errors, you have successfully enabled HTTPS in your Nginx server.