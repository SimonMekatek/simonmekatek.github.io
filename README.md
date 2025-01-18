<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spis Pracowników</title>
    <!-- Załadowanie czcionki Poppins z Google Fonts -->
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
            overflow-x: hidden;
        }

        .container {
            width: 100%;
            max-width: 90%;
            margin: 0 auto;
            padding: 40px 20px;
            box-sizing: border-box;
        }

        .header {
            text-align: center;
            padding: 30px 40px;
            background-color: #4A90E2;
            color: white;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            margin-bottom: 40px;
            width: 100%;
        }

        .header h1 {
            margin: 0;
            font-size: 36px;
            font-weight: 600;
        }

        .add-button {
            padding: 12px 24px;
            background-color: #57A6E8;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 18px;
            cursor: pointer;
            text-decoration: none;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
            transition: background-color 0.3s ease, box-shadow 0.3s ease;
        }

        .add-button:hover {
            background-color: #4092c9;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }

        .employee-list {
            width: 100%;
            border-collapse: collapse;
            margin-top: 40px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
            border-radius: 12px;
            overflow: hidden;
        }

        .employee-list th, .employee-list td {
            padding: 15px 20px;
            text-align: center;
            border: 1px solid #E0E0E0;
            font-size: 14px;
        }

        .employee-list th {
            background-color: #4A90E2;
            color: white;
            font-size: 16px;
        }

        .employee-list td {
            background-color: #FFFFFF;
            font-size: 14px;
            color: #555;
        }

        .remove-button {
            padding: 8px 16px;
            background-color: #E74C3C;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
        }

        .remove-button:hover {
            background-color: #C0392B;
            transform: scale(1.05);
        }

        .status-clickable {
            color: #4A90E2;
            font-weight: bold;
            cursor: pointer;
        }

        .status-clickable:hover {
            text-decoration: underline;
        }

        .overlay, .modal {
            display: flex;
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
            padding: 40px;
            border-radius: 12px;
            width: 600px;
            max-width: 90%;
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
        }

        .modal-header {
            margin-bottom: 30px;
        }

        .modal-input {
            width: 100%;
            padding: 14px;
            margin: 12px 0;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            background-color: #FAFAFA;
        }

        .modal-button {
            padding: 14px 28px;
            background-color: #4A90E2;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 18px;
            width: 100%;
            transition: background-color 0.3s ease;
        }

        .modal-button:hover {
            background-color: #357ab7;
        }

        @media (max-width: 768px) {
            .employee-list th, .employee-list td {
                font-size: 12px;
                padding: 10px 15px;
            }

            .add-button {
                font-size: 16px;
                padding: 10px 20px;
            }

            .modal-content {
                width: 90%;
                padding: 20px;
            }
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
                        <span id="workStatus-${index}" class="status-clickable" onclick="openChangeStatusModal(${index})">${employee.works}</span>
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
