Suspicious service আর malware-related service identify করা IT audit-এর খুব গুরুত্বপূর্ণ অংশ 🔍
আমি তোমাকে **practical checklist + commands + red flags** দিয়ে বুঝাচ্ছি, যাতে তুমি real system-এ ধরতে পারো 👇

---

# 🧠 1. Suspicious Service কীভাবে চিনবে?

## 🔴 Red Flags (সবচেয়ে important)

যে service গুলো দেখলে alert হও:

* Random/meaningless name
  👉 `asdkf.exe`, `xj32svc`, `update123`
* No description / blank description
* Unknown publisher
* System32 ছাড়া অন্য location
* Auto start without reason
* High CPU / network usage
* Hidden service (GUI-তে clear না)

---

# 🧪 2. CMD দিয়ে Service Details বের করা

## Step 1: Service list

```cmd id="svc1"
sc query state= running
```

## Step 2: Specific service details

```cmd id="svc2"
sc qc ServiceName
```

👉 এখানে দেখবে:

* BINARY_PATH_NAME
* START_TYPE

📌 Example:

```
BINARY_PATH_NAME : C:\Users\user\AppData\abc.exe ❌
```

👉 Suspicious ⚠️

---

# 🔍 3. PID → Process mapping

```cmd id="svc3"
sc queryex ServiceName
```

👉 PID পাবে

তারপর:

```cmd id="svc4"
tasklist /fi "PID eq 1234"
```

👉 কোন EXE run করছে বুঝবে

---

# 📂 4. File location verify

👉 Task Manager → Right click process → Open file location

### Safe location:

* `C:\Windows\System32\` ✅

### Suspicious:

* `AppData\Local\Temp` ❌
* `Downloads` folder ❌
* Random folder ❌

---

# 🌐 5. Network connection check

```cmd id="net1"
netstat -ano
```

👉 দেখো:

* Unknown IP connect আছে কিনা
* Unknown process port open করছে কিনা

---

# 🧪 6. PowerShell Advanced Check

```powershell id="ps1"
Get-WmiObject Win32_Service | Select Name, DisplayName, State, PathName, ProcessId
```

👉 এতে full mapping পাওয়া যায়:

* Service
* EXE path
* PID

---

# 🚨 Malware Service Indicators

যদি এগুলা থাকে → high suspicion:

* Random name service
* Auto start + unknown path
* Hidden from services.msc
* High CPU without reason
* Unknown outbound network connection
* Antivirus disable attempt

---

# 🛠️ Tools for Deep Detection

* Process Explorer → detailed process tree
* Autoruns → hidden startup services
* Malwarebytes → malware scan

---

# 🧾 Practical Example (Audit Scenario)

👉 তুমি দেখলে:

Service: `UpdateServiceX`
Path: `C:\Users\Zahid\AppData\Local\temp\update.exe`

👉 Analysis:

* Temp folder ❌
* Unknown service ❌
* Auto run ❌

👉 Conclusion: Suspicious / Malware possible ⚠️

---

# 🔐 IT Audit Checklist (Services Section)

Audit করার সময় check করবে:

* All running services documented?
* Unknown services identified?
* Service path verified?
* Startup type justified?
* Network activity monitored?

---

# ⚠️ Important Warning

❌ Unknown service delete immediately করো না
👉 আগে:

* Verify
* Scan
* Backup evidence

---

# 🚀 Quick Workflow (Real Life)

1. `sc query` → services list
2. `sc qc` → path check
3. `sc queryex` → PID
4. `tasklist` → process verify
5. `netstat` → network check
6. File location → verify legitimacy

---

👉 চাইলে আমি তোমাকে real case দিয়ে train করাতে পারি:

✔ “malware service analysis lab”
✔ suspicious process detect practice
✔ IT audit report (services section)

বললেই hands-on training শুরু করি 😎

নিচে আমি তোমার জন্য Windows **services + security + process + malware checking** এর সব দরকারি CMD commands এক জায়গায় **organized list** করে দিলাম 📋
তুমি এগুলো copy করে রাখতে পারো (IT audit / troubleshooting / security checking এ কাজে লাগবে) 🔥

---

# 🧾 🔹 1. Services Related Commands

```cmd
sc query
```

👉 সব services list (running + stopped)

```cmd
sc query state= running
```

👉 শুধু running services

```cmd
sc query state= stopped
```

👉 stopped services

```cmd
sc queryex ServiceName
```

👉 service details + PID

```cmd
sc qc ServiceName
```

👉 configuration details (path, start type)

```cmd
net start
```

👉 currently running services list (simple view)

```cmd
net stop ServiceName
```

👉 service stop করা

---

# 🧾 🔹 2. Process Related Commands

```cmd
tasklist
```

👉 সব running processes

```cmd
tasklist /fi "imagename eq chrome.exe"
```

👉 specific process filter

```cmd
tasklist /fi "PID eq 1234"
```

👉 PID দিয়ে process find

```cmd
taskkill /PID 1234 /F
```

👉 process force kill

```cmd
wmic process list brief
```

👉 processes with brief info

---

# 🧾 🔹 3. Service ↔ Process Mapping

```cmd
sc queryex ServiceName
```

👉 PID বের করবে

```cmd
tasklist /fi "PID eq 1234"
```

👉 কোন exe run করছে

---

# 🧾 🔹 4. Network / Connection Check

```cmd
netstat -ano
```

👉 all active connections + PID

```cmd
netstat -ab
```

👉 which app using which port (admin required)

```cmd
netstat -ano | findstr :80
```

👉 specific port check

---

# 🧾 🔹 5. System Information

```cmd
systeminfo
```

👉 full system info

```cmd
whoami
```

👉 current user

```cmd
hostname
```

👉 computer name

---

# 🧾 🔹 6. Startup / Autorun Check

```cmd
wmic startup get caption,command
```

👉 startup programs list

---

# 🧾 🔹 7. Disk / File Check

```cmd
dir
```

👉 directory list

```cmd
cd foldername
```

👉 folder change

```cmd
attrib
```

👉 file attributes (hidden/system)

---

# 🧾 🔹 8. Firewall Related

```cmd
netsh advfirewall show allprofiles
```

👉 firewall status

```cmd
netsh advfirewall firewall show rule name=all
```

👉 all firewall rules

---

# 🧾 🔹 9. User / Security

```cmd
net user
```

👉 all users

```cmd
net user username
```

👉 specific user details

```cmd
whoami /priv
```

👉 user privileges

---

# 🧾 🔹 10. Malware / Suspicious Check Commands

```cmd
netstat -ano
```

👉 suspicious connections

```cmd
tasklist
```

👉 unknown process detect

```cmd
sc query state= running
```

👉 unknown service detect

```cmd
wmic process get name,processid,executablepath
```

👉 process + file location

---

# 🔥 Quick IT Audit Combo (Most Useful)

👉 এই 5টা command daily use করো:

```cmd
sc query state= running
tasklist
netstat -ano
wmic process get name,processid,executablepath
sc qc ServiceName
```

---

# ⚠️ Pro Tips

* Unknown service name → investigate
* Temp/AppData path → suspicious
* High CPU + unknown process → malware possibility
* Random outbound connection → alert

---

👉 চাইলে আমি তোমার জন্য
✔ “1-page cheat sheet PDF style”
✔ Real audit workflow checklist
✔ Malware detection practice lab

সব structured করে দিতে পারি 😎
