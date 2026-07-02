<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Intereses Automática</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 20px;
            background-color: #f4f7f6;
            color: #333;
        }
        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            padding: 25px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #2c3e50;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input, select, button {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 16px;
        }
        button {
            background-color: #27ae60;
            color: white;
            border: none;
            cursor: pointer;
            font-weight: bold;
            margin-top: 10px;
        }
        button:hover {
            background-color: #219653;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 25px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: right;
        }
        th {
            background-color: #2c3e50;
            color: white;
            text-align: center;
        }
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        .totals {
            font-weight: bold;
            background-color: #eaeded !important;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Sistema Automatizado de Intereses</h1>
    
    <div class="form-group">
        <label for="assetValue">Valor del Activo ($):</label>
        <input type="number" id="assetValue" placeholder="Ej. 10000" value="10000" min="1">
    </div>
    
    <div class="form-group">
        <label for="term">Plazo (Periodos/Meses):</label>
        <input type="number" id="term" placeholder="Ej. 12" value="12" min="1">
    </div>
    
    <div class="form-group">
        <label for="interestRate">Tasa de Interés por Periodo (%):</label>
        <input type="number" id="interestRate" step="0.01" placeholder="Ej. 2" value="2" min="0">
    </div>
    
    <div class="form-group">
        <label for="systemType">Sistema de Amortización:</label>
        <select id="systemType">
            <option value="french">Francés (Cuota Constante)</option>
            <option value="linear">Lineal / Alemán (Amortización Constante)</option>
            <option value="regressive">Regresivo / Americano (Fin del Plazo)</option>
        </select>
    </div>
    
    <button onclick="generateAmortizationTable()">Calcular Tabla de Amortización</button>

    <table id="amortizationTable" style="display:none;">
        <thead>
            <tr>
                <th>Periodo</th>
                <th>Cuota</th>
                <th>Interés</th>
                <th>Amortización</th>
                <th>Saldo Pendiente</th>
            </tr>
        </thead>
        <tbody id="tableBody">
            </tbody>
    </table>
</div>

<script>
function generateAmortizationTable() {
    const assetValue = parseFloat(document.getElementById('assetValue').value);
    const term = parseInt(document.getElementById('term').value);
    const interestRate = parseFloat(document.getElementById('interestRate').value) / 100;
    const systemType = document.getElementById('systemType').value;
    
    if (isNaN(assetValue) || isNaN(term) || isNaN(interestRate) || assetValue <= 0 || term <= 0) {
        alert("Por favor, ingresa valores válidos.");
        return;
    }

    const table = document.getElementById('amortizationTable');
    const tbody = document.getElementById('tableBody');
    tbody.innerHTML = ""; // Limpiar tabla anterior
    table.style.display = "table";

    let balance = assetValue;
    let totalInterests = 0;
    let totalPrincipal = 0;
    let totalPayment = 0;

    // Fila inicial (Periodo 0)
    let rowHtml = `<tr>
        <td style="text-align:center;">0</td>
        <td>-</td>
        <td>-</td>
        <td>-</td>
        <td>$${balance.toFixed(2)}</td>
    </tr>`;
    tbody.innerHTML += rowHtml;

    // Cálculos específicos por sistema
    let fixedPayment = 0;
    let fixedAmortization = 0;

    if (systemType === 'french') {
        // Cuota fija: R = P * [i * (1 + i)^n] / [(1 + i)^n - 1]
        if (interestRate === 0) {
            fixedPayment = assetValue / term;
        } else {
            fixedPayment = assetValue * (interestRate * Math.pow(1 + interestRate, term)) / (Math.pow(1 + interestRate, term) - 1);
        }
    } else if (systemType === 'linear') {
        // Amortización fija de capital
        fixedAmortization = assetValue / term;
    }

    for (let i = 1; i <= term; i++) {
        let interest = balance * interestRate;
        let amortization = 0;
        let payment = 0;

        if (systemType === 'french') {
            payment = fixedPayment;
            amortization = payment - interest;
        } else if (systemType === 'linear') {
            amortization = fixedAmortization;
            payment = amortization + interest;
        } else if (systemType === 'regressive') {
            // Regresivo/Americano: Solo paga intereses, el capital al final
            interest = assetValue * interestRate; 
            amortization = (i === term) ? assetValue : 0;
            payment = interest + amortization;
        }

        balance -= amortization;
        // Corrección por decimales flotantes en el último periodo
        if (i === term || balance < 0.01) {
            balance = 0;
        }

        totalInterests += interest;
        totalPrincipal += amortization;
        totalPayment += payment;

        rowHtml = `<tr>
            <td style="text-align:center;">${i}</td>
            <td>$${payment.toFixed(2)}</td>
            <td>$${interest.toFixed(2)}</td>
            <td>$${amortization.toFixed(2)}</td>
            <td>$${balance.toFixed(2)}</td>
        </tr>`;
        tbody.innerHTML += rowHtml;
    }

    // Fila de Totales
    let totalsHtml = `<tr class="totals">
        <td style="text-align:center;">TOTAL</td>
        <td>$${totalPayment.toFixed(2)}</td>
        <td>$${totalInterests.toFixed(2)}</td>
        <td>$${totalPrincipal.toFixed(2)}</td>
        <td>-</td>
    </tr>`;
    tbody.innerHTML += totalsHtml;
}
</script>

</body>
</html>