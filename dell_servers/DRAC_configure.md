### **How to Configure iDRAC on Dell PowerEdge 750**  
Integrated Dell Remote Access Controller (**iDRAC**) allows remote management of your Dell server, even if it's powered off.  

---

### **1️⃣ Access iDRAC Setup**
1. **Power on the Server** and press `F2` to enter **System Setup**.  
2. Go to **iDRAC Settings** → Select **Network**.  

---

### **2️⃣ Configure Network Settings**
1. **Enable iDRAC** (if not enabled).  
2. **Set Network Mode**:  
   - Choose **Dedicated** (if your server has a separate iDRAC port).  
   - Choose **Shared LOM** (if using regular Ethernet ports).  
3. **Set IP Address**:  
   - Use **Static IP** (recommended for stable access).  
   - Example:  
     - IP: `192.168.1.100`  
     - Subnet: `255.255.255.0`  
     - Gateway: `192.168.1.1`  
   - Or choose **DHCP** if you want it assigned automatically.  
4. **Save & Exit** → Reboot the server.  

---

### **3️⃣ Access iDRAC Web Interface**
1. Open a browser on your laptop.  
2. Enter `https://192.168.1.100` (replace with your iDRAC IP).  
3. **Login** (Default credentials):  
   - **Username**: `root`  
   - **Password**: `calvin`  

---

### **4️⃣ Configure & Secure iDRAC**
1. **Change Default Password**: Go to **Settings → User Configuration**.  
2. **Enable Virtual Console & Virtual Media** for remote control.  
3. **Set Up Email Alerts**: Configure notifications for hardware failures.  

---

### **5️⃣ Use iDRAC for Remote Management**
- **Power on/off server** remotely.  
- **Monitor system health** (CPU, RAM, RAID).  
- **Update firmware** via iDRAC.  
- **Remote Console** → Access BIOS & reinstall OS without physical access.  

✅ **Now, you can fully manage your Dell server remotely!** 🚀 Let me know if you need advanced configurations.
