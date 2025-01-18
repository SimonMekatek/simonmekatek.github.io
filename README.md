# Department of Justice - Kartoteka pracowników

<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spis Pracowników</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 0;
            color: #333;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            padding: 20px;
            background-color: #4A90E2;
            color: white;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .header h1 {
            margin: 0;
            font-size: 36px;
            font-weight: 600;
        }

        .add-button {
            padding: 10px 20px;
            background-color: #4A90E2;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 18px;
            cursor: pointer;
            text-decoration: none;
            transition: background-color 0.3s ease;
        }

        .add-button:hover {
            background-color: #357ab7;
        }

        .employee-list {
            margin-top: 40px;
            width: 100%;
            border-collapse: collapse;
        }

        .employee-list th, .employee-list td {
            padding: 12px;
            text-align: center;
            border: 1px solid #ddd;
        }

        .employee-list th {
            background-color: #4A90E2;
            color: white;
        }

        .employee-list td {
            background-color: #ffffff;
        }

        .remove-button {
            padding: 5px 10px;
            background-color: #e74c3c;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .remove-button:hover {
            background-color: #c0392b;
        }

        .pencil-icon {
            cursor: pointer;
            color: #f39c12;
            font-size: 20px;
        }

        .pencil-icon:hover {
            color: #e67e22;
        }

        .modal, .overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            width: 400px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        .modal-header {
            text-align: center;
            margin-bottom: 20px;
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
            font-size: 16px;
            width: 100%;
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
            <tbody>
                <!-- Pracownicy zostaną dodani tutaj dynamicznie -->
            </tbody>
        </table>
    </div>

    <!-- Modal Dodawania Pracownika -->
    <div class="overlay" id="addEmployeeOverlay" onclick="closeModal()"></div>
    <div class="modal" id="addEmployeeModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Dodaj Pracownika</h3>
            </div>
            <input type="text" id="employeeName" class="modal-input" placeholder="Imię i Nazwisko" />
            <input type="text" id="employeeSSN" class="modal-input" placeholder="SSN" />
            <input type="text" id="employeeDiscord" class="modal-input" placeholder="Discord ID" />
            <button class="modal-button" onclick="addEmployee()">Dodaj</button>
        </div>
    </div>

    <!-- Modal Zmiany Stanu Pracownika -->
    <div class="overlay" id="changeStatusOverlay" onclick="closeStatusModal()"></div>
    <div class="modal" id="changeStatusModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Zmień status pracy</h3>
            </div>
            <select id="statusSelect" class="modal-input">
                <option value="Tak">Tak</option>
                <option value="Nie">Nie</option>
            </select>
            <button class="modal-button" onclick="changeEmployeeStatus()">Zmień status</button>
        </div>
    </div>

    <script>
        let employees = [
            { name: "Jan Kowalski", ssn: "123-45-6789", discord: "jan.kowalski#1234", works: "Tak" },
            { name: "Agnieszka Nowak", ssn: "987-65-4321", discord: "agnieszka.nowak#5678", works: "Nie" },
            { name: "Piotr Wiśniewski", ssn: "456-78-1234", discord: "piotr.wisniewski#91011", works: "Tak" },
        ];

        function renderEmployees() {
            const employeeList = document.getElementById("employeeList").getElementsByTagName("tbody")[0];
            employeeList.innerHTML = '';
            employees.forEach((employee, index) => {
                const row = document.createElement("tr");
                row.innerHTML = `
                    <td>${employee.name}</td>
                    <td>${employee.ssn}</td>
                    <td>${employee.discord}</td>
                    <td>
                        <span id="workStatus-${index}">${employee.works}</span>
                        <span class="pencil-icon" onclick="openChangeStatusModal(${index})">&#9998;</span>
                    </td>
                    <td><button class="remove-button" onclick="removeEmployee(${index})">Usuń</button></td>
                `;
                employeeList.appendChild(row);
            });
        }

        function openAddEmployeeModal() {
            document.getElementById("addEmployeeModal").style.display = 'block';
            document.getElementById("addEmployeeOverlay").style.display = 'block';
        }

        function closeModal() {
            document.getElementById("addEmployeeModal").style.display = 'none';
            document.getElementById("addEmployeeOverlay").style.display = 'none';
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

        function openChangeStatusModal(index) {
            document.getElementById("changeStatusModal").style.display = 'block';
            document.getElementById("changeStatusOverlay").style.display = 'block';
            document.getElementById("statusSelect").value = employees[index].works;
            document.getElementById("statusSelect").setAttribute("data-index", index);
        }

        function closeStatusModal() {
            document.getElementById("changeStatusModal").style.display = 'none';
            document.getElementById("changeStatusOverlay").style.display = 'none';
        }

        function changeEmployeeStatus() {
            const index = document.getElementById("statusSelect").getAttribute("data-index");
            const newStatus = document.getElementById("statusSelect").value;
            employees[index].works = newStatus;
            document.getElementById(`workStatus-${index}`).textContent = newStatus;
            closeStatusModal();
        }

        // Initialize the page
        renderEmployees();
    </script>
</body>
</html>
