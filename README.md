# Department of Justice - Kartoteka pracowników

<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spis Pracowników</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f8ff;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            padding: 20px;
            background-color: #007acc;
            color: white;
            border-radius: 10px;
        }

        .header h1 {
            margin: 0;
            font-size: 36px;
        }

        .add-button {
            padding: 10px 20px;
            background-color: #007acc;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 18px;
            cursor: pointer;
            text-decoration: none;
        }

        .add-button:hover {
            background-color: #005fa3;
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
            background-color: #007acc;
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
        }

        .remove-button:hover {
            background-color: #c0392b;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Spis Pracowników</h1>
            <a href="#" class="add-button">Dodaj Pracownika</a>
        </div>

        <table class="employee-list">
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
                <tr>
                    <td>Jan Kowalski</td>
                    <td>123-45-6789</td>
                    <td>jan.kowalski#1234</td>
                    <td>Tak</td>
                    <td><button class="remove-button">Usuń</button></td>
                </tr>
                <tr>
                    <td>Agnieszka Nowak</td>
                    <td>987-65-4321</td>
                    <td>agnieszka.nowak#5678</td>
                    <td>Nie</td>
                    <td><button class="remove-button">Usuń</button></td>
                </tr>
                <tr>
                    <td>Piotr Wiśniewski</td>
                    <td>456-78-1234</td>
                    <td>piotr.wisniewski#91011</td>
                    <td>Tak</td>
                    <td><button class="remove-button">Usuń</button></td>
                </tr>
                <!-- Można dodać więcej pracowników tutaj -->
            </tbody>
        </table>
    </div>
</body>
</html>
