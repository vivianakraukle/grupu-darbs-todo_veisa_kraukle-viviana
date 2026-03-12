<!DOCTYPE html>
<html lang="lv">

<head>
  <meta charset="UTF-8">
  <title>Mani darbi</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>

<body>
  <div class="container">
    <h3 class="text-center mb-3">Mani darbi</h3>
    <div class="card"></div>
    <div class="input-group mb-3">
      <select class="form-select" id="prioritySelect">
        <option value="zema">Zema</option>
        <option value="vidēji">Vidēji</option>
        <option value="svarīgi">Svarīgi</option>
      </select>
      <div class="card-body">
        <input type="text" class="form-control" placeholder=" Ieraksti darbu" id="teksts" name="etxt">
      </div>
      <button class="btn btn-dark" onclick="addTask()">Pievienot</button>
    </div>
    <ul id="task-list" class="list-group"></ul>
  </div>

  <script>
    function addTask(textFromStorage, priorityFromStorage) {
      let input = document.getElementById("teksts");
      let list = document.getElementById("task-list");
      let select = document.getElementById("prioritySelect");
      let text = textFromStorage ? textFromStorage : input.value.trim();
      let priority = priorityFromStorage ? priorityFromStorage : select.value;

      if (text === "") return;

      let li = document.createElement("li");
      li.className = "list-group-item d-flex justify-content-between align-items-center";

      let span = document.createElement("span");
      span.innerHTML = text;
      span.style.cursor = "pointer";
      span.onclick = function() {
        span.classList.toggle("text-decoration-line-through");
        saveTasks();
      };

      let badge = document.createElement("span");
      if (priority === "svarīgi") {
        badge.className = "badge bg-danger me-2";
        badge.innerHTML = "Svarīgi";
      }
      if (priority === "vidēji") {
        badge.className = "badge bg-warning text-dark me-2";
        badge.innerHTML = "Vidēji";
      }
      if (priority === "zema") {
        badge.className = "badge bg-secondary me-2";
        badge.innerHTML = "Zema";
      }

      let left = document.createElement("div");
      left.appendChild(badge);
      left.appendChild(span);

      let deleteBtn = document.createElement("button");
      deleteBtn.className = "btn btn-dark btn-sm";
      deleteBtn.innerHTML = "Dzēst";
      deleteBtn.onclick = function() {
        li.remove();
        saveTasks();
      };

      li.appendChild(left);
      li.appendChild(deleteBtn)
      list.appendChild(li);

      input.value = "";
      saveTasks();
    }

    function saveTasks() {
      let tasks = [];
      let items = document.querySelectorAll("#task-list li");
      items.forEach(function(li) {
        let text = li.querySelector("span:last-child").innerText;
        let priority = li.querySelector(".badge").innerText;
        let done = li.querySelector("span:last-child").classList.contains("text-decoration-line-through");
        tasks.push({
          text: text,
          priority: priority,
          done: done
        });
      });
      localStorage.setItem("uzdevumi", JSON.stringify(tasks));
    }

    function loadTasks() {
      let tasks = JSON.parse(localStorage.getItem("uzdevumi")) || [];
      tasks.forEach(function(task) {
        let priority = "zema";
        if (task.priority === "Vidēji") priority = "vidēji";
        if (task.priority === "Svarīgi") priority = "svarīgi";
        addTask(task.text, priority);
        let last = document.querySelector("#task-list li:last-child span:last-child");
        if (task.done) {
          last.classList.add("text-decoration-line-through");
        }
      });
    }

    loadTasks();
  </script>
</body>

</html>
