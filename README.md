<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>CCC Members Portal</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: var(--bg);
      color: var(--text);
      transition: 0.3s;
    }
    :root {
      --bg: #f0f2f5;
      --text: #000;
      --card: #fff;
    }
    .dark {
      --bg: #1e1e1e;
      --text: #eee;
      --card: #2e2e2e;
    }
    .container {
      max-width: 700px;
      margin: 50px auto;
      padding: 20px;
      background: var(--card);
      border-radius: 10px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.2);
    }
    h2, h3 {
      text-align: center;
    }
    input {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      padding: 10px 20px;
      margin: 5px 0;
      border-radius: 5px;
      border: none;
      background: #4CAF50;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }
    button:hover {
      background: #45a049;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      background: #e2e2e2;
      margin: 5px 0;
      padding: 10px;
      border-radius: 5px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .section {
      margin-bottom: 30px;
    }
    .dashboard {
      display: none;
    }
    .calendar table {
      width: 100%;
      border-collapse: collapse;
    }
    .calendar th, .calendar td {
      border: 1px solid #ddd;
      padding: 8px;
    }
    .calendar th {
      background-color: #ccc;
    }
    .today {
      background-color: #8ef58e;
    }
    label {
      font-weight: bold;
    }
  </style>
</head>
<body>

<div class="container" id="loginPage">
  <h2>CCC Member Login</h2>
  <input type="text" id="username" placeholder="Username">
  <input type="password" id="password" placeholder="Password">
  <button onclick="login()">Login</button>
</div>

<div class="container dashboard" id="dashboard">
  <h2>Welcome to CCC Portal</h2>

  <div style="text-align: center; margin-bottom: 20px;">
    <button onclick="toggleTheme()">Toggle Dark/Light Mode</button>
  </div>

  <!-- User Profile -->
  <div class="section" id="userProfileSection">
    <h3>User Profile</h3>
    <p><strong>Name:</strong> <span id="profileName"></span></p>
    <p><strong>Branch:</strong> <span id="profileBranch"></span></p>
    <p><strong>Position:</strong> <span id="profilePosition"></span></p>
  </div>

  <!-- Events -->
  <div class="section" id="events">
    <h3>Upcoming Events</h3>
    <input type="text" id="eventInput" placeholder="Add new event (e.g. Meeting - 25 Sept)">
    <button onclick="addEvent()">Add Event</button>
    <ul id="eventList"></ul>
  </div>

  <!-- Class Tasks -->
  <div class="section" id="classTasks">
    <h3>Class Tasks</h3>
    <input type="text" id="classTaskInput" placeholder="Add new class task">
    <button onclick="addClassTask()">Add Task</button>
    <ul id="classTaskList"></ul>
  </div>

  <!-- Personal To-Do -->
  <div class="section" id="tasks">
    <h3>My To-Do List</h3>
    <input type="text" id="todoInput" placeholder="Enter your task">
    <button onclick="addTodo()">Add Task</button>
    <ul id="todoList"></ul>
  </div>

  <!-- Calendar -->
  <div class="section calendar">
    <h3>Calendar</h3>
    <button id="prevMonth">Previous</button>
    <span id="monthYear" style="font-weight: bold; margin: 0 10px;"></span>
    <button id="nextMonth">Next</button>
    <table>
      <thead>
        <tr>
          <th>Sun</th><th>Mon</th><th>Tue</th><th>Wed</th>
          <th>Thu</th><th>Fri</th><th>Sat</th>
        </tr>
      </thead>
      <tbody id="calendarBody"></tbody>
    </table>
  </div>

  <button onclick="logout()">Logout</button>
</div>

<script>
  const members = [
    { username: "kavi", password: "cccmemCEO/HS15", name: "G. Kavi Siva Murugan", branch: "Overall", position: "Head Supervisor / CEO / Founder" },
    { username: "tharshan", password: "cccmemAS11", name: "Tharshan Sai", branch: "Academics", position: "Academic Supervisor & Secret Spy" },
    { username: "kavinesh", password: "cccmemCS08", name: "Kavinesh", branch: "Cleanliness", position: "Cleanliness Supervisor" },
    { username: "aditya", password: "cccmemDS06", name: "Aditya", branch: "Discipline", position: "Disciplinary Supervisor" },
    { username: "arunagiri", password: "cccmemADS09", name: "Arunagiri", branch: "Discipline", position: "Asst. Disciplinary Supervisor" },
    { username: "santhik", password: "cccmemCMD07", name: "Santhick", branch: "Cleanliness", position: "Cleanliness Managing Director" },
    { username: "allen", password: "cccmemCLL05", name: "Allen", branch: "Cleanliness", position: "Labour Leader" },
    { username: "sriram", password: "cccmemCW06", name: "Sri Ram", branch: "Cleanliness", position: "Worker" },
    { username: "thaswin", password: "cccmemCW07", name: "Thaswin", branch: "Cleanliness", position: "Worker" }
  ];

  let currentUser = null;

  function login() {
    const uname = document.getElementById("username").value.trim();
    const pword = document.getElementById("password").value.trim();
    const user = members.find(u => u.username === uname && u.password === pword);

    if (user) {
      currentUser = uname;
      document.getElementById("loginPage").style.display = "none";
      document.getElementById("dashboard").style.display = "block";
      document.getElementById("profileName").textContent = user.name;
      document.getElementById("profileBranch").textContent = user.branch;
      document.getElementById("profilePosition").textContent = user.position;
      loadTodoList();
      loadEventList();
      loadClassTaskList();
      renderCalendar(currentYear, currentMonth);
      applyTheme();
    } else {
      alert("Invalid login.");
    }
  }

  function logout() {
    currentUser = null;
    document.getElementById("loginPage").style.display = "block";
    document.getElementById("dashboard").style.display = "none";
    document.getElementById("username").value = "";
    document.getElementById("password").value = "";
  }

  // --------- Dark/Light Mode ----------
  function toggleTheme() {
    const isDark = document.body.classList.toggle("dark");
    localStorage.setItem("ccc-theme", isDark ? "dark" : "light");
  }

  function applyTheme() {
    const theme = localStorage.getItem("ccc-theme");
    if (theme === "dark") {
      document.body.classList.add("dark");
    } else {
      document.body.classList.remove("dark");
    }
  }

  // --------- ToDo / Events / Tasks ---------
  function addTodo() {
    const input = document.getElementById("todoInput");
    const text = input.value.trim();
    if (text !== "" && currentUser) {
      let todos = JSON.parse(localStorage.getItem(`todo-${currentUser}`)) || [];
      todos.push({ text, done: false });
      localStorage.setItem(`todo-${currentUser}`, JSON.stringify(todos));
      input.value = "";
      loadTodoList();
    }
  }

  function loadTodoList() {
    const list = document.getElementById("todoList");
    list.innerHTML = "";
    let todos = JSON.parse(localStorage.getItem(`todo-${currentUser}`)) || [];

    todos.forEach((item, index) => {
      const li = document.createElement("li");
      li.textContent = item.text;
      if (item.done) li.style.textDecoration = "line-through";

      li.onclick = () => {
        todos[index].done = !todos[index].done;
        localStorage.setItem(`todo-${currentUser}`, JSON.stringify(todos));
        loadTodoList();
      };

      const delBtn = document.createElement("button");
      delBtn.textContent = "Delete";
      delBtn.onclick = (e) => {
        e.stopPropagation();
        todos.splice(index, 1);
        localStorage.setItem(`todo-${currentUser}`, JSON.stringify(todos));
        loadTodoList();
      };

      li.appendChild(delBtn);
      list.appendChild(li);
    });
  }

  function addEvent() {
    const input = document.getElementById("eventInput");
    const text = input.value.trim();
    if (text !== "" && currentUser) {
      let events = JSON.parse(localStorage.getItem(`events-${currentUser}`)) || [];
      events.push({ text, done: false });
      localStorage.setItem(`events-${currentUser}`, JSON.stringify(events));
      input.value = "";
      loadEventList();
    }
  }

  function loadEventList() {
    const list = document.getElementById("eventList");
    list.innerHTML = "";
    let events = JSON.parse(localStorage.getItem(`events-${currentUser}`)) || [];

    events.forEach((item, index) => {
      const li = document.createElement("li");
      li.textContent = item.text;
      if (item.done) li.style.textDecoration = "line-through";

      li.onclick = () => {
        events[index].done = !events[index].done;
        localStorage.setItem(`events-${currentUser}`, JSON.stringify(events));
        loadEventList();
      };

      const delBtn = document.createElement("button");
      delBtn.textContent = "Delete";
      delBtn.onclick = (e) => {
        e.stopPropagation();
        events.splice(index, 1);
        localStorage.setItem(`events-${currentUser}`, JSON.stringify(events));
        loadEventList();
      };

      li.appendChild(delBtn);
      list.appendChild(li);
    });
  }

  function addClassTask() {
    const input = document.getElementById("classTaskInput");
    const text = input.value.trim();
    if (text !== "" && currentUser) {
      let tasks = JSON.parse(localStorage.getItem(`classTasks-${currentUser}`)) || [];
      tasks.push({ text, done: false });
      localStorage.setItem(`classTasks-${currentUser}`, JSON.stringify(tasks));
      input.value = "";
      loadClassTaskList();
    }
  }

  function loadClassTaskList() {
    const list = document.getElementById("classTaskList");
    list.innerHTML = "";
    let tasks = JSON.parse(localStorage.getItem(`classTasks-${currentUser}`)) || [];

    tasks.forEach((item, index) => {
      const li = document.createElement("li");
      li.textContent = item.text;
      if (item.done) li.style.textDecoration = "line-through";

      li.onclick = () => {
        tasks[index].done = !tasks[index].done;
        localStorage.setItem(`classTasks-${currentUser}`, JSON.stringify(tasks));
        loadClassTaskList();
      };

      const delBtn = document.createElement("button");
      delBtn.textContent = "Delete";
      delBtn.onclick = (e) => {
        e.stopPropagation();
        tasks.splice(index, 1);
        localStorage.setItem(`classTasks-${currentUser}`, JSON.stringify(tasks));
        loadClassTaskList();
      };

      li.appendChild(delBtn);
      list.appendChild(li);
    });
  }

  // --------- Calendar ----------
  const calendarBody = document.getElementById("calendarBody");
  const monthYear = document.getElementById("monthYear");
  let currentDate = new Date();
  let currentMonth = currentDate.getMonth();
  let currentYear = currentDate.getFullYear();

  function renderCalendar(year, month) {
    const daysInMonth = new Date(year, month + 1, 0).getDate();
    const firstDay = new Date(year, month, 1).getDay();
    const today = new Date();

    calendarBody.innerHTML = "";
    monthYear.textContent = `${new Date(year, month).toLocaleString("default", { month: "long" })} ${year}`;

    let row = document.createElement("tr");
    for (let i = 0; i < firstDay; i++) {
      row.appendChild(document.createElement("td"));
    }

    for (let day = 1; day <= daysInMonth; day++) {
      if ((firstDay + day - 1) % 7 === 0) {
        calendarBody.appendChild(row);
        row = document.createElement("tr");
      }

      const cell = document.createElement("td");
      cell.textContent = day;

      if (
        year === today.getFullYear() &&
        month === today.getMonth() &&
        day === today.getDate()
      ) {
        cell.classList.add("today");
      }

      row.appendChild(cell);
    }

    calendarBody.appendChild(row);
  }

  document.getElementById("prevMonth").addEventListener("click", () => {
    currentMonth--;
    if (currentMonth < 0) {
      currentMonth = 11;
      currentYear--;
    }
    renderCalendar(currentYear, currentMonth);
  });

  document.getElementById("nextMonth").addEventListener("click", () => {
    currentMonth++;
    if (currentMonth > 11) {
      currentMonth = 0;
      currentYear++;
    }
    renderCalendar(currentYear, currentMonth);
  });

  window.onload = applyTheme;
</script>

</body>
</html>
