<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Attendance Management System</title>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #6366f1;
            --accent: #00f2ff;
            --bg: #030712;
            --glass: rgba(255, 255, 255, 0.03);
            --border: rgba(255, 255, 255, 0.1);
            --danger: #ff4757;
            --success: #10b981;
        }

        * { box-sizing: border-box; transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }

        body {
            margin: 0;
            font-family: 'Plus Jakarta Sans', sans-serif;
            background: var(--bg);
            color: white;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            background-image: 
                radial-gradient(at 0% 0%, rgba(99, 102, 241, 0.15) 0px, transparent 50%),
                radial-gradient(at 100% 100%, rgba(0, 242, 255, 0.1) 0px, transparent 50%);
        }

        .container { width: 100%; max-width: 500px; padding: 30px 20px; }

        header { margin-bottom: 30px; text-align: center; }
        header h1 { 
            margin: 0; font-weight: 800; font-size: 1.8rem; letter-spacing: -1px; 
            line-height: 1.2;
            background: linear-gradient(to right, #fff, var(--primary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .date-display { color: #64748b; font-size: 0.85rem; font-weight: 600; margin-top: 10px; }

        .stats-row { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-bottom: 30px; }
        .stat-card { 
            background: var(--glass); 
            border: 1px solid var(--border); 
            padding: 15px 10px; 
            border-radius: 20px; 
            backdrop-filter: blur(10px);
            text-align: center;
        }
        .stat-card small { color: #64748b; font-size: 0.6rem; text-transform: uppercase; font-weight: 800; display: block; margin-bottom: 5px; }
        .stat-card div { font-size: 1.4rem; font-weight: 800; }

        .card { 
            background: var(--glass); 
            border: 1px solid var(--border); 
            padding: 25px; 
            border-radius: 24px; 
            backdrop-filter: blur(20px);
            margin-bottom: 25px;
        }
        h3 { margin: 0 0 20px 0; font-size: 0.85rem; color: #94a3b8; text-transform: uppercase; letter-spacing: 1px; }

        input, select {
            width: 100%; background: rgba(0,0,0,0.3); border: 1px solid var(--border);
            padding: 14px 16px; border-radius: 14px; color: white; margin-bottom: 12px; outline: none;
            font-size: 0.95rem;
        }
        input::placeholder { color: #475569; }
        input:focus { border-color: var(--primary); background: rgba(0,0,0,0.5); box-shadow: 0 0 15px rgba(99, 102, 241, 0.2); }

        /* Search Bar Specific Style */
        .search-container {
            margin-bottom: 20px;
        }
        .search-bar {
            border-radius: 20px;
            padding: 12px 20px;
            border: 1px solid var(--accent);
            background: rgba(0, 242, 255, 0.05);
        }

        .btn {
            width: 100%; padding: 15px; border-radius: 14px; border: none; font-weight: 700;
            cursor: pointer; background: var(--primary); color: white; font-size: 1rem;
            box-shadow: 0 4px 15px rgba(99, 102, 241, 0.3);
        }
        .btn:active { transform: scale(0.96); }

        .student-item {
            background: var(--glass); border: 1px solid var(--border); padding: 18px;
            border-radius: 20px; display: flex; justify-content: space-between; align-items: center;
            margin-bottom: 12px;
        }
        .s-info h4 { margin: 0; font-size: 1.1rem; }
        .s-info small { color: #64748b; font-family: monospace; }
        
        .liquid-battery {
            width: 45px; height: 45px; border-radius: 12px; position: relative;
            overflow: hidden; background: #000; border: 1px solid var(--border);
        }
        .liquid-fill {
            position: absolute; bottom: 0; left: 0; width: 100%;
            background: linear-gradient(to top, var(--primary), var(--accent));
            height: var(--fill, 0%);
            box-shadow: 0 0 15px var(--primary);
        }

        .delete-btn { color: #475569; background: none; border: none; cursor: pointer; font-size: 0.75rem; font-weight: bold; margin-top: 5px; padding: 0;}
        .delete-btn:hover { color: var(--danger); }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>STUDENT ATTENDANCE<br>MANAGEMENT SYSTEM</h1>
        <div class="date-display" id="dateText"></div>
    </header>

    <div class="stats-row">
        <div class="stat-card"><small>Total Students</small><div id="statTotal">0</div></div>
        <div class="stat-card"><small>Avg. Attendance</small><div id="statAvg">0%</div></div>
        <div class="stat-card"><small>Marked Today</small><div id="statToday">0</div></div>
    </div>

    <div class="card">
        <h3>Add New Student</h3>
        <input id="roll" type="text" placeholder="Enter Roll Number">
        <input id="name" type="text" placeholder="Full Student Name">
        <button class="btn" onclick="addStudent()">Add to Registry</button>
    </div>

    <div class="card">
        <h3>Daily Attendance Log</h3>
        <div style="display:flex; gap:10px;">
            <input id="markRoll" placeholder="Roll No" style="flex:1">
            <input type="date" id="dateInput" style="flex:1.5">
        </div>
        <select id="status">
            <option value="Present">Present</option>
            <option value="Absent">Absent</option>
        </select>
        <button class="btn" style="background: rgba(255,255,255,0.05); color: white; border: 1px solid var(--border); box-shadow: none;" onclick="markAttendance()">Log Entry</button>
    </div>

    <div class="search-container">
        <input type="text" id="searchInput" class="search-bar" placeholder="🔍 Search by Name or Roll No..." onkeyup="render()">
    </div>

    <h3 style="padding-left: 10px;">Directory (A-Z)</h3>
    <div id="studentList"></div>
</div>

<script>
    let students = JSON.parse(localStorage.getItem("aura_pro_db")) || [];
    
    // Auto-set Date
    const today = new Date();
    document.getElementById('dateInput').valueAsDate = today;
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    document.getElementById('dateText').innerText = today.toLocaleDateString('en-US', options);

    function addStudent() {
        const roll = document.getElementById("roll").value.trim();
        const name = document.getElementById("name").value.trim();
        if(!roll || !name) return alert("Please fill all fields");
        if(students.some(s => s.roll === roll)) return alert("Roll number already exists");

        students.push({ roll, name, p: 0, a: 0, logs: {} });
        saveAndRender();
        document.getElementById("roll").value = "";
        document.getElementById("name").value = "";
    }

    function markAttendance() {
        const roll = document.getElementById("markRoll").value.trim();
        const date = document.getElementById("dateInput").value;
        const status = document.getElementById("status").value;
        const student = students.find(s => s.roll === roll);

        if(!student) return alert("Student not found");
        if(student.logs[date]) return alert("Attendance already marked for this date");

        student.logs[date] = status;
        status === "Present" ? student.p++ : student.a++;
        saveAndRender();
        document.getElementById("markRoll").value = "";
    }

    function saveAndRender() {
        students.sort((a, b) => a.name.localeCompare(b.name));
        localStorage.setItem("aura_pro_db", JSON.stringify(students));
        render();
    }

    function render() {
        const list = document.getElementById("studentList");
        const todayDate = document.getElementById("dateInput").value;
        const searchTerm = document.getElementById("searchInput").value.toLowerCase();
        list.innerHTML = "";
        
        let totalLogs = 0, totalP = 0, markedToday = 0;

        // Filter students based on search
        const filteredStudents = students.filter(s => 
            s.name.toLowerCase().includes(searchTerm) || 
            s.roll.toLowerCase().includes(searchTerm)
        );

        students.forEach(s => {
            const total = s.p + s.a;
            totalLogs += total;
            totalP += s.p;
            if(s.logs[todayDate]) markedToday++;
        });

        filteredStudents.forEach(s => {
            const total = s.p + s.a;
            const perc = total ? Math.round((s.p / total) * 100) : 0;

            list.innerHTML += `
                <div class="student-item">
                    <div class="s-info">
                        <h4>${s.name}</h4>
                        <small>Roll: ${s.roll} • ${perc}% Success</small>
                        <br><button class="delete-btn" onclick="remove('${s.roll}')">REMOVE RECORD</button>
                    </div>
                    <div class="liquid-battery">
                        <div class="liquid-fill" style="--fill: ${perc}%"></div>
                    </div>
                </div>
            `;
        });

        document.getElementById("statTotal").innerText = students.length;
        document.getElementById("statAvg").innerText = totalLogs ? Math.round((totalP / totalLogs) * 100) + "%" : "0%";
        document.getElementById("statToday").innerText = markedToday;
    }

    function remove(roll) {
        if(confirm("Are you sure you want to delete this record?")) {
            students = students.filter(s => s.roll !== roll);
            saveAndRender();
        }
    }

    render();
</script>

</body>
</html>
