<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spis Pracowników</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #F3F6FB;
            margin: 0;
            padding: 0;
            color: #333;
            width: 100%;
            min-height: 100vh;
        }

        .container {
            max-width: 90%;
            margin: 0 auto;
            padding: 40px 20px;
        }

        .header {
            text-align: center;
            background-color: #4A90E2;
            color: white;
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 40px;
        }

        .header h1 {
            margin: 0;
            font-size: 36px;
        }

        .add-button {
            margin-top: 20px;
            padding: 12px 24px;
            background-color: #57A6E8;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 18px;
            cursor: pointer;
            text-decoration: none;
            transition: background-color 0.3s ease, box-shadow 0.3s ease;
        }

        .add-button:hover {
            background-color: #4092c9;
        }

        .employee-list {
            width: 150%;
            border-collapse: collapse;
            margin-top: 30px;
            font-size: 18px; /* Dwukrotne zwiększenie rozmiaru */
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
        }

        .employee-list th, .employee-list td {
            padding: 50px; /* Większa przestrzeń w komórkach */
            text-align: center;
            border: 3px solid #4A90E2; /* Grubsze linie między wierszami */
        }

        .employee-list th {
            background-color: #4A90E2;
            color: white;
            font-weight: bold;
        }

        .employee-list td {
            background-color: #FFF;
            transition: background-color 0.3s ease;
        }

        .employee-list tr:hover td {
            background-color: #EAF4FF; /* Efekt hover na wierszach */
        }

        .remove-button {
            padding: 10px 20px;
            background-color: #E74C3C;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .remove-button:hover {
            background-color: #C0392B;
        }

        .status-clickable {
            color: #4A90E2;
            cursor: pointer;
            text-decoration: underline;
        }

        .overlay, .modal {
            display: none;
            justify-content: center;
            align-items: center;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 999;
        }

        .modal-content {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            width: 400px;
            text-align: center;
        }

        .modal-input {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
        }

        .modal-button {
            padding: 10px 20px;
            background-color: #4A90E2;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .modal-button:hover {
            background-color: #357ab7;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Spis Pracowników</h1>
            <button class="add-button" onclick="openAddEmployeeModal()">Dodaj Pracownika</button>
        </div>

        <table class="employee-list" id="employeeList">
            <thead>
                <tr>
                    <th>Imię i Nazwisko</th>
                    <th>SSN</th>
                    <th>Discord ID</th>
                    <th>Pracuje</th>
                    <th>Akcja</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>

    <div class="overlay" id="addEmployeeOverlay" onclick="closeModal()"></div>
    <div class="modal" id="addEmployeeModal">
        <div class="modal-content">
            <h3>Dodaj Pracownika</h3>
            <input type="text" id="employeeName" class="modal-input" placeholder="Imię i Nazwisko">
            <input type="text" id="employeeSSN" class="modal-input" placeholder="SSN">
            <input type="text" id="employeeDiscord" class="modal-input" placeholder="Discord ID">
            <button class="modal-button" onclick="addEmployee()">Dodaj</button>
        </div>
    </div>

    <div class="overlay" id="changeStatusOverlay" onclick="closeStatusModal()"></div>
    <div class="modal" id="changeStatusModal">
        <div class="modal-content">
            <h3>Zmień status pracy</h3>
            <select id="statusSelect" class="modal-input">
                <option value="Tak">Tak</option>
                <option value="Nie">Nie</option>
            </select>
            <button class="modal-button" onclick="changeEmployeeStatus()">Zmień status</button>
        </div>
    </div>

    <script>
        let employees = [
            { name: "Jan Kowalski", ssn: "123-45-6789", discord: "jan.kowalski#1234", works: "Tak" }
        ];

        function renderEmployees() {
            const employeeList = document.getElementById("employeeList").getElementsByTagName("tbody")[0];
            employeeList.innerHTML = "";
            employees.forEach((employee, index) => {
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td>${employee.name}</td>
                    <td>${employee.ssn}</td>
                    <td>${employee.discord}</td>
                    <td><span class="status-clickable" onclick="openChangeStatusModal(${index})">${employee.works}</span></td>
                    <td><button class="remove-button" onclick="removeEmployee(${index})">Usuń</button></td>
                `;
                employeeList.appendChild(row);
            });
        }

        function openAddEmployeeModal() {
            document.getElementById("addEmployeeModal").style.display = "flex";
            document.getElementById("addEmployeeOverlay").style.display = "flex";
        }

        function closeModal() {
            document.getElementById("addEmployeeModal").style.display = "none";
            document.getElementById("addEmployeeOverlay").style.display = "none";
        }

        function openChangeStatusModal(index) {
            document.getElementById("changeStatusModal").style.display = "flex";
            document.getElementById("changeStatusOverlay").style.display = "flex";
            document.getElementById("statusSelect").value = employees[index].works;
            document.getElementById("statusSelect").setAttribute("data-index", index);
        }

        function closeStatusModal() {
            document.getElementById("changeStatusModal").style.display = "none";
            document.getElementById("changeStatusOverlay").style.display = "none";
        }

        function addEmployee() {
            const name = document.getElementById("employeeName").value;
            const ssn = document.getElementById("employeeSSN").value;
            const discord = document.getElementById("employeeDiscord").value;

            if (name && ssn && discord) {
                employees.push({ name, ssn, discord, works: "Tak" });
                renderEmployees();
                closeModal();
            }
        }

        function removeEmployee(index) {
            employees.splice(index, 1);
            renderEmployees();
        }

        function changeEmployeeStatus() {
            const index = document.getElementById("statusSelect").getAttribute("data-index");
            const newStatus = document.getElementById("statusSelect").value;
            employees[index].works = newStatus;
            renderEmployees();
            closeStatusModal();
        }

        renderEmployees();
    </script>
</body>
</html>
