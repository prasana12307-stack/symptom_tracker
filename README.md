<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Symptom Tracker</title>

<style>
    body {
        font-family: Arial, sans-serif;
        background: #f4f6f9;
        margin: 0;
        padding: 20px;
    }

    .container {
        max-width: 800px;
        margin: auto;
        background: white;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }

    h1 {
        text-align: center;
        color: #2c3e50;
    }

    form {
        display: grid;
        gap: 12px;
    }

    input, textarea, select, button {
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 8px;
        font-size: 16px;
    }

    button {
        background: #3498db;
        color: white;
        border: none;
        cursor: pointer;
    }

    button:hover {
        background: #2980b9;
    }

    table {
        width: 100%;
        margin-top: 20px;
        border-collapse: collapse;
    }

    th, td {
        border: 1px solid #ddd;
        padding: 10px;
        text-align: left;
    }

    th {
        background: #3498db;
        color: white;
    }

    .delete-btn {
        background: #e74c3c;
        padding: 6px 10px;
    }

    .delete-btn:hover {
        background: #c0392b;
    }
</style>
</head>
<body>

<div class="container">
    <h1>🩺 Symptom Tracker</h1>

    <form id="symptomForm">
        <input type="date" id="date" required>

        <input
            type="text"
            id="symptom"
            placeholder="Enter symptom"
            required
        >

        <select id="severity">
            <option value="Mild">Mild</option>
            <option value="Moderate">Moderate</option>
            <option value="Severe">Severe</option>
        </select>

        <textarea
            id="notes"
            rows="3"
            placeholder="Additional notes..."
        ></textarea>

        <button type="submit">Add Entry</button>
    </form>

    <table>
        <thead>
            <tr>
                <th>Date</th>
                <th>Symptom</th>
                <th>Severity</th>
                <th>Notes</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody id="symptomTable"></tbody>
    </table>
</div>

<script>
const form = document.getElementById("symptomForm");
const table = document.getElementById("symptomTable");

let entries = JSON.parse(localStorage.getItem("symptomEntries")) || [];

function saveEntries() {
    localStorage.setItem(
        "symptomEntries",
        JSON.stringify(entries)
    );
}

function renderEntries() {
    table.innerHTML = "";

    entries.forEach((entry, index) => {
        const row = document.createElement("tr");

        row.innerHTML = `
            <td>${entry.date}</td>
            <td>${entry.symptom}</td>
            <td>${entry.severity}</td>
            <td>${entry.notes}</td>
            <td>
                <button
                    class="delete-btn"
                    onclick="deleteEntry(${index})"
                >
                    Delete
                </button>
            </td>
        `;

        table.appendChild(row);
    });
}

function deleteEntry(index) {
    entries.splice(index, 1);
    saveEntries();
    renderEntries();
}

form.addEventListener("submit", function(e) {
    e.preventDefault();

    const entry = {
        date: document.getElementById("date").value,
        symptom: document.getElementById("symptom").value,
        severity: document.getElementById("severity").value,
        notes: document.getElementById("notes").value
    };

    entries.push(entry);

    saveEntries();
    renderEntries();

    form.reset();
});

renderEntries();
</script>

</body>
</html>
