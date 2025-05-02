<!DOCTYPE html>
<html lang="fr">
<head>
  <!DOCTYPE html>
<html lang="fr">
<head><link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/handsontable@12.3.1/dist/handsontable.full.min.css">
  <script src="https://cdn.jsdelivr.net/npm/handsontable@12.3.1/dist/handsontable.full.min.js"></script><script>
    const container = document.getElementById('financeTable');
    const hot = new Handsontable(container, {
      data: [], // Les données initiales
      colHeaders: ['Type', 'Description', 'Montant (CFA)', 'Date'],
      columns: [
        { type: 'dropdown', source: ['Revenu', 'Dépense'] },
        { type: 'text' },
        { type: 'numeric', numericFormat: { pattern: '0,0 CFA' } },
        { type: 'date', dateFormat: 'DD/MM/YYYY', correctFormat: true }
      ],
      rowHeaders: true,
      filters: true,
      dropdownMenu: true,
      contextMenu: true,
      licenseKey: 'non-commercial-and-evaluation' // Licence gratuite pour usage non commercial
    });
  
    // Fonction pour exporter le tableau en CSV
    function exportTableToCSV() {
      const exportPlugin = hot.getPlugin('exportFile');
      exportPlugin.downloadFile('csv', {
        filename: 'tableau_gestion_financiere',
        columnHeaders: true
      });
    }
  
    // Ajouter un bouton pour exporter le tableau
    const exportButton = document.createElement('button');
    exportButton.textContent = 'Exporter le tableau en CSV';
    exportButton.style.marginTop = '10px';
    exportButton.style.backgroundColor = '#28a745';
    exportButton.style.color = '#fff';
    exportButton.style.border = 'none';
    exportButton.style.padding = '10px';
    exportButton.style.borderRadius = '4px';
    exportButton.style.cursor = 'pointer';
    exportButton.onclick = exportTableToCSV;
    container.parentNode.appendChild(exportButton);
  </script>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Aboubacar - Gestion Financière</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');

    body {
      font-family: 'Poppins', sans-serif;
      background-color: #f4f4f9;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      background: #007bff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      width: 100%;
      max-width: 600px;
      color: #fff;
    }

    .container h1 {
      text-align: center;
      margin-bottom: 20px;
    }

    .form-group {
      margin-bottom: 15px;
    }

    .form-group label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }

    .form-group input, .form-group select {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 16px;
    }

    .form-group input:focus, .form-group select:focus {
      border-color: #0056b3;
      outline: none;
    }

    .form-group button {
      width: 100%;
      padding: 10px;
      background-color: #0056b3;
      color: #fff;
      border: none;
      border-radius: 4px;
      font-size: 16px;
      cursor: pointer;
    }

    .form-group button:hover {
      opacity: 0.9;
    }

    .transactions {
      margin-top: 20px;
      background: #fff;
      color: #000;
      border-radius: 8px;
      padding: 10px;
    }

    .transactions table {
      width: 100%;
      border-collapse: collapse;
    }

    .transactions th, .transactions td {
      padding: 10px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }

    .transactions th {
      background-color: #007bff;
      color: #fff;
    }

    .balance {
      margin-top: 20px;
      font-size: 18px;
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Aboubacar - Gestion Financière</h1>
    <form id="financeForm">
      <div class="form-group">
        <label for="type">Type de transaction</label>
        <select id="type" name="type" required>
          <option value="revenue">Revenu</option>
          <option value="expense">Dépense</option>
        </select>
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" id="description" name="description" required>
      </div>
      <div class="form-group">
        <label for="amount">Montant (en Franc CFA)</label>
        <input type="number" id="amount" name="amount" required>
      </div>
      <div class="form-group">
        <label for="profilePhoto">Photo de profil</label>
        <input type="file" id="profilePhoto" name="profilePhoto" accept="image/*" onchange="previewProfilePhoto()">
        <div id="profilePhotoPreview" style="margin-top: 10px;">
          <!-- Aperçu de la photo de profil -->
        </div>
      </div>
      <div class="form-group">
        <button type="submit">payer</button>
      </div>
    </form>

    <!-- Boutons supplémentaires -->
    <div class="form-group">
      <button type="button" onclick="resetAll()" style="background-color: #dc3545;">Réinitialiser les données</button>
    </div>
    <div class="form-group">
      <button type="button" onclick="exportCSV()" style="background-color: #28a745;">Exporter en CSV</button>
    </div>
    <div class="form-group">
      <h2>Tableau de Gestion Financière</h2>
      <div id="financeTable" style="margin-top: 20px;"></div>
    </div>
    <div class="form-group">
      <button type="button" onclick="exportPDF()" style="background-color: #6f42c1;">Exporter en PDF</button>
    </div>

    <!-- Bouton pour accéder à la page de paramétrage -->
    <div class="form-group">
      <button type="button" onclick="openSettingsPage()" style="background-color: #ffc107;">Paramètres</button>
    </div>

    <div class="transactions">
      <h2>Historique des Transactions</h2>
      <table>
        <thead>
          <tr>
            <th>Type</th>
            <th>Description</th>
            <th>Montant</th>
          </tr>
        </thead>
        <tbody id="transactionTable">
          <!-- Transactions ici -->
        </tbody>
      </table>
    </div>
    <div class="balance" id="balance">
      Solde actuel : 0 Franc CFA
    </div>
  </div>

  <!-- Ajout d'une page de paramétrage -->
  <div class="container" id="settingsPage" style="display: none;">
    <h1>Paramètres</h1>
    <form id="settingsForm">
      <div class="form-group">
        <label for="currency">Devise</label>
        <select id="currency" name="currency" required>
          <option value="CFA">Franc CFA</option>
          <option value="USD">Dollar US</option>
          <option value="EUR">Euro</option>
        </select>
      </div>
      <div class="form-group">
        <label for="language">Langue</label>
        <select id="language" name="language" required>
          <option value="fr">Français</option>
          <option value="en">Anglais</option>
        </select>
      </div>
      <div class="form-group">
        <button type="submit" style="background-color: #007bff;">Enregistrer les paramètres</button>
      </div>
    </form>
    <div class="form-group">
      <button type="button" onclick="goBackToMainPage()" style="background-color: #6c757d;">Retour</button>
    </div>
  </div>

  <script>
    const form = document.getElementById('financeForm');
    const transactionTable = document.getElementById('transactionTable');
    const balanceElement = document.getElementById('balance');
    let transactions = JSON.parse(localStorage.getItem('transactions')) || [];

    const formatAmount = (value) => {
      return new Intl.NumberFormat('fr-FR').format(value) + ' Franc CFA';
    };

    function updateBalance() {
      const balance = transactions.reduce((acc, t) => acc + (t.type === 'revenue' ? t.amount : -t.amount), 0);
      balanceElement.textContent = `Solde actuel : ${formatAmount(balance)}`;
    }

    function saveTransactions() {
      localStorage.setItem('transactions', JSON.stringify(transactions));
    }

    function renderTransactions() {
      transactionTable.innerHTML = '';
      transactions.forEach((t, index) => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${t.type === 'revenue' ? 'Revenu' : 'Dépense'}</td>
          <td>${t.description} <br><small>${t.date}</small></td>
          <td>
            ${t.type === 'revenue' ? '+' : '-'}${formatAmount(t.amount)}
            <button onclick="deleteTransaction(${index})" style="margin-left:10px;color:red;">Supprimer</button>
          </td>
        `;
        transactionTable.appendChild(row);
      });
    }

    function deleteTransaction(index) {
      transactions.splice(index, 1);
      saveTransactions();
      renderTransactions();
      updateBalance();
    }

    function resetAll() {
      const confirmReset = confirm("Voulez-vous vraiment tout réinitialiser ? Cette action est irréversible.");
      if (confirmReset) {
        transactions = [];
        localStorage.removeItem('transactions');
        renderTransactions();
        updateBalance();
      }
    }

    function exportCSV() {
      if (transactions.length === 0) {
        alert("Aucune transaction à exporter.");
        return;
      }

      let csvContent = "Type,Description,Montant,Date\n";
      transactions.forEach(t => {
        const type = t.type === 'revenue' ? 'Revenu' : 'Dépense';
        const amount = t.type === 'revenue' ? t.amount : -t.amount;
        csvContent += `${type},"${t.description.replace(/"/g, '""')}",${amount},"${t.date}"\n`;
      });

      const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.setAttribute("href", url);
      link.setAttribute("download", "transactions.csv");
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }

    async function exportPDF() {
      if (transactions.length === 0) {
        alert("Aucune transaction à exporter.");
        return;
      }

      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();

      doc.setFontSize(16);
      doc.text("Historique des Transactions", 14, 20);

      const headers = [["Type", "Description", "Montant", "Date"]];
      const rows = transactions.map(t => [
        t.type === 'revenue' ? "Revenu" : "Dépense",
        t.description,
        (t.type === 'revenue' ? "+" : "-") + t.amount + " CFA",
        t.date
      ]);

      if (doc.autoTable) {
        doc.autoTable({
          startY: 30,
          head: headers,
          body: rows
        });
      } else {
        let y = 30;
        headers.concat(rows).forEach(row => {
          doc.text(row.join(" | "), 14, y);
          y += 10;
        });
      }

      doc.save("transactions.pdf");
    }

    const settingsPage = document.getElementById('settingsPage');
    const mainContainer = document.querySelector('.container');

    function openSettingsPage() {
      mainContainer.style.display = 'none';
      settingsPage.style.display = 'block';
    }

    function goBackToMainPage() {
      settingsPage.style.display = 'none';
      mainContainer.style.display = 'block';
    }

    document.getElementById('settingsForm').addEventListener('submit', function(event) {
      event.preventDefault();

      const currency = document.getElementById('currency').value;
      const language = document.getElementById('language').value;

      localStorage.setItem('currency', currency);
      localStorage.setItem('language', language);

      alert('Paramètres enregistrés avec succès !');
      goBackToMainPage();
    });

    // Charger les paramètres enregistrés
    window.addEventListener('load', function() {
      const savedCurrency = localStorage.getItem('currency');
      const savedLanguage = localStorage.getItem('language');

      if (savedCurrency) {
        document.getElementById('currency').value = savedCurrency;
      }

      if (savedLanguage) {
        document.getElementById('language').value = savedLanguage;
      }
    });

    form.addEventListener('submit', function(event) {
      event.preventDefault();

      const type = document.getElementById('type').value;
      const description = document.getElementById('description').value;
      const amount = parseFloat(document.getElementById('amount').value);

      if (amount <= 0) {
        alert('Le montant doit être supérieur à 0.');
        return;
      }

      const newTransaction = {
        type,
        description,
        amount,
        date: new Date().toLocaleString('fr-FR')
      };

      transactions.push(newTransaction);
      saveTransactions();
      renderTransactions();
      updateBalance();
      form.reset();
    });

    function previewProfilePhoto() {
      const fileInput = document.getElementById('profilePhoto');
      const previewContainer = document.getElementById('profilePhotoPreview');
      previewContainer.innerHTML = ''; // Réinitialiser l'aperçu

      if (fileInput.files && fileInput.files[0]) {
        const reader = new FileReader();

        reader.onload = function(e) {
          const img = document.createElement('img');
          img.src = e.target.result;
          img.alt = 'Aperçu de la photo de profil';
          img.style.maxWidth = '100px';
          img.style.borderRadius = '50%';
          previewContainer.appendChild(img);
        };

        reader.readAsDataURL(fileInput.files[0]);
      }
    }

    // Initialisation
    renderTransactions();
    updateBalance();
  </script>
</body>
</html>
``` 
