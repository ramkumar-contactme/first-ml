# Setting Up and Running Jupyter Notebook on an Ubuntu EC2 Instance

## **1. Install Jupyter Notebook on Ubuntu EC2**

### **Step 1: Update and Upgrade Ubuntu Packages**

```bash
sudo apt update && sudo apt upgrade -y
```

### **Step 2: Install Python and Pip**

```bash
sudo apt install python3 python3-pip -y
```

Verify installation:

```bash
python3 --version
pip3 --version
```

### **Step 3: Install Virtual Environment (Optional but Recommended)**

```bash
sudo apt install python3-venv -y
python3 -m venv jupyter_env
source jupyter_env/bin/activate
```

### **Step 4: Install Jupyter Notebook**

```bash
pip install jupyter
```

## **2. Start Jupyter Notebook with Public Access**

To access Jupyter Notebook from a browser using the EC2 Public IP, start it with the following command:

```bash
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root
```

This will display an output with a URL containing a token, such as:

```
http://localhost:8888/tree?token=YOUR_TOKEN
```

## **3. Configure EC2 Security Group to Allow Access**

Since Jupyter runs on port `8888`, update your **AWS Security Group** settings:

1. Open **AWS EC2 Console** → **Instances**.
2. Select your instance → Click on **Security**.
3. Under **Security Groups**, click on the **linked security group**.
4. Edit **Inbound Rules**:
   - Add a new rule:
     - **Type**: Custom TCP Rule
     - **Protocol**: TCP
     - **Port Range**: `8888`
     - **Source**: Your IP (`My IP`) or **Anywhere (`0.0.0.0/0` for testing, but restrict later)**.
   - Click **Save Rules**.

## **4. Access Jupyter Notebook from a Browser**

Find your **EC2 public IP** using:

```bash
curl ifconfig.me
```

Now, open Jupyter in your browser:

```
http://<EC2_PUBLIC_IP>:8888/tree?token=YOUR_TOKEN
```

## **5. (Optional) Run Jupyter in the Background**

To keep Jupyter running even after closing the SSH session:

```bash
nohup jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root > jupyter.log 2>&1 &
```

## **6. Stop Jupyter Notebook**

### **Method 1: Kill the Jupyter Process Manually**

1. Find the process ID (PID):
   ```bash
   ps aux | grep jupyter
   ```
2. Kill the process:
   ```bash
   kill -9 <PID>
   ```

### **Method 2: Stop All Running Jupyter Instances**

```bash
pkill -9 -f jupyter
```

### **Method 3: Stop Jupyter Using Built-in Command**

Check running instances:

```bash
jupyter notebook list
```

Stop Jupyter:

```bash
jupyter notebook stop 8888
```
